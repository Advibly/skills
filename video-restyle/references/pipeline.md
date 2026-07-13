# Pipeline reference: recipes, limits, billing, triage

Everything operational. Read this before debugging a weird render, a failed
upload, or a duration mismatch.

## The moving parts

| Step | Tool | Cost |
|------|------|------|
| Analyze | `advibly_analyze_video` | free |
| Upload proxy/segments/final | `advibly_upload_asset` | free |
| Restyle a segment | `advibly_generate_video` (gemini-omni-flash, `source_video_url`) | ~0.4 credits/second of segment |
| Final composition + original audio | `advibly_render_composition` | free |
| Post captions (optional) | `advibly_add_subtitles` | 0.2-0.4 credits/min, 1-min minimum |

## Billing detail that matters

A video-to-video edit bills on the `duration` you pass, clamped to 4-10s,
defaulting to the 10s MAXIMUM when omitted. In v2v the model ignores duration
for the actual output (it keeps the source length), but the passed value still
drives billing, so always pass
`duration: <segment length rounded up to a whole second>`. Omitting it on a
5s segment doubles its price. Rounding up is a billing artifact only: the
output length is fixed by the local conform step (Phase 5), not by this value.

Packing to 8-10s segments does not change total credits (you still pay for the
same total seconds), but it means fewer calls, fewer seams, and fewer re-rolls.

Ballpark quote for the user before Phase 4:
`total credits ≈ 0.4 x video seconds`, plus 20-30% for re-rolls.

## Omni Flash v2v envelope

- Source: 4-10s per clip. Nothing longer; nothing shorter than 4s (the model
  rejects sub-4s sources; merge sub-4s orphans into a neighbor at planning
  time). **Pack toward 8-10s**: join
  adjacent sentences so each segment holds two or three phrases, which both
  reduces seams and gives the beat timeline room to move. Drop below 8s only
  when a boundary forces it.
- Output: aspect inherits from the source (any aspect works in edit mode,
  including square-ish sources); `aspect_ratio` and out-of-range `duration` are
  ignored/clamped. Output length *usually* matches the source but can drift a
  fraction of a second under heavy motion, so it is conformed locally after
  every call (see below). Never assume the raw output length is exact.
- No end frame, no mode tiers, output is 720p-class.
- Content filter blocks recognizable public figures and third-party
  logos/marks. `advibly_analyze_video` flags these in `notes`; there is no
  alternative v2v model, so surface it before spending.
- **Positive-only phrasing.** Omni ignores or inverts negations. Convert
  every "no X / don't Y" into what should happen instead: "the lettering
  stays exactly as printed", "the camera stays locked", "the face stays
  clearly lit".

## Upload limits

`advibly_upload_asset`: videos up to 50 MB, images up to 8 MB. `data_base64`
for local files, `source_url` for anything already hosted. Base64 inflates
size by ~33%, so keep local video payloads well under the cap; the cut
recipe below produces segments of a few MB. Response includes a public `url`
to pass as `source_video_url`.

Analysis input is tighter: `advibly_analyze_video` fetches at most ~18 MB
(Gemini inline limit). Bigger sources analyze via a proxy (recipe below);
the cut plan applies to the original.

## Source preparation, conform, and final composition

Use a local media cutter for three things: an analysis proxy, frame-accurate
source segments, and the post-restyle conform pass.

**Conform (Phase 5, after each v2v call):** ffprobe the restyled clip and
compare to that segment's recorded source duration. If they differ, trim a long
clip or pad a short one (hold last frame) to the exact source length, video
only:

```bash
OUT_DUR=$(ffprobe -v error -show_entries format=duration -of csv=p=0 restyled-seg-01.mp4)
# when OUT_DUR != SRC_DUR:
ffmpeg -y -i restyled-seg-01.mp4 -an \
  -vf "tpad=stop_mode=clone:stop_duration=3,trim=duration=<SRC_DUR>,setpts=PTS-STARTPTS" \
  -c:v libx264 -crf 18 -preset veryfast -pix_fmt yuv420p conformed-seg-01.mp4
```

