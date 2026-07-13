# Audio, mix, and gotchas (Advibly)

The last mile: turn the SFX-only stitched cut into a finished narrated ad, plus the model and
failure-mode notes learned the hard way. Read this before the audio mix or debugging a weak
render.

## Why the narration is always external

**Hard rule: never bake the narrator into the video model.** The claymation visual is the
storytelling vehicle; baking VO into Gemini or Seedance forces character and lip-sync compromises,
produces a different voice every beat, and locks pacing to the video model's delivery. Generate
the clips SFX-only, then generate one `advibly_generate_voiceover` track for the whole ad: one
consistent warm voice across all 8 beats, predictable length for caption timing, and a clean MP3
to transcribe against. Every video prompt omits the `Narrator:` line and forbids spoken words in
its ambient block.

## Voiceover (advibly_generate_voiceover)

One continuous read for the whole ad. Join the beats' narration lines in order; use `[pause]`
between beats when a line lands short of its window. The script was written to time in the beat
map (~2.5 to 3 words per second).

```
advibly_generate_voiceover
  brand_id: <brand id>
  text: <full narration, beats joined in order, [pause] between beats>
  voice: <ara / sal / leo / rex / eve>
  language: <only when auto-detection would get it wrong>
```

**Voice map** (the five xAI voices, matched to the claymation narrator tones):

| Narrator tone | Voice | Notes |
|---|---|---|
| Warm Aardman storyteller (default) | `ara` | warm, friendly; the closest to the classic gentle-storyteller read |
| Smooth, even, calm | `sal` | smooth, balanced; good for beauty / wellness |
| Wry documentary, dry humor | `leo` | authoritative, strong; the "matter-of-fact narrator" register |
| Confident, direct | `rex` | confident, clear; punchier CTAs |
| Energetic, upbeat | `eve` | for a lighter, faster food / lifestyle read |

Pick **one** voice for the whole ad; do not mix narrator voices across beats.

**Delivery tags** the script supports: inline `[pause]`, `[sigh]`, `[laugh]`; wrapping
`<whisper>...</whisper>`, `<slow>...</slow>`. Use `<slow>` on the CTA line for a warm landing,
`[pause]` between the despair beat and the discovery beat for a breath.

**Check the length** against the stitched cut before mixing:
```bash
ffprobe -v error -show_entries format=duration -of csv=p=0 clay-sfx.mp4   # video length
ffprobe -v error -show_entries format=duration -of csv=p=0 vo.mp3         # VO length
```
If the VO runs long, tighten the narration and regenerate (cheap); never time-stretch the voice.

**Character dialogue (beat 3), optional second voice.** By default fold the supporting
character's line into the narrator's read. If the user wants a distinct voice, generate that one
line as a separate `advibly_generate_voiceover` call with a different `voice`, note its duration,
and add it as a third input in the mix at the beat-3 offset (add another `[i:a]adelay=<ms>|<ms>`
and include it in the `amix`).

## Music (advibly_generate_music)

A gentle instrumental bed. Storybook, unhurried, warm; it sits under the voice.

```
advibly_generate_music
  brand_id: <brand id>
  prompt: <the beat map's music description + tempo + "modern storybook ad underscore, instrumental">
  instrumental: true
```

Good default brief: "gentle warm music-box and soft felt-mallet percussion with light strings,
unhurried, nostalgic and hopeful, ~80 BPM, storybook ad underscore, instrumental". Keep it
instrumental; lyrics fight the narration. Tracks run longer than the ad; trim in the mix.

## Final composition (one free call)

Call `advibly_render_composition` once with the approved clips as ordered `scenes`, each with
`volume: 0.3`; the narration in `voiceovers`; the instrumental generation as `music`; the
chosen `aspect_ratio`; and `keep_scene_audio: true`. If the user requested stop-motion judder,
also pass `frame_cadence: "on_twos"`; otherwise leave cadence smooth. A separately voiced character line gets
its own voiceover entry at that beat's cumulative `start_seconds`. The default static music bed
already sits correctly under narration, so do not add dynamic gain processing or loudness targets.
Music is automatically trimmed to the composition with a tail fade.

The call returns `status: pending`, `generation_id`, and `edit_url`. Let the chat widget poll.
Use `advibly_get_generation` with `wait: true` only when the finished URL is needed for captions
or publishing. Mention the edit URL in final delivery so the user can fine-tune the ad in the
Advibly video editor.

## Optional: final-render stop-motion judder

Smooth motion is the default (all the reference clips are smooth). Only if the user explicitly
wants the ~12 fps stop-motion judder, pass `frame_cadence: "on_twos"` to
`advibly_render_composition`, never in a video prompt. The temporal effect holds visuals while
keeping scene audio, narration, and music continuous. It appears in the returned project under
**Effects > On Twos** for realtime preview and adjustment.

