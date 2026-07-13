# Models and Gotchas (Advibly)

Everything here is learned failure. Read it before debugging a bad render or a bad mix; most
problems below are already solved.

## Image models (the keyframe / collage poster)

| Model | Best at | Use it when |
|---|---|---|
| `nano-banana-2` | **paper texture and grain**, up to 4K, renders baked text cleanly, takes references | **the default.** The Vox look lives in the grain and this holds it best. |
| `gpt-image-2` | the strongest dense baked text, `quality: high`, reliable reference edits | one poster's headline keeps degrading, or a product-reference edit fights the collage on nano |
| `seedream-5-pro` | very dense typographic layouts, native text | a poster is mostly type / a ransom-note wall of words |
| `nano-banana-lite`, `seedream-5-lite` | speed and cost, lower fidelity | rough drafts only, never the delivered set |

- **One image model per delivered ad.** Whatever wins the Phase 3 bake-off is used for every
  poster. Do not mix.
- **The most common failure is a digital-smooth, plasticky poster.** That is the model
  flattening the paper, not a prompt problem. Re-roll on `nano-banana-2` before rewriting the
  prompt. The texture words in the style block (misregistration, aged paper, torn edges) must
  survive verbatim.
- **`quality` is `gpt-image-2` only.** `resolution` is `1K` / `2K` / `4K`; `2K` is the right
  default for posters that get animated.
- **`on_brand: false` on every call.** The theme palette is the whole point; the brand-kit
  board would recolor it. `brand_id` is still required (it files the work); with `on_brand`
  false it does not style the output.
- **Do not pass `product_id` to the generation tools.** On an image call it forces edit mode
  against the raw photo and fights the collage; on a video call it overrides the start frame.
  Pass the product **photo URL** in `reference_image_urls` instead, only on product beats.
- **Prompts cap at 2000 characters.** The full-density style block plus a scene fits in about
  900 to 1200. If you are over, trim adjectives, never the layering or the product line.

## Video models (the motion)

| Model | Range | Aspect | End frame | Real people / logos | Notes |
|---|---|---|---|---|---|
| `gemini-omni-flash` | 4 to 10s | 9:16, 16:9 only | no | **blocked** | **the default.** Best text stability and layered parallax on flat art. Max 3 reference images. Positive phrasing only. |
| `seedance-2.0` | 4 to 15s (`mode: fast`) | all | **yes** (`end_image_url`) | blocked | use for end-frame reveals (assemble-from-empty hard version), 1:1 / non-vertical, or shots past 10s. Supports negatives. Max 4 refs. |
| `kling-v3` | 3 to 15s | 16:9, 9:16, 1:1 | yes | **allowed** | the only one that animates recognizable celebrities and third-party marks. Native audio (`generate_audio`). |

- **One video model per delivered ad.** Move the whole set, never one shot.
- **Positive phrasing for omni-flash.** "no" and "don't" can produce the opposite. Convert
  every constraint into a positive: "the camera stays locked" not "no camera move"; "the
  lettering stays exactly as printed" not "don't redraw the text".
- **SFX-only audio in every clip.** Paper foley (tear, rustle, whoosh, settle thunk) and
  quiet room tone. Explicitly forbid narration, dialogue, and lyrics: the voiceover is composed
  on top in the final mix and anything spoken in a clip collides with it.

### Content blocks (architecture-level, not fixable by rewording)

Feeding a recognizable real celebrity or a third-party brand logo into omni or seedance is
blocked at the content filter. Removing the name from the prompt, converting the file, or
using a faceless silhouette does not help: it is the image content. Route the whole ad to
`kling-v3`. The user's **own** brand and product are not third-party; the product photo
composites fine.

## Text and headlines

- **Every headline is baked into the image.** There is no text-overlay tool. Video models
  smear text, so the keyframe carries it and the motion prompt only anchors it.
- If a headline degrades: re-roll with `num_images` (up to 4), shorten to 2 to 3 words, or
  render that one poster on `gpt-image-2` / `seedream-5-pro`. Never plan to add text after.
- Headline protection is per shot. Anchor the text hard on title shots; detail shots with no
  headline can move wilder.

## The asset workflow

- An `advibly_generate_image` call returns a public `url` you pass straight into
  `start_image_url` / `reference_image_urls` / `end_image_url` on the next call. No re-upload.
- If a call returns `status: pending`, the media still renders in chat. Call
  `advibly_get_generation` with `wait: true` only when you need the finished URL downstream
  (feeding a keyframe into motion, you always do).
- To bring in a user's file: `advibly_upload_asset` with `source_url` (public link) or
  `data_base64` (small local files) plus `brand_id`, returns a reusable `url`.

## Final composition

Call `advibly_render_composition` once with the clips as ordered `scenes`, each with
`volume: 0.3`; the narration in `voiceovers`; the instrumental generation as `music`; the
chosen `aspect_ratio`; and `keep_scene_audio: true`. The default static music level already sits
correctly under narration. Music auto-trims to video length with a tail fade. Keep whip effects
inside the source clips; final composition uses hard cuts.

The call returns `status: pending`, `generation_id`, and `edit_url`. Let the chat widget poll,
and wait explicitly only for subtitles or another downstream step. Mention the edit URL in final
delivery.

## Captions (optional, after the VO is mixed in)

Upload the final with `advibly_upload_asset` (`source_url`), then `advibly_add_subtitles`
with a dynamic preset (`glide`, `fusion`, `glass`). Never caption the SFX-only cut, there is
nothing to transcribe. Add the brand and product names to `vocabulary` so the transcriber
spells them right.

## Failure modes

- **`content_rejected`**: the policy blocked the prompt. Rework wording; if the trigger is a
  real person or a third-party mark, move the whole ad to `kling-v3`.
- **`insufficient_credits`**: `advibly_buy_credits`, share the checkout link.
- **A stubborn reveal that will not land**: switch that beat (and the ad) to the Seedance
  start-frame plus `end_image_url` route.
- **Plasticky, smooth posters**: model flattening, re-roll on `nano-banana-2` (see above).
