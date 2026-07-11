# Audio, mix, and gotchas (Advibly)

The last mile: turn the SFX-only stitched cut into a finished narrated ad with per-beat voiceover,
music, and the on-twos snap, plus the model and failure-mode notes learned the hard way. Read this
before the audio mix or debugging a weak render.

## Why the narration is always external

**Hard rule: never bake the narrator into the video model.** The stick-figure visual is the
storytelling vehicle; baking VO into Gemini or Seedance forces a different voice every beat and
locks pacing to the video model's delivery. Generate the clips SFX-only, then generate the narration
separately (one line per beat, same voice): one consistent narrator across every beat, exact control
over timing, and clean MP3s to place and transcribe against. Every video prompt omits the
`Narrator:` line and forbids spoken words in its ambient block.

## Voiceover (advibly_generate_voiceover)

**Generate one VO line per beat, not one combined read.** This is the reliable way to time the
narration to the beats. A single `advibly_generate_voiceover` of the whole script ignores clip
timing: `[pause]` tags are short and unpredictable, so all the lines land front-loaded in the first
15 to 20 seconds and the later beats play silent. Instead, render each beat's line as its own
short VO clip and place each at its beat's start offset in the mix with `adelay` (see the final-mix
recipe). One call per beat, same `voice` every time.

```
# one call PER BEAT (same voice each time), e.g.:
advibly_generate_voiceover
  brand_id: <brand id>
  text: "It's late. You're exhausted. But your mind won't stop."   # beat 1 line only
  voice: <leo / rex / ara / sal / eve>
  language: <only when auto-detection would get it wrong>
```

Keep the delivery tags out of these short lines unless needed: a bare `<slow>...</slow>` wrapper on
a very short CTA line has been observed to return a clipped, too-fast render; if that happens,
regenerate the line as plain text. Probe each line's duration and confirm it fits its beat window
(a 6s beat holds ~15 to 18 spoken words); re-roll only the lines that overflow.

**Voice map** (the five xAI voices, matched to the stickman announcer tones):

| Narrator tone | Voice | Notes |
|---|---|---|
| Punchy announcer (default) | `leo` | deep, authoritative, strong; the classic DR-announcer read |
| Harder sell, direct CTA | `rex` | confident, clear; punchier close |
| Warmer, more human | `ara` | warm, friendly; softens the struggle beats |
| Smooth, even | `sal` | smooth, balanced; good for wellness / beauty pain points |
| High-energy, upbeat | `eve` | faster, lighter; for a playful food / lifestyle read |

Pick **one** voice for the whole ad; do not mix narrator voices across beats.

**Delivery tags** the script supports: inline `[pause]`, `[sigh]`, `[laugh]`; wrapping
`<whisper>...</whisper>`, `<slow>...</slow>`. Match delivery to your story's tone (calm and slow for a
gentle piece, punchy for a hard sell). Keep tags minimal on very short lines.

**A character's comic line** (if your story has one, e.g. a mascot quipping) can be a separate
`advibly_generate_voiceover` call in a different `voice`, placed at that beat's offset in the mix
(add another `adelay` input and include it in the VO `amix`).

**Check each line's length** against its beat window before mixing (`ffprobe -show_entries
format=duration`). If a line overruns its beat, tighten that line and regenerate it (cheap); never
time-stretch the voice.

## Music: one bed, or two with a switch (driven by the story)

Most stickman ads work with a **single instrumental bed** in the mood your concept wants. If your
story has a clear turn (a problem-to-relief pivot, a reveal, a punchline), generate **two beds** and
hard-cut between them at that turn's timestamp for a satisfying lift. This is optional and story-led,
not a required move.

```
advibly_generate_music
  brand_id: <brand id>
  prompt: "<the mood your story wants: tense and minimal, warm and calm, playful and bouncy, driving
            and upbeat...>, instrumental"
  instrumental: true
