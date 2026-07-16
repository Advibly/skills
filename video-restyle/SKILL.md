---
name: video-restyle
description: >
  Restyle an existing talking-head or UGC video with Gemini Omni Flash video-to-video on the
  Advibly MCP while preserving identity, expressions, lip-sync, and original audio. Analyze and
  segment the source, apply beat-timed prompts using podcast-pop, watercolor, anime-manga,
  newspaper, notebook-doodle, neon-vaporwave, comic-book, vox-style, psychedelic-swirl,
  flat-illustration, or cardboard-cutout templates, conform every segment to its source
  length, then reassemble with the original audio. Trigger for "restyle a video", "apply a style
  to this video", "make my talking head look like X", "video style transfer", "turn my video
  into a cartoon, watercolor, anime, comic, psychedelic, illustration, or cardboard look",
  "podcast-style edit", "vox-style edit", or a source clip plus a look reference. Use when the
  requested transformation must retain the person and speech.
---

# Advibly Video Restyle

Take a finished video (usually a talking-head or UGC clip) and re-render its
entire look while keeping the person, their performance, their lip-sync, and
their exact original audio. One template = one visual world; the video is
processed segment by segment and reassembled so the result reads as a
deliberately edited, multi-look cut rather than one long static shot.

Everything generates and assembles on the Advibly MCP. Use a local video cutter only to prepare
the source segments when necessary.

Speak in the user's language. No em dashes anywhere in output; use periods or
line breaks. Keep on-screen copy free of emoji unless asked.

## The core idea (read this first)

1. **Omni Flash video-to-video is the engine.** `advibly_generate_video` with
   `model: "gemini-omni-flash"` and `source_video_url` transforms an existing
   clip: it repaints the frame while following the source's motion, so faces,
   gestures, and mouth movements carry over. The output *should* inherit the
   source segment's length, but heavy motion can make Omni retime a clip by a
   fraction of a second, so every restyled segment is conformed back to its
   exact source length (Phase 5) before assembly. The hard limit: a source
   clip can be at most **10 seconds**. That limit is why the pipeline segments.
2. **Segmentation is a feature, not a workaround.** Each segment gets its own
   variation of the template (a palette swap, a new background motif, a
   punch-in), so the assembled result changes visuals every few seconds, which
   is exactly what retention editing wants. The cut plan comes from
   `advibly_analyze_video`. **Pack segments to 8-10s** (each holds two or three
   spoken phrases, which gives the beat timeline room to move); only drop to
   4-7s when a single sentence genuinely cannot be joined to a neighbor without
   breaking a thought. Cuts always land on sentence boundaries so no word is
   split across a seam. Omni v2v accepts 4-10s sources only, so 4s is the hard
   floor.
3. **Motion is anchored to spoken words.** This is what keeps the output from
   going static. A segment is not one look held for 8 seconds. Each segment
   prompt is a **beat-for-beat timeline** over that segment's own transcript:
   every visual event (a background swap, a punch-in, a typography word landing,
   an impact frame) is tied to a specific spoken word with the construction
   "Exactly on the word X, [event]". A packed 8-10s segment carries two or
   three such beats, so the frame is always moving and always landing events on
   the words that matter.
4. **The template holds the world together.** A template file in `references/`
   defines the STYLE BLOCK (travels verbatim in every segment prompt) and the
   VARIATION AXES (what changes per segment). Style block verbatim + axes
   rotating + word-anchored beats is what makes the segments feel like one
   edited video instead of a slideshow of unrelated clips.
5. **The original audio is the ground truth.** Whatever Omni does to a
   segment's audio, the final composition uses the ORIGINAL video as its audio
   source over the ordered restyled scenes. This only stays in lip-sync because
   every restyled segment is conformed to its source segment's exact length, so
   the sum of the restyled visuals equals the original runtime frame for frame.
   Never ship Omni's resynthesized audio when the original is available, and
   never let a segment's output length drift from its source.

## Hard defaults (do not drift)

