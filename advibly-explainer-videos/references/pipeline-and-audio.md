# Pipeline and audio (Advibly mechanics)

The shared machinery under every style: the asset workflow, the image and video model matrix, the
on-twos step-frame snap, generating and composition the audio (`advibly_render_composition` by default, an
ffmpeg recipe for full control), captions, and failure modes. Read before the keyframe, motion, or
audio steps. Style-specific look/motion/audio DNA lives in the style file; this file is
style-agnostic plumbing.

## The Advibly asset workflow

- An `advibly_generate_image` call returns a public `url`. Pass it straight into `start_image_url`,
  `end_image_url`, or `reference_image_urls` on the next call. No re-upload.
- `status: pending` still renders in chat; call `advibly_get_generation` (`wait: true`) only when
  you need the finished URL downstream (feeding a keyframe into motion, you always do).
- **The product photo** is the one reference you carry: store brands
  (`brand_type: "ecom_store"`) use `advibly_get_products` (note the product's image URL); other
  brands use a photo from `advibly_get_assets` or an `advibly_upload_asset` (`source_url` for large
  files / video, `data_base64` for small local images, plus `brand_id`). Pass the URL in
  `reference_image_urls` on product shots. **Never pass `product_id`** to a generation tool: on an
  image call it forces edit mode against the raw photo (fighting the style); on a video call it
  replaces your start frame.
- **Pre-reveal and no-product shots get no product reference**, or the model leaks the product early.

## Image model matrix

| Model | Best at | Param | Use it when |
|---|---|---|---|
| `gpt-image-2` | dense spec adherence, baked-in text, strict palettes | `quality: "high"` | the default for text-heavy and clean styles (pixel, whiteboard, papercraft, mixed-media, kawaii, low-poly, cinematic 2D print) |
| `nano-banana-2` | holding organic grain (clay, felt, paper, gouache) | `resolution: "2K"` (up to `4K`) | the texture styles (claymation, felted wool, textured gouache) |
| `seedream-5-pro` | very dense native text | `resolution: "2K"` | a single headline that keeps degrading on the primary model (rare) |

- **Use the model named in the chosen style's `recommended_image_model`.** One model across every
  keyframe. If the texture keeps flattening on gpt-image-2 for a style that should be organic,
  the style file already routes you to nano-banana-2; do not improvise a mid-set swap.
- **Baked headline text** (styles whose `typography.headline_treatment` is not "None") is written
  into the prompt and rendered by the image model, treated as an in-world object (clay letters,
  marker handwriting, a pixel banner, a gold-foil ribbon, a sticky note). Advibly has no
  text-overlay tool; never plan to overlay it later. If a headline degrades: reroll with
  `num_images` (up to 4), shorten the words, or (rarely) render that one frame on `seedream-5-pro`.

## Video model matrix

| Model | Range | End frame | Aspect | Notes |
|---|---|---|---|---|
| `gemini-omni-flash` | 4 to 10s | no | 9:16 / 16:9 | **the default.** Positive-phrasing only, most stable baked text, best layered parallax from one start frame. |
| `seedance-2.0` | 4 to 15s | **yes** (`end_image_url`) | all | `mode: "pro"`. For an exact end-frame landing (papercraft's z-tunnel), a non-standard aspect, or a shot past 10s. |
| `kling-v3` | 3 to 15s | yes | all | only if the ad must show a recognizable real person or third-party mark (Omni Flash and Seedance block these at the content filter). |

**Duration policy:** recommend 4 to 6 seconds per generated shot on every route. Omni Flash does
not accept durations below 4 seconds. Use 7 to 10 seconds only when the approved beat genuinely
needs a longer hold; do not use a different model merely to create sub-4-second shots. Create a
faster feel with motion, internal transitions, or downstream edits inside the valid clip.

- **Use the model named in the style's `recommended_video_model`.** One video model per ad. Only
  papercraft defaults to Seedance (it needs the end frame for the tunnel reveal); everything else
  is Omni Flash.
- **Omni Flash is positive-only.** The style's `motion_prompt_dna` is already phrased this way;
  convert any "no X" you add into a positive ("the texture stays exactly as printed").
- **Do not ask the video model for 12fps.** Cadence is set by the snap pass below. Ask only for the
  style's element and camera motion.
- **SFX-only, no spoken words** in every clip prompt; the VO is composed on top.
- Escalation for a stubborn clip: two Omni Flash retries (reroll or adjust the motion prompt), then
  the style's Seedance route if it needs an end frame.

## The on-twos step-frame snap (stop-motion styles only)

The stop-motion styles (Claymation, Felted Wool, Whiteboard, Textured Gouache, Mixed-Media, Pixel
Art) read as ~12fps. The video model renders smooth, so the snap is added **after** the mix, never
in a video prompt. The catalog's "On-twos snap" column and the beat map's `snap` field say when.

```bash
# drop to 12 fps, then duplicate frames back to 24 for the authentic pose-to-pose snap; audio intact
ffmpeg -i shot.mp4 -filter:v "fps=12,fps=24" -c:a copy shot-snap.mp4
```

- Apply it to the **final mixed file** (last step), so it snaps the whole delivered cut and masks
  minor line wobble from the video model. For a slightly softer snap use `fps=15,fps=30`.
- **Smooth styles skip it** (Low-Poly, Cozy Kawaii, Papercraft). **Cinematic 2D Print skips it too**:
  its choppiness comes from hard cuts and limited element motion, and a global snap would wrongly
  choppify its smooth camera push. A global snap on a smooth style ruins it.
- **No shell (claude.ai, mobile):** the snap cannot be applied. Ship the smooth mix and tell the
  user they can apply a posterize-time / frame-rate effect in CapCut, or accept the smoother look.

## Voiceover (advibly_generate_voiceover)

**Voice map** (the five xAI voices): `eve` energetic/upbeat, `ara` warm/friendly, `rex`
confident/clear, `sal` smooth/balanced, `leo` authoritative/strong. Map the style's
`voiceover.voice_character` to one (the catalog's suggested-voice column does this) and confirm
with the user. **One voice for the whole ad.**

**Default: one VO line per shot, never one continuous read.** This is the sync rule and it is not
optional. A single combined read laid from t=0 drifts: the moment any line runs shorter or longer
than its shot, every line after it lands on the wrong visual, and a read that totals less than the
video finishes early and leaves the payoff shot silent (the exact failure to avoid). Generate each
shot's line as its own short `advibly_generate_voiceover` clip, same voice each time, then place each
at its shot's start so the narration is pinned to the cuts.

```
# one call PER SHOT (same voice each time), e.g. shot 1:
advibly_generate_voiceover
  brand_id: <brand id>
  text: "Your mornings shouldn't start with a fistful of pills."   # this shot's line only
  voice: <eve|ara|rex|sal|leo>
  language: <only when auto-detection would get it wrong>
```

- **Write each line to fit its shot.** At the style's `voiceover.pace_words_per_sec` (most ~2.5 wps),
  a 5s shot holds ~11 to 13 words. Keep each line at or under its shot length so it finishes before
  the cut; a hair of breathing room at the end reads better than a line bleeding into the next shot.
- **Probe every line** (`ffprobe -show_entries format=duration`) against its shot's duration. If a
  line overruns its shot, **tighten it and regenerate** (cheap); never time-stretch the voice.
- **The lines still read as one script.** Write the whole narration first as a continuous read, then
  split it at the shot boundaries so it flows when heard in order.
- Delivery tags: inline `[pause]`, `[sigh]`; wrapping `<whisper>`, `<slow>`. Keep them minimal on
  very short lines (a `<slow>` on a tiny CTA can render clipped).

A single continuous read is a last resort ONLY when you cannot place audio per shot at all (a
no-shell client that also cannot do the per-clip compose below). It will drift; warn the user.

## Music (advibly_generate_music)

One instrumental bed from the style's `audio_recipe.music_prompt` (append tempo + "modern ad
underscore" if not already there). Keep it instrumental; lyrics fight the narration.

```
advibly_generate_music
  brand_id: <brand id>
  prompt: <style.audio_recipe.music_prompt + tempo + "instrumental, modern ad underscore">
  instrumental: true
```

Generate a **second bed** only if the arc clearly turns (problem to relief, a reveal); hard-cut
between them at the turn's timestamp. Optional and story-led. Tracks run longer than the ad;
the composition automatically trims them to length.

## Final composition (one call)

For on-twos styles, first apply the step-frame pass to each individual clip. The composition tool
cannot decimate frames. Then call `advibly_render_composition` once:

- `scenes`: 1 to 12 ordered clip generation ids or HTTPS URLs; set each scene's `volume` to
  `0.2` for quiet SFX.
- `voiceovers`: one generated narration line per shot, each with `start_seconds` equal to that
  shot's cumulative timeline offset.
- `music`: the instrumental generation id or URL. It auto-trims with a tail fade.
- `music_volume`: **leave it unset.** The default bed (0.22) is the level the score sits at
  BETWEEN narration lines, and the renderer automatically ducks it to ~35% of that (~0.08) while a
  voiceover segment is speaking, ramping down just before each line and swelling back after it.
  Setting a low value by hand double-dips: a 0.08 bed ducks to ~0.03 and disappears. Only pass it
  to raise the bed for a piece with no voiceover at all.
- `aspect_ratio`: the chosen delivery aspect.
- `keep_scene_audio: true`.

Ducking is handled by the renderer, not by you. Do not hand-ride the music level, add loudness
targets, or try to fake a duck by splitting the bed into segments. The call returns
`status: pending`, `generation_id`, and `edit_url`; the chat widget polls. Use
`advibly_get_generation` with `wait: true` only when a finished URL is required for captions or
another next step. Mention the edit URL in final delivery.

## Captions (optional, after the VO is mixed in)

Upload the final with `advibly_upload_asset` (`source_url`), then `advibly_add_subtitles` with a
dynamic preset (`glide`, `fusion`, `glass`). Never caption the SFX-only cut. Add brand and product
names to `vocabulary` so the transcriber spells them right. Styles whose design already carries a
subtitle band (Cinematic 2D Print, Textured Gouache, Cozy Kawaii, Low-Poly, Felted Wool) should
match that band's white-with-shadow look; the baked-headline styles usually need no captions unless
the user wants accessibility subtitles.

## Failure modes

- **`content_rejected`**: the policy blocked the prompt. Rework wording; avoid real people and
  third-party marks. If the ad genuinely needs a recognizable real person, move that clip to
  `kling-v3`. The user's own brand and product are fine.
- **`insufficient_credits`**: `advibly_buy_credits`, share the checkout link; credits apply after
  payment.
- **Off-style keyframe** (smooth 3D where it should be clay, anti-aliasing where it should be pixel,
  clean vector where it should be marker): the model ignoring the STYLE LOCK. Reroll; the style
  file's `image_style_block` and `image_negative_prompt` must survive verbatim. Switch the image
  model only per the style file's guidance.
- **Texture smoothing or 3D creep mid-clip**: the top motion failure. Reroll the clip; if a keyframe
  keeps shading, reroll it on nano-banana-2 (for the texture styles). The style's `failure_modes`
  list the exact guard per style.
- **Smooth motion where it should be stop-motion**: expected. The video model cannot hold 12fps.
  Fix it with the snap pass, not by re-rolling.
- **VO longer than the video**: tighten the script and regenerate (cheap); never time-stretch.
- **Composed video missing the voiceover**: confirm every narration generation id or URL is present in `voiceovers` and its `start_seconds` is correct.