# generate a second bed only if your arc turns; describe the second mood
```

Keep beds instrumental; lyrics fight the narration. They run longer than needed; trim (and splice,
if two) in the mix. When you use two, the switch timestamp `T` is the cumulative duration of the
beats before the turn (sum the clip durations up to that beat).

## The final mix (local ffmpeg; Advibly has no mux tool)

Stitch the SFX-only clips with `advibly_stitch_videos` first, then build the spliced music bed and
mux voice and music locally. Clip foley quiet, music sidechain-ducked under the voice, VO on top,
tail protected, trimmed to video length.

```bash
curl -sL -o stick-sfx.mp4 "<stitched url>"
# one VO file per beat: v1.mp3 v2.mp3 ... (in beat order)
curl -sL -o v1.mp3 "<beat 1 VO url>"; curl -sL -o v2.mp3 "<beat 2 VO url>"   # ...etc
# music: one bed, or two (bedA.mp3 + bedB.mp3) if your arc turns
curl -sL -o bedA.mp3 "<music url>"

# duration of the stitched cut drives everything
ffprobe -v error -show_entries format=duration -of csv=p=0 stick-sfx.mp4   # e.g. 40

VID=$(ffprobe -v error -show_entries format=duration -of csv=p=0 stick-sfx.mp4)

# SINGLE-BED story: skip step 1 entirely and use your one bed as music-bed.mp3
#   cp bed.mp3 music-bed.mp3
# TWO-BED story (an arc that turns): set T = the turn's start = sum of clip durations before it,
# then build one spliced bed (bed A [0..T] crossfading into bed B [T..end]):
T=42   # example: the turn starts at 42s
BEATLEN=$(python3 -c "print(max(0, $VID - $T))")
ffmpeg -y -i bedA.mp3 -i bedB.mp3 -filter_complex \
  "[0:a]atrim=0:${T},afade=t=out:st=$(python3 -c "print($T-0.3)"):d=0.3[d];\
   [1:a]atrim=0:${BEATLEN},afade=t=in:st=0:d=0.3[b];\
   [d][b]concat=n=2:v=0:a=1[bed]" \
  -map "[bed]" music-bed.mp3

# 2) mux: per-beat VO placed by offset, music ducked under the VO, VO mixed back on top,
#    everything padded and trimmed to the exact video length VID.
#    Inputs: 0 = video (carries SFX), 1..N = the per-beat VO line clips, then the music bed.
#    OFF1..OFFN = each beat's start time in seconds (cumulative clip durations; add ~0.4s so the
#    line sits just inside its beat). Example offsets: 0.4 8.4 16.4 26.4 33.4 (one per beat, from cumulative clip durations)
ffmpeg -y -i stick-sfx.mp4 -i v1.mp3 -i v2.mp3 -i v3.mp3 -i v4.mp3 -i v5.mp3 -i music-bed.mp3 \
 -filter_complex \
  "[1]adelay=${OFF1}|${OFF1}[a1];[2]adelay=${OFF2}|${OFF2}[a2];[3]adelay=${OFF3}|${OFF3}[a3];\
   [4]adelay=${OFF4}|${OFF4}[a4];[5]adelay=${OFF5}|${OFF5}[a5];\
   [a1][a2][a3][a4][a5]amix=inputs=5:duration=longest:normalize=0,volume=1.7,apad,atrim=0:${VID}[vo];\
   [vo]asplit=2[vok][vom];\
   [0:a]volume=0.22,apad,atrim=0:${VID}[sfx];\
   [6:a]volume=0.55,apad,atrim=0:${VID}[bed];\
   [sfx][bed]amix=inputs=2:duration=longest:normalize=0[bg];\
   [bg][vok]sidechaincompress=threshold=0.03:ratio=10:attack=5:release=400,apad,atrim=0:${VID}[duck];\
   [duck][vom]amix=inputs=2:duration=longest:normalize=0[premix];\
   [premix]loudnorm=I=-15:TP=-1.5:LRA=11,afade=t=out:st=$(python3 -c "print($VID-0.6)"):d=0.6[a]" \
  -map 0:v -map "[a]" -t ${VID} -c:v copy -c:a aac -b:a 192k stick-final.mp4
