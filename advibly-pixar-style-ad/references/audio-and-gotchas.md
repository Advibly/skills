# Audio, mix, and gotchas (Advibly)

The last mile: turn the SFX-only stitched cut into a finished narrated ad, plus the model and
failure-mode notes learned the hard way. Read this before the audio mix or debugging a weak
render.

## Why the narration is always external

**Hard rule: never bake the narrator into the video model.** The feature-3D visual is the
storytelling vehicle; baking VO into Gemini or Seedance forces character and lip-sync compromises,
produces a different voice every beat, and locks pacing to the video model's delivery. Generate
the four clips SFX-only, then generate one `advibly_generate_voiceover` track for the whole ad:
one consistent warm voice across all four beats, predictable length for caption timing, and a
clean MP3 to transcribe against. Every video prompt omits any narrator line and forbids spoken
words in its audio block.

## Voiceover (advibly_generate_voiceover)

One continuous read for the whole ad. Join the four beats' narration lines in order; use `[pause]`
between beats when a line lands short of its 8-second window. The script was written to time in
the beat map (~2.2 to 2.6 words per second, roughly 18 to 20 words per beat).

```text
advibly_generate_voiceover
  brand_id: <brand id>
  text: <full narration, beats joined in order, [pause] between beats, <slow> on the CTA line>
  voice: <ara / sal / leo / rex / eve>
  language: <only when auto-detection would get it wrong>
```

**Voice map** (the five xAI voices, matched to feature-3D narrator tones):

| Narrator tone | Voice | Notes |
|---|---|---|
| Warm feature-film storyteller (default) | `ara` | warm, friendly; the gentle wonder-and-heart read that fits a Pixar-style short |
| Smooth, even, calm | `sal` | smooth, balanced; good for beauty / wellness |
| Authoritative, matter-of-fact | `leo` | strong, documentary register |
| Confident, direct | `rex` | punchier CTAs |
| Energetic, upbeat | `eve` | lighter, faster food / lifestyle read |

Pick **one** voice for the whole ad; do not mix narrator voices across beats.

**Delivery tags** the script supports: inline `[pause]`, `[sigh]`, `[laugh]`; wrapping
`<whisper>...</whisper>`, `<slow>...</slow>`. Use `<slow>` on the CTA line for a warm landing, and
`[pause]` between the hook beat and the reveal beat for a beat of breath.

**Check the length** against the stitched cut before mixing:
```bash
ffprobe -v error -show_entries format=duration -of csv=p=0 pixar-sfx.mp4   # video length (~32s)
ffprobe -v error -show_entries format=duration -of csv=p=0 vo.mp3          # VO length
```
If the VO runs long, tighten the narration and regenerate (cheap); never time-stretch the voice.

## Music (advibly_generate_music)

One gentle instrumental bed. Warm, hopeful, storybook; it sits under the voice.

```text
advibly_generate_music
  brand_id: <brand id>
  prompt: <the beat map's music description + tempo + "modern feature-film ad underscore, instrumental">
  instrumental: true
```

Good default brief: "gentle warm feature-film underscore, soft piano and light strings with a
hint of glockenspiel, hopeful and unhurried, ~85 BPM, storybook ad bed, instrumental". Keep it
instrumental; lyrics fight the narration. Tracks run longer than the ad; trim in the mix.

## Final composition (one free call)

Call `advibly_render_composition` once with the approved clips as ordered `scenes`, each with
`volume: 0.3`; the narration in `voiceovers`; the instrumental generation as `music`;
`aspect_ratio: "9:16"`; and `keep_scene_audio: true`. The default static music bed already sits
correctly under narration. Music is automatically trimmed to the composition with a tail fade.

The call returns `status: pending`, `generation_id`, and `edit_url`. Let the chat widget poll.
Use `advibly_get_generation` with `wait: true` only when the finished URL is needed downstream.
Mention the edit URL in final delivery so the user can fine-tune the ad in the Advibly video editor.