- **Video model:** `advibly_generate_video` with `model: "gemini-omni-flash"`
  and `source_video_url`. It is the ONLY model with video-to-video. Aspect
  ratio is inherited from the source segment. Pass `duration` = the segment's
  real length rounded up to a whole second (the API clamps to whole seconds
  4-10 and bills on what you send, defaulting to the 10s maximum when omitted;
  in v2v the model ignores duration for output length and keeps the source
  length, but the passed value still drives billing, so always send the real
  length).
  Rounding up is a BILLING artifact only: it never defines the output length,
  because Phase 5 conforms every restyled segment back to its exact source
  duration. Roughly 0.4 credits per second, so a 60s video runs about 24
  credits plus re-rolls.
- **Segments pack to 8-10s, cut at sentence boundaries.** Prefer 10s, accept
  8-10s (`max_segment_seconds: 10`); join adjacent sentences to fill a segment
  rather than shipping many short ones. Drop below 8s only when a sentence
  cannot be joined to a neighbor without splitting a thought, and never below
  4s (Omni v2v rejects sub-4s sources; merge sub-4s orphans into a neighbor).
  Segments are contiguous and cover the whole video with no gaps or overlaps.
  Assembly caps at 12 scenes per composition, and packing to 8-10s keeps a
  source up to ~2 minutes within that cap; a longer source needs more than one
  composition.
- **Every restyled segment is conformed to its source length.** After each v2v
  call, ffprobe the output; if it differs from that segment's source duration,
  trim or pad it to match to the frame before assembly (Phase 5). The sum of
  conformed segment lengths must equal the ffprobe duration of the whole
  source, or the original audio drifts against the visuals and lip-sync
  breaks. This is the fix for out-of-sync output.
- **Each segment prompt is a beat-for-beat timeline**, not a single static
  look. Anchor two or three visual events to exact spoken words per packed
  segment (see Phase 5). A whole-segment single-look prompt is the cause of
  static output and is not acceptable.
- **One template per video.** Never mix templates across segments. Variation
  comes from the template's own axes.
- **Identity preservation block is mandatory** in every segment prompt (see
  below), before the style block. This is what keeps the person recognizable
  and the lip-sync intact.
- **Positive-only phrasing.** Omni ignores or inverts negations. "The
  lettering stays exactly as printed", never "don't change the text".
- **Captions are baked by the model** as part of the style (the default).
  Every caption line in a prompt carries the exact text in quotes plus
  "spelled exactly, every word correct". Keep captions 3-6 UPPERCASE words.
  Verify spelling on every delivered clip; a misspelled caption is a re-roll,
  not a keep. If the user prefers clean footage plus post captions, drop the
  caption lines from all prompts and offer `advibly_add_subtitles` on the
  final cut instead.
- **Real people are fine; celebrities and third-party logos are not.** Omni
  blocks recognizable public figures and brand marks at the content-filter
  level. `advibly_analyze_video` reports both under `notes`; warn the user
  before spending if either shows up. There is no fallback model for
  video-to-video.
- **Prompts cap at 2000 characters.** Identity block + style block + beat
  timeline must fit. Trim style vocabulary, never the identity block or the
  word anchors.
- **Tools are deferred.** Load the Advibly tool schemas with tool search
  before the first call each session (search "advibly analyze video",
  "advibly generate video", "advibly upload asset", "advibly render composition",
  "advibly add subtitles", "advibly create project", "advibly update
  project"). Confirm parameter names against what loads.

## The identity preservation block (memorize)

This exact block opens every segment prompt, before the template's style
block:

```
Transform this clip into a new visual style. The person stays exactly who
they are: same face, same identity, same expressions, same mouth movements
and lip-sync, same hairstyle, same clothing, same hand gestures, matching
the source frame for frame. Their spoken performance continues unchanged.
Foreground props they hold or touch (microphone, laptop, product) stay
present and anchored.
```

Templates that repaint the subject (watercolor, anime) adapt the wording (the
face becomes a painted/drawn version OF THE SAME face) - each template file
says how.

---

## PHASE 1: INTAKE

One message, only what you still need:

1. **The source video**: a local file path, a URL, or something already in
   Advibly (a generation or an uploaded asset; find it with
   `advibly_list_generations` / `advibly_get_assets`). Best sources are a
   single continuous talking-head or UGC shot with clear speech.
2. **Brand**: `advibly_list_brands`. One brand: use it. Several: ask. The
   brand files the work; the template's look is not recolored to the brand.
   Then create the run's project with `advibly_create_project` (`brand_id`
   plus a deliverable-shaped name like "Acme podcast-pop restyle") and pass
   the returned `project_id` on every v2v call and the final composition so
   the segments land as one tile in the library. If the user is continuing
   an earlier run, find its project with `advibly_list_projects` instead of
   creating a duplicate.
