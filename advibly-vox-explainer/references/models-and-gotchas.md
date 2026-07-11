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
  quiet room tone. Explicitly forbid narration, dialogue, and lyrics: the voiceover is muxed
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

## The final audio mix (local ffmpeg; Advibly has no mux tool)

Stitch the SFX-only clips with `advibly_stitch_videos` first, then mux voice and music
locally. The reference pipeline's mix craft, adapted:

```bash
curl -sL -o vox-sfx.mp4 "<stitched url>"; curl -sL -o vo.mp3 "<voiceover url>"; curl -sL -o music.mp3 "<music url>"

# durations first, so you know if the VO overruns the video
ffprobe -v error -show_entries format=duration -of csv=p=0 vox-sfx.mp4
ffprobe -v error -show_entries format=duration -of csv=p=0 vo.mp3

ffmpeg -y -i vox-sfx.mp4 -i vo.mp3 -i music.mp3 -filter_complex \
  "[0:a]volume=0.30[sfx];\
   [2:a]volume=0.90,apad[bedraw];\
   [sfx][bedraw]amix=inputs=2:duration=first:normalize=0[bg];\
   [1:a]apad[vopad];\
   [bg][vopad]sidechaincompress=threshold=0.03:ratio=8:attack=5:release=350[ducked];\
   [ducked]loudnorm=I=-14:TP=-1.5:LRA=11[a]" \
  -map 0:v -map "[a]" -shortest -c:v copy -c:a aac vox-final.mp4
```

Why each piece matters:

- **`sidechaincompress`** ducks the music (and clip SFX bed) **only while the voice speaks**,
  so the bed swells back in the gaps. Much better than a fixed music volume.
- **`apad` on the voice and the bed** protects the tail: without it, a sidechain follows the
  shorter input and `-shortest` clips a music-only or silent ending beat. `-shortest` then
  cuts cleanly to the video length.
- **`loudnorm=I=-14`** is the platform loudness standard. `amix` halves each input, so
  without a normalize stage the mix lands around -12 dB peak and reads quiet on social.
- **If the voiceover runs longer than the video** (check with the ffprobe lines above), do
  not time-stretch the voice. Either tighten the narration lines and regenerate the VO
  (cheap, preferred), or slow the video to fit by re-timing before the mux:
  `ffmpeg -i vox-sfx.mp4 -filter:v "setpts={vo_dur/vid_dur}*PTS" -an vox-slow.mp4` and mux
  against `vox-slow.mp4`. `setpts` slows the picture instead of freezing on the last frame.

### Optional: whip transitions instead of hard cuts

`advibly_stitch_videos` only hard-cuts. For whip transitions between shots, skip the MCP
stitch and concat the downloaded clips locally with `xfade`, recomputing each clip's start
(every transition overlaps by its own duration, so caption / timing offsets shift):

```bash
# two clips, a 0.3s whip slide between them (repeat the pattern down the chain)
ffmpeg -y -i a.mp4 -i b.mp4 -filter_complex \
  "[0:v][1:v]xfade=transition=slideleft:duration=0.3:offset=<a_dur-0.3>[v]" \
  -map "[v]" -an whip_ab.mp4
```

Then mux audio as above. Keep whips to one or two transitions, not every cut.

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