## Captions (optional, after the VO is mixed in)

Upload the final mix with `advibly_upload_asset` (`source_url`), then `advibly_add_subtitles`:

- **TikTok white-with-stroke**: white bold text, thick black stroke, lower third, per-phrase
  timing. The neutral default.
- **Highlight block**: white bold text on a solid rounded rectangle, one word or short phrase per
  block, lower third slightly off-center. The punchier, more playful option.

Add the brand and product names to `vocabulary` so the transcriber spells them right. Never
caption the SFX-only cut; there is nothing to transcribe.

## Models and gotchas

### Image (the storyboard still)

| Model | Best at | Use it when |
|---|---|---|
| `gpt-image-2` | clean feature-3D render, cross-beat identity when fed the prior still, baked label text | **the default** for every beat |
| `nano-banana-2` | texture retention, up to 4K | a single beat whose detail keeps flattening on a close-up or product prop; slightly weaker on identity, so switch only that beat |

- `quality` (`medium`/`high`) is `gpt-image-2` only; `resolution` (`1K`/`2K`/`4K`) is the others.
  Use `quality: "high"` on gpt-image-2.
- **One image model across the character-carrying beats.** Do not mix; a nano beat spliced between
  gpt beats shifts the face.
- **Do not pass `product_id`.** On an image call it forces edit mode against the raw photo. Pass
  the product **photo URL** in `reference_image_urls` on the reveal and CTA beats instead, and
  keep the label copied exactly from the reference.

### Video (the motion)

| Model | Range | End frame | Notes |
|---|---|---|---|
| `gemini-omni-flash` | any 4-10s, 9:16 / 16:9 | no | **the default here.** Use **8s**, a start frame, and an explicit SFX-only / no-spoken-words prompt. |
| `seedance-2.0` | any 4-15s, all aspects | **yes** (`end_image_url`) | Secondary fallback after two Gemini retries or for an approved end-frame transition. Use **8s** and `mode: pro`. |
| `kling-v3` | 3 to 15s | yes | only if the ad must feature a recognizable real person (rare here). |

- **8 seconds per shot is the rule.** 10-second beats go static and start to morph. Both default
  models support 8s.
- **Model precedence:** an explicit user request for Gemini or Seedance locks that model for the
  whole ad. Without one, use Gemini throughout; switch a single clip to Seedance only after two
  Gemini retries or for an end-frame transition, with user approval.
- **SFX-only audio, no narrator line.** The voiceover is composed on top; anything spoken in a clip
  collides with it.
- **Morphing / drift mid-clip is the #1 motion failure.** Simplify to one primary action, lock
  the camera, hold to 8 seconds, and repeat the start-frame preservation constraint. Re-roll
  clips that morph; do not add more motion to hide the defect.
- **`end_image_url` is Seedance-only** and only fires alongside `start_image_url`.

### The asset workflow

- An `advibly_generate_image` call returns a public `url` you pass straight into
  `start_image_url` / `end_image_url` / `reference_image_urls`. No re-upload.
- `status: pending` still renders in chat; call `advibly_get_generation` (`wait: true`) only when
  you need the URL downstream (feeding a still into motion, or anchoring the next beat).
- User files: `advibly_upload_asset` with `source_url` or `data_base64` plus `brand_id`, returns a
  reusable `url`.

### Failure modes

- **`content_rejected`**: the policy blocked the prompt. Rework wording; avoid real people,
  studio names, and third-party marks. The user's own brand and product are fine.
- **`insufficient_credits`**: `advibly_buy_credits`, share the checkout link; credits apply
  automatically after payment.
- **Label text garbles**: keep the product large and close (30 to 40 percent of frame) on the
  reveal and CTA beats, and copy the exact label text into the prompt.
- **Identity drift between beats**: regenerate the drifting still anchored on the prior approved
  protagonist still (not the motion prompt); the fix lives in the image step.