3. **Template**: show the eleven and let the user pick (details in
   `references/`):
   - **podcast-pop**: the viral podcast-clip edit. Subject cut out as a
     sticker, bold rotating backgrounds (kinetic typography, crumpled paper,
     retro OS, halftone pop), punch-ins, torn caption bands.
   - **watercolor-wash**: the whole frame becomes a living watercolor
     painting, soft pigment blooms, paper grain.
   - **anime-manga**: cel-shaded anime repaint, speed lines, screentones,
     impact frames.
   - **newspaper-print**: black-and-white halftone newsprint world, headline
     columns, red marker accents.
   - **notebook-doodle**: subject on ruled notebook paper, hand-drawn ink
     doodles, highlighter marks, sticky notes.
   - **neon-vaporwave**: synthwave gradients, neon rim glow, chrome text,
     retro grid horizon.
   - **comic-book**: pop-art comic repaint, bold ink outlines, Ben-Day
     halftone dots, starburst backdrops, onomatopoeia bursts, caption boxes.
   - **vox-style**: subject cut out as a sticker over a bright yellow field
     of black and off-white Bauhaus geometry, editorial explainer energy.
   - **psychedelic-swirl**: posterized repaint over liquid marbled rainbow
     swirls, color inversion flashes, groovy motifs.
   - **flat-illustration**: clean flat vector-style repaint in an idyllic
     illustrated scene, soft skies, rolling hills, calm brand-explainer warmth.
   - **cardboard-cutout**: photoreal subject torn out with a rough cardboard
     edge on corrugated kraft scenes, packing tape, marker doodles, stamps.
   If the user shows a reference clip instead, match it to the closest
   template and adapt its axes; if nothing fits, compose a custom template
   with the same structure (subject treatment, style block, axes) and confirm
   it with the user.
4. **Captions**: baked captions on (default) or clean footage.

## PHASE 2: PROBE + ANALYZE

1. **Probe locally.** `ffprobe -v error -show_entries format=duration -of
   csv=p=0 src.mp4` plus width/height/fps. Note the aspect; the output keeps
   it, whatever it is.
2. **Get the video to a URL the analyzer can read.** Already in Advibly or
   public: use that URL if the file is under ~18 MB. Local or oversized: make
   a small analysis proxy and upload it (the cut plan applies to the
   original):
   ```bash
   ffmpeg -y -i src.mp4 -vf "scale=-2:480" -c:v libx264 -crf 30 -preset veryfast \
     -c:a aac -b:a 64k -ac 1 proxy.mp4
   ```
   Upload with `advibly_upload_asset` (`data_base64`, `mime_type: video/mp4`,
   `brand_id`).