## Captions (optional, after the VO is mixed in)

Upload the final mix with `advibly_upload_asset` (`source_url`), then `advibly_add_subtitles`.
Two styles fit this genre:

- **TikTok white-with-stroke**: white bold text, thick black stroke, lower third, per-phrase
  timing. The neutral default.
- **Orange highlight block**: white bold text on a solid orange-red rounded rectangle, a slight
  2 to 3 degree tilt, one word or short phrase per block, lower third slightly off-center. The
  handmade, on-brand-for-claymation option.

Add the brand and product names to `vocabulary` so the transcriber spells them right. Never
caption the SFX-only cut; there is nothing to transcribe.

## Models and gotchas

### Image (the storyboard still)

| Model | Best at | Use it when |
|---|---|---|
| `gpt-image-2` | clean sculpted-clay render, cross-beat identity when fed the prior still, baked chart text | **the default** for every beat |
| `nano-banana-2` | texture retention, up to 4K | a single beat whose clay keeps flattening on a close-up or product prop; slightly weaker on identity, so switch only that beat |
| `seedream-5-pro` | dense native text | a text-heavy infographic beat (rare) |

- `quality` (`medium`/`high`) is `gpt-image-2` only; `resolution` (`1K`/`2K`/`4K`) is the others.
  Use `quality: "high"` on gpt-image-2, `2K` on nano.
- **One image model across the character-carrying beats.** Do not mix; a nano beat spliced
  between gpt beats shifts the face.
- **The most common still failure is a smooth 3D render instead of sculpted clay.** That is the
  model ignoring the STYLE LOCK, not a story problem. The clay-texture words (thumbprint
  impressions, tool marks, matte, wool weave) must survive verbatim; re-roll, and switch to
  nano-banana-2 if it persists on that beat.
- **Do not pass `product_id`.** On an image call it forces edit mode against the raw photo and
  pulls toward a glossy real bottle; the claymation product is a clay prop. Pass the product
  **photo URL** in `reference_image_urls` on beats 6 to 8 and say "rendered as a clay-stylized
  prop, copy the exact label text".

### Video (the motion)

| Model | Range | End frame | Notes |
|---|---|---|---|
| `gemini-omni-flash` | up to 10s, 9:16 / 16:9 | no | **the default here.** Use 10s, a start frame, and an explicit SFX-only/no-spoken-words prompt. |
| `seedance-2.0` | 10 or 15s, all aspects | **yes** (`end_image_url`) | Secondary fallback after two Gemini retries or for an approved end-frame transformation. Use 10s and `mode: pro`. |
| `kling-v3` | 3 to 15s | yes | only if the ad must feature a recognizable real person (rare for claymation). |

- **Model precedence:** an explicit user request for Gemini or Seedance locks that model for the
  whole ad. Without one, use Gemini throughout; switch a single clip to Seedance only after two
  Gemini retries or for an end-frame transition, with user approval.
- **SFX-only audio, no `Narrator:` line.** The voiceover is composed on top; anything spoken in a
  clip collides with it.
- **Clay smoothing mid-clip is the #1 motion failure.** The anti-smoothing constraint block
  (see `animate-prompts.md`) goes in every prompt; re-roll clips that flatten, and re-roll the
  underlying still on nano-banana-2 if a beat keeps flattening.
- **`end_image_url` is Seedance-only** and only fires alongside `start_image_url`. It is the
  clean route for the beat-7 before/after transformation.

### The asset workflow

- An `advibly_generate_image` call returns a public `url` you pass straight into
  `start_image_url` / `end_image_url` / `reference_image_urls`. No re-upload.
- `status: pending` still renders in chat; call `advibly_get_generation` (`wait: true`) only
  when you need the URL downstream (feeding a still into motion, or anchoring the next beat).
- User files: `advibly_upload_asset` with `source_url` or `data_base64` plus `brand_id`, returns
  a reusable `url`.

### Failure modes

- **`content_rejected`**: the policy blocked the prompt. Rework wording; avoid real people and
  third-party marks. If the ad genuinely needs a recognizable real person animated, move it to
  `kling-v3`. The user's own brand and product are fine.
- **`insufficient_credits`**: `advibly_buy_credits`, share the checkout link; credits apply
  automatically after payment.
- **Plasticky, smooth stills or clips**: model flattening the clay. Re-roll; switch that still to
  `nano-banana-2`. This is the single most common claymation issue, and it is a texture problem,
  not a prompt-structure problem.
- **Label text garbles**: keep the clay prop large and close (30 to 40 percent of frame) on
  beats 6 and 8, and copy the exact label text into the prompt.
- **Identity drift between beats**: regenerate the drifting still anchored on the prior approved
  protagonist still (not the motion prompt); the fix lives in the image step.