```

`OFF*` are in milliseconds for `adelay` (e.g. `OFF1=400`, `OFF2=8400`). Set them from the cumulative
clip durations so each line opens on its beat.

Why each piece matters (and the two bugs this recipe fixes):

- **The VO must be MIXED BACK IN, not just used as a sidechain key.** `sidechaincompress` outputs
  only its first input (the ducked music bed); the voice is merely the trigger. So after ducking you
  MUST `amix` the voice copy (`[vom]`) back on top (`[duck][vom]amix...`). The earlier recipe mapped
  the ducked bed directly and shipped a video with music but **no voiceover at all**. `asplit`
  makes the two VO copies: `[vok]` triggers the duck, `[vom]` is the audible voice.
- **`sidechaincompress` truncates to the shorter input,** so its output (and the whole mix under
  `-shortest`) gets cut to the VO length, dropping the video tail. Fix: `apad,atrim=0:${VID}` after
  every stage and use `-t ${VID}` instead of `-shortest` so the audio spans the full video.
- **The music-bed splice** puts the tense drone under the problem beats and the upbeat beat from the
  transformation onward (line `T` up with the real transformation start).
- **`sidechaincompress`** ducks the bed **only while the voice speaks**, so it swells back in the
  gaps. Verify: `volumedetect` a VO window vs a gap window; speech windows should read ~8 to 10 dB
  louder than a music-only gap. If they read the same, the VO is missing (the bug above).
- **`loudnorm=I=-15`** is a social-loudness target (slightly softer than -14 suits a calm read).
- **If a VO line runs longer than its beat window:** tighten that one line and regenerate it
  (cheap); never time-stretch the voice.

## The on-twos snap pass (default for this genre)

The stick-figure look is limited animation on twos (~12 fps). AI video renders smooth, so add the
snap **after** the mix, never in a video prompt:

```bash
ffmpeg -i stick-final.mp4 -filter:v "fps=12,fps=24" -c:a copy stick-final-snap.mp4
```

This drops to 12 fps then duplicates frames back to 24, producing the authentic pose-to-pose snap
while keeping the audio intact. It also masks minor line wobble from the video model. **Default-ON
for this genre** (unlike the smooth claymation ad); skip it only if the user wants fully smooth
motion. For a slightly softer snap use `fps=15,fps=30`.

**Without a shell** (claude.ai, mobile): deliver the stitched cut, the VO URL, the two music URLs,
the switch timestamp, and the per-beat timing table; the user builds the music switch, mixes, and
applies the snap in CapCut (drone then beat on the music track split at the transformation, VO on
top, then a frame-rate/posterize-time effect for the snap).

## Captions (optional, after the VO is mixed in)

Upload the final mix with `advibly_upload_asset` (`source_url`), then `advibly_add_subtitles`. Two
styles fit this genre:

- **TikTok white-with-stroke**: white bold text, thick black stroke, lower quarter, per-phrase
  timing. The neutral default.
- **{BRAND_COLOR} highlight block**: white bold text on a solid {BRAND_COLOR} rounded rectangle, a
  slight 2 to 3 degree tilt, one word or short phrase per block, lower quarter. The comic, on-brand
  option that echoes the POW burst.

Add the brand and product names to `vocabulary` so the transcriber spells them right. Caption the
mixed cut, never the SFX-only cut, and caption before the snap pass or after (either works; the
snap does not change duration).

## Models and gotchas

### Image (the storyboard still)

| Model | Best at | Use it when |
|---|---|---|
| `gpt-image-2` | clean flat vector linework, cross-beat line-weight consistency when fed the style plate, baked POW/slogan text | **the default** for every beat |
| `nano-banana-2` | flat-line and silhouette retention | a single beat whose linework keeps picking up shading or whose cloud silhouette drifts; switch only that beat |
| `seedream-5-pro` | dense native text | a text-heavy hero burst (rare) |

- `quality` (`medium`/`high`) is `gpt-image-2` only; `resolution` (`1K`/`2K`/`4K`) is the others.
  Use `quality: "high"` on gpt-image-2, `2K` on nano.
- **One image model across the beats.** Do not mix mid-ad; a nano beat spliced between gpt beats can
  shift the line weight. If a single beat needs nano, re-anchor it on the first style plate.
- **The most common still failure is a shaded cartoon or 3D render instead of flat vector.** That is
  the model ignoring the STYLE LOCK, not a story problem. The flat-vector words (uniform black
  outline, no shading, no gradients, pure white) must survive verbatim; re-roll, and switch to
  nano-banana-2 if it persists on that beat.
- **Do not pass `product_id`.** On an image call it forces edit mode against the raw photo and pulls
  toward a glossy real product; the stickman product is a flat prop. Pass the product **photo URL**
  in `reference_image_urls` only on the beats that show the product and say "redrawn as a flat 2D
  vector cartoon icon in {BRAND_COLOR}, copy the exact label text".

### Video (the motion)

| Model | Range | End frame | Notes |
|---|---|---|---|
| `gemini-omni-flash` | up to 10s, 9:16 / 16:9 | no | **the default here.** Use a start frame and an explicit SFX-only/no-spoken-words prompt. |
| `seedance-2.0` | 10 or 15s, all aspects | **yes** (`end_image_url`) | Secondary fallback after two Gemini retries or for an approved before/after transformation. Use `mode: pro`. |
| `kling-v3` | 3 to 15s | yes | only if the ad must feature a recognizable real person (rare for stickman). |

- **Model precedence:** an explicit user request for Gemini or Seedance locks that model for the
  whole ad. Without one, use Gemini throughout; switch a single clip to Seedance only after two
  Gemini retries or for an approved before/after transformation.
- **SFX-only audio, no `Narrator:` line.** The voiceover is muxed on top; anything spoken in a clip
  collides with it.
- **Shading / 3D creep mid-clip is the #1 motion failure.** The anti-3D / anti-flicker constraint
  block (see `animate-prompts.md`) goes in every prompt; re-roll clips that pick up volume or
  shadows, and re-roll the underlying still on nano-banana-2 if a beat keeps shading.
- **Line boiling / flicker** is the second failure. The constraint block helps; the on-twos snap
  pass masks minor wobble; re-roll a clip whose outlines visibly boil.
- **`end_image_url` is Seedance-only** and only fires alongside `start_image_url`. It is the clean
  route for a before/after transformation beat, if your story has one.

### The asset workflow

- An `advibly_generate_image` call returns a public `url` you pass straight into `start_image_url` /
  `end_image_url` / `reference_image_urls`. No re-upload.
- `status: pending` still renders in chat; call `advibly_get_generation` (`wait: true`) only when you
  need the URL downstream (feeding a still into motion, or anchoring the next beat on the style
  plate).
- User files: `advibly_upload_asset` with `source_url` or `data_base64` plus `brand_id`, returns a
  reusable `url`.

### Failure modes

- **`content_rejected`**: the policy blocked the prompt. Rework wording; avoid real people and
  third-party marks. If the ad genuinely needs a recognizable real person animated, move that clip
  to `kling-v3`. The user's own brand and product are fine.
- **`insufficient_credits`**: `advibly_buy_credits`, share the checkout link; credits apply
  automatically after payment.
- **Shaded or 3D stills or clips**: the model ignoring the flat-vector STYLE LOCK. Re-roll; switch
  that still to `nano-banana-2`. The single most common stickman issue, and it is a style problem,
  not a structure problem.
- **{BRAND_COLOR} bleeding onto non-product elements** (a colored wall, the figure's body before the
  transformation): re-roll the still with the COLOR SYSTEM discipline reasserted ("{BRAND_COLOR}
  only on the product and its energy").
- **Recurring-element silhouette drift between beats**: regenerate the drifting still anchored on the
  first style plate (not the motion prompt); the fix lives in the image step. This is the hardest element
  to keep consistent.
- **Product logo text garbles**: keep the flat prop large and close (30 to 40 percent of frame) on
  the beats that feature it, and copy the exact logo/label text into the prompt.