3. **Analyze.** `advibly_analyze_video` with the URL,
   `max_segment_seconds: 10`, and `focus` describing the chosen template ("this
   will be restyled as <template>; flag moments that deserve emphasis").
   Returns the summary, timestamped transcript, cut plan, and notes.
4. **Reconcile the plan with reality.** Gemini's timestamps are estimates.
   First **pack**: merge adjacent analyzer segments so each packed segment runs
   8-10s wherever consecutive sentences allow it (the analyzer tends to return
   one short segment per sentence; you want two or three sentences per segment).
   Only leave a segment at 4-7s when the remaining tail cannot join a neighbor
   without exceeding 10s or splitting a thought. Then scale or nudge so the last
   segment ends exactly at the ffprobe duration, keep every segment inside
   4-10s, and merge any sub-4s orphan into its neighbor. Record each segment's
   exact source duration now: it is the conform target in Phase 5. If `notes`
   reports celebrities, third-party logos, or baked-in captions/watermarks in
   the source, tell the user now.

## PHASE 3: SEGMENT PLAN (the one mandatory approval gate)

Read the chosen template file in `references/` fully. Then assign each
segment its variation from the template's axes AND its beat list, and show the
plan for approval before cutting or spending:

| # | in-out | dur | spoken line | caption | look variation | beats (word → event) |
|---|--------|-----|-------------|---------|----------------|----------------------|
| 1 | 0.0-9.4 | 9.4s | "Most videos lose you in five seconds. That is the whole game." | MOST VIDEOS LOSE YOU | lime/hot-pink kinetic type | "Most" → type floods in; "lose" → punch-in tight; "whole game" → hard-cut to hot-pink layout, caption locks |

Rules for a good plan:

- **Pack to 8-10s first.** Each row should hold two or three spoken phrases,
  not one. Short single-sentence rows are only for a tail that cannot join a
  neighbor. This is what gives the beat column something to work with.
- **Every segment has a beat list.** Two or three events per packed segment,
  each tied to an exact word from that segment's spoken line: a background
  swap, a punch-in, a typography word landing, an impact frame, a caption
  reveal. The first beat is usually the segment's opening look establishing on
  the first word; the last beat lands on the segment's strongest word. Use the
  analyzer's `emphasis` field to pick which word gets the biggest event.
- **Rotate the axes.** No two adjacent segments share the same palette or
  background motif. The template file says what rotates and in what order. A
  within-segment background swap (a mid-segment beat) uses the NEXT palette in
  the rotation so it still reads as one designed sequence.
- **Spend the punch where the speech earns it.** Punch-ins, impact frames,
  and burst moments go on the words whose `emphasis` has something real, not on
  every word.
- **Captions are distilled, not transcribed.** 3-6 UPPERCASE words from the
  segment's strongest line (the analyzer's `caption` is the starting point;
  tighten it). Templates with kinetic typography reveal the caption word by
  word on its spoken beats rather than showing it all at once.
- **Sticker/topic graphics** (templates that use them) come from the
  segment's `topics` nouns and pop in on the word that names them.

Ask: "Approve this plan, or edit any row?" Only proceed on a yes.

## PHASE 4: CUT + UPLOAD SEGMENTS

Cut the ORIGINAL (not the proxy) with a re-encode so boundaries are
frame-accurate. Downscale huge sources; Omni outputs 720p-class video anyway
and smaller segments upload faster:

```bash
# per segment: START and DUR from the approved plan
ffmpeg -y -ss <START> -i src.mp4 -t <DUR> \
  -vf "scale='min(1280,iw)':-2" \
  -c:v libx264 -crf 18 -preset veryfast -pix_fmt yuv420p \
  -c:a aac -b:a 128k seg-01.mp4
```

Keep the audio in the segments: it is the model's lip-sync reference. Verify
each segment's duration with ffprobe (must be 4-10s), then upload each with
`advibly_upload_asset` (`data_base64`, `brand_id`, name them `restyle-seg-01`
and so on). Segments at these settings run a few MB each; if one exceeds
~35 MB, raise the CRF for that segment.

## PHASE 5: RESTYLE (one v2v call per segment)

For each segment, compose the prompt in this order:

1. The identity preservation block (verbatim, or the template's repaint
   variant).