The summed conformed durations must equal the whole-source ffprobe duration.
That equality is the entire lip-sync guarantee.

**Composition:** call `advibly_render_composition` once with `brand_id`
(required), the CONFORMED segments as ordered `scenes` (max 12 per
composition),
`voiceovers: [{source: <ORIGINAL video generation id, OR the uploaded source-video HTTPS url>}]`,
the source aspect ratio, and `keep_scene_audio: false`. Use the generation id
when the source was an Advibly generation; use the uploaded source video's URL
when it came in as a local file or asset (the common case) so the original
audio track is pulled from it. The original audio becomes the continuous
soundtrack over the restyled hard-cut visuals, and because the visuals were
conformed it stays in lip-sync.

The call returns `status: pending`, `generation_id`, and `edit_url`. Wait with
`advibly_get_generation` only when post captions or publishing needs the finished URL, and mention
the edit URL in final delivery.

## Analysis output, field by field

`advibly_analyze_video` returns `{ analysis: { ... } }`:

- `summary`: description, subject, setting, camera, speech_style. Use it to
  sanity-check template fit (a silent b-roll montage is a poor podcast-pop
  candidate).
- `language`, `duration_seconds`: the model's estimate; trust ffprobe over
  it.
- `transcript[]`: `{start, end, text}` per sentence. The captions and the
  segment plan derive from this.
- `segments[]`: the draft cut plan: `{index, start, end, transcript, visual,
  emphasis, topics[], caption}`. The analyzer tends to return one short segment
  per sentence; **pack them** into 8-10s segments (merge adjacent sentences)
  before doing anything else, then reconcile ends against the ffprobe duration
  (scale proportionally when the totals disagree, then snap to the nearest
  transcript boundary). Keep each segment's `emphasis` word: it decides which
  beat gets the biggest event.
- `notes`: watermarks, baked captions, logos, recognizable people, scene
  changes. Read it every time; it is the content-filter early warning.

## Failure triage

| Symptom | Cause | Fix |
|---------|-------|-----|
| `content_rejected` on one segment | public figure / third-party mark / sensitive content in that segment | crop or blur the element at the cut step if peripheral; otherwise the video cannot run v2v |
| `too_large` from analyze | source over ~18 MB | analysis proxy recipe |
| Upload fails on a segment | base64 payload too big | raise CRF to 23-26 or scale to 960 wide for that segment |
| Face drifts / new person | style vocab crowded the identity block | shorten the beat timeline, keep identity lines intact, re-roll |
| Caption misspelled | model spelling variance | re-roll same prompt; then shorten caption to 3-4 words |
| Style barely applied | style block diluted or trimmed | restore the template's block verbatim; re-roll before rewriting |
| Output looks static | beat timeline missing, trimmed, or written as one look | rewrite the segment with 2-3 word-anchored beats; pack the segment to 8-10s so there are words to anchor to |
| Lip-sync drifts in the final | a segment was not conformed to its source length | ffprobe each conformed segment; re-conform (trim/pad) the one that disagrees; verify summed lengths equal the source; re-render |
| Final ships silent | `voiceovers` empty or wrong source with `keep_scene_audio:false` | pass the original source video's generation id or uploaded URL as the voiceover source |
| Segment came back a different length | expected drift, or wrong cut boundary | conform it to the source length; if the boundary itself was wrong, re-cut and re-run |
| `insufficient_credits` | balance too low | `advibly_buy_credits`, share the checkout link |

## Order of operations (compressed)

probe → (proxy) → upload → analyze → pack to 8-10s → reconcile plan (record
each source duration) → APPROVAL → cut → verify cuts → upload segments →
v2v per segment (beat timeline; real `duration`!) → review each (identity,
lip-sync, caption, style+motion) → conform each to source length → verify
summed lengths equal source → render_composition (conformed scenes + original
audio) → deliver with edit_url.