2. The template's STYLE BLOCK (verbatim across all segments).
3. **The per-segment BEAT TIMELINE** (this is what replaces the old
   single-look block and kills static output). Structure it exactly like this,
   with the segment's own transcript split into its phrases and a visual event
   anchored to specific words:

   ```
   The segment runs this beat timeline over the spoken words:
   - It opens as <this segment's opening look from the template axes,
     palette named>, establishing on the first word "<first word>".
   - Exactly on the word "<mid word>", <a mid-segment event: a punch-in, a
     background swap to the next palette, a typography word landing, an
     impact frame, a sticker popping in>.
   - Exactly on the word "<strongest/emphasis word>", <the biggest event of
     the segment>, and the caption band reads exactly "<CAPTION>", spelled
     exactly, every word correct.
   ```

   Rules inside the timeline: quote the segment's words verbatim; every beat
   states what IS on screen (positive phrasing); a packed 8-10s segment gets
   two or three beats; the emphasis word gets the largest event; the final
   beat describes how the segment ends (a lock, a hold, a settle) so cuts
   between segments read cleanly. The caption on kinetic-typography templates
   reveals word by word on its spoken beats.
4. The audio line: "The person's voice and the room's sound continue
   naturally." (The original audio returns in Phase 6 regardless.)

```
advibly_generate_video
  prompt: <identity block + style block + beat timeline + audio line, under 2000 chars>
  brand_id: <brand id>
  project_id: <project id>
  model: "gemini-omni-flash"
  source_video_url: <that segment's uploaded URL>
  duration: <the segment's real length, rounded UP to a whole second — billing only>
```

Fire the calls in parallel if the client allows it; order does not matter
until assembly. Then review every clip against four checks:

- **Identity**: still recognizably the same person, same expressions.
- **Lip-sync**: mouth movement matches the spoken words.
- **Caption**: exact spelling, correct words, legible.
- **Style + motion**: the template landed AND the frame actually moves on the
  beats (compare against the style block and the beat timeline; a static or
  weak render means the style/beat lines got diluted, so re-roll before
  rewriting).

Re-roll misses with the same prompt first (v2v has healthy variance), then
with an adjusted beat timeline. Never accept a misspelled caption or a broken
face.

**Then conform every accepted clip to its source length (mandatory).** Omni's
output length can drift a fraction of a second, especially on segments with
punch-ins or impact frames. ffprobe each restyled clip and compare to that
segment's recorded source duration:

```bash
# SRC_DUR = the exact source-segment duration recorded in Phase 2
OUT_DUR=$(ffprobe -v error -show_entries format=duration -of csv=p=0 restyled-seg-01.mp4)
# if OUT_DUR != SRC_DUR, conform video-only to the exact length (audio is dropped at assembly):
ffmpeg -y -i restyled-seg-01.mp4 -an \
  -vf "tpad=stop_mode=clone:stop_duration=3,trim=duration=<SRC_DUR>,setpts=PTS-STARTPTS" \
  -c:v libx264 -crf 18 -preset veryfast -pix_fmt yuv420p conformed-seg-01.mp4
```

(The `tpad` pads a slightly short clip by holding its last frame; `trim` cuts
a slightly long one. Upload the conformed file for assembly.) Verify the sum
of conformed durations equals the ffprobe duration of the whole source before
Phase 6 — that equality is what keeps the original audio in lip-sync.

## PHASE 6: ASSEMBLE + AUDIO LOCK

Assemble from the CONFORMED segments (Phase 5), never the raw Omni outputs.
Call `advibly_render_composition` once. Pass the conformed restyled segment
HTTPS URLs (or their generation ids) in plan order as `scenes`, pass the
ORIGINAL audio as the voiceover, and set `keep_scene_audio: false`. Use the
source aspect ratio. The restyled visuals play as hard cuts while the original
video's audio runs from the start across the full composition:

```
advibly_render_composition
  brand_id: <brand id>                                            # required
  project_id: <the run's project id>
  scenes: [<conformed seg-01 url>, <conformed seg-02 url>, ...]   # plan order, max 12
  voiceovers: [{ source: <ORIGINAL video generation id, OR the uploaded source-video URL> }]
  keep_scene_audio: false
  aspect_ratio: <source aspect>
```

**Audio source:** if the source came in as an Advibly video generation, pass
its generation id. If it came in as a local file or uploaded asset (the common
case), pass the uploaded source video's HTTPS `url` so the composition pulls
the original audio track from it. Do not leave `voiceovers` empty with
`keep_scene_audio: false` or the render ships silent.

Because every segment was conformed to its source length, the summed visuals
equal the original runtime and the audio stays in lip-sync end to end. If
lip-sync still drifts, a segment was NOT conformed correctly: ffprobe each
conformed segment, find the one whose length disagrees with its Phase 2 source
duration, re-conform (or re-cut and re-run) that one, then render again. Never
time-stretch the audio to hide drift.

The call returns `status: pending`, a `generation_id`, and an `edit_url`; the chat widget polls
the render. Call `advibly_get_generation` with `wait: true` only when a finished URL is needed for
post captions or publishing. Deliver the result and mention the `edit_url` so the user can
fine-tune it in the Advibly video editor. Then set the finished cut as the project cover with
`advibly_update_project` (`project_id` plus `cover_generation_id: <the composition's
generation id>`).

## PHASE 7: OPTIONAL EXTRAS

- **Post captions** (only if baked captions were off):
  `advibly_add_subtitles` on the final, dynamic preset, brand words in
  `vocabulary`.
- **Publish**: `advibly_social_list_accounts`, then
  `advibly_social_create_post`. Only offer after the user has seen the final.

---

## Notes and rules

- **The segment plan is approved before any cutting or spending.**
- **Pack to 8-10s.** Prefer full 8-10s segments (two or three phrases each);
  go shorter only when a boundary forces it. Fewer, richer segments beat many
  thin ones.
- **Style block verbatim, axes rotating, word-anchored beats, one template per
  video.** That is the whole cohesion-plus-motion mechanism. A single static
  look per segment is the cause of dead output.
- **Every segment prompt is a beat timeline.** Two or three events tied to
  exact spoken words. If the output is static, the beats got trimmed or were
  never written.
- **Conform before assembly.** ffprobe every restyled segment; trim/pad to its
  exact source length; the summed lengths must equal the source runtime. This
  is the fix for out-of-sync lip-sync. Assemble conformed segments only.
- **Identity block in every prompt.** The most common failure is a prompt
  where style vocabulary crowded out the identity lines and the face drifted.
- **Pass the real `duration` on every v2v call** or the edit bills at the
  10s maximum. Rounding up is billing only; the conform step defines length.
- **Analysis timestamps are estimates**; the ffprobe duration is the truth.
  Reconcile before cutting.
- **On `content_rejected`**: the segment shows a recognizable public figure,
  a third-party mark, or policy-sensitive content. Crop/blur the offending
  element in the cut step if peripheral; otherwise that video cannot run
  through Omni v2v.
- **On `insufficient_credits`**: `advibly_buy_credits`, share the checkout
  link.
- **No em dashes, no emoji** in any caption, label, or copy you draft.

## Reference files

Read the chosen template file fully before writing the segment plan. Each
defines: when to pick it, the subject treatment (cutout-real or repaint),
the verbatim style block, the per-segment variation axes, the caption
treatment, the energy moves, a full example prompt, and its failure modes.

- `references/podcast-pop.md`: sticker-cutout subject, rotating bold
  backgrounds, punch-ins, torn caption bands. The arcads-style edit.
- `references/watercolor-wash.md`: full watercolor repaint.
- `references/anime-manga.md`: full anime/manga repaint.
- `references/newspaper-print.md`: cutout on halftone newsprint.
- `references/notebook-doodle.md`: cutout on notebook paper with ink doodles.
- `references/neon-vaporwave.md`: cutout in a synthwave neon world.
- `references/comic-book.md`: full pop-art comic repaint, halftone dots and
  starbursts.
- `references/vox-style.md`: cutout on yellow Bauhaus-geometry editorial
  layouts.
- `references/psychedelic-swirl.md`: full posterized repaint over liquid
  rainbow swirls.
- `references/flat-illustration.md`: full flat vector-style repaint in
  illustrated scenes.
- `references/cardboard-cutout.md`: torn-edge cutout on corrugated cardboard
  craft scenes.
- `references/pipeline.md`: source probing, proxy and cut recipes, composition assembly, upload
  limits, billing math, the positive-phrasing rule, failure triage. Read
  before debugging anything.
