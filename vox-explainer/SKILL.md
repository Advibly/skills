---
name: vox-explainer
description: >
  Turn a brand angle into a finished Vox-style paper-collage explainer ad on the Advibly MCP.
  Approve a narrative beat map and visual theme, generate layered collage-poster keyframes,
  animate living paper motion with Gemini Omni Flash, then compose narration, music, and SFX.
  Supports baked headlines, photoreal product cutouts, assemble-from-empty reveals, confetti,
  impact shake, and editorial collage motion. Trigger for "Vox style video", "paper collage
  animation", "motion collage", "collage explainer", "collage ad", "paper collage explainer",
  "make a collage ad", "explainer video for my product", or a reference with the editorial
  torn-paper look. Use even without the word "skill" when that style is clearly requested.
---

# Advibly Vox Explainer

Turn one brand angle into a finished **Vox-style paper-collage explainer ad**: a bold, punchy,
narrated spot where each beat is a torn-paper collage poster that comes alive, cut every 4 to 6
seconds, with the real product photo living photoreal inside the paper world. Everything
generates and assembles on the Advibly MCP.

The look is the modern editorial paper collage popularized by Vox explainers and creators like
Stav Zilber and rom1trs: hand-cut paper cut-outs, torn edges, tape, halftone dots, newspaper
clippings, bold flat color per beat, big cut-out headlines baked into the image.

Speak in the user's language. No em dashes anywhere in output; use periods or line breaks.
Keep labels and on-screen copy free of emoji unless asked.

## The core idea (read this first)

The collage look and the collage motion are **two different steps**:

1. **The look is born in the IMAGE step.** Each shot is a finished collage *poster* made by the
   image model. All the collage DNA (torn paper, cut-outs, halftone, bold flat color, headline
   text) lives in that image. If the poster is not a rich layered collage, nothing downstream
   will save it. Re-roll cheap here rather than paying to animate a weak image.
2. **The motion is added after.** The video model animates the whole poster into a "living
   collage": layers parallax, elements bob and scatter, edges flutter. The more clearly layered
   the poster (distinct cut-outs, visible edges, own drop shadows), the more layered motion the
   video model can produce. A flat blended image can only be panned as one plane.
3. **The Advibly twist: the product stays photoreal.** Everything in the frame is printed
   paper EXCEPT the real product photo, which is composited in unchanged. Real product in a
   craft world is what sells it as an ad. This is the one element you never stylize.

Two layers drive everything, each with its own reference file. Read both before writing the
beat map or any prompt:

- **STORY layer**: `references/beat-layer.md`. Narrative arcs, hook patterns, beat counts,
  shot sizes, camera moves, element motion, anti-monotony rules.
- **LOOK layer**: `references/prompt-guide.md`. The image and video prompt structures, the
  vocabulary banks that fill them, and the theme presets.

## Hard defaults (do not drift)

- **Image model:** `advibly_generate_image` with `model: "nano-banana-2"`, `resolution: "2K"`.
  The whole Vox look lives in the paper grain, and nano-banana-2 holds that grain the best
  while still rendering baked headline text cleanly and taking the product photo as a
  reference. It is the default. `gpt-image-2` (`quality: "high"`) is the fallback for one
  poster whose headline keeps degrading (strongest at dense baked text) or a product-reference
  edit that fights the collage; `seedream-5-pro` is the option for a type-heavy poster. The
  Phase 3 bake-off decides the winner by eye, and that one model renders the whole ad. If a
  set comes back digital-smooth and plasticky, that is the model flattening the paper: re-roll
  on nano-banana-2 before touching the prompt. Never mix image models within one ad.
- **Video model:** `advibly_generate_video` with `model: "gemini-omni-flash"`. Omni Flash
  takes 4 to 10s durations (the 4 to 6s shot cadence fits), keeps baked text the most
  stable, and produces the best layered parallax on flat art. It is 9:16/16:9 only, has no
  end frame, and needs positive-only phrasing. Fall back to `seedance-2.0` (`mode: "fast"`,
  any 4 to 15s, every aspect ratio) when you need an exact reveal landing via
  `end_image_url`, a 1:1 or other non-vertical/horizontal aspect, or a shot past 10s. For
  **real people or third-party brand logos** switch the ad to `kling-v3` (3 to 15s, native
  audio): Omni Flash and Seedance block recognizable celebrities and marks at the
  content-filter level. One video model per delivered ad.
- **Audio in clips is SFX only, no spoken words.** Paper foley (tear, rustle, whoosh, settle
  thunk) and quiet room tone. Explicitly forbid narration, dialogue, and lyrics in every
  motion prompt: the voiceover is mixed on top in Phase 7 and anything spoken in a clip
  collides with it.
- **Voiceover and music generate in-platform.** `advibly_generate_voiceover` narrates the
  script (xAI TTS: voices eve / ara / rex / sal / leo, 20+ languages, ~0.03 credits per
  1000 characters) and `advibly_generate_music` composes the instrumental bed (MiniMax
  Music 2.6, 0.3 credits per track). Phase 7 assembles VO, a static music bed, and quiet clip
  SFX in one free `advibly_render_composition` call.
- **Faithful theme, not brand recolor:** pass `on_brand: false` on every generation. The
  theme's palette is the whole point; the brand-kit board would recolor it. `brand_id` is
  still **required** on every call, and the run's `project_id` goes on every call too (together
  they file the work as one project tile in the user's library); with `on_brand`
  false neither styles the output. The brand shows through the real product photo, the
  headline copy, and one accent color, never through a stamped logo.
- **No text-overlay tool.** Every headline is baked in at image generation. If a headline
  degrades, re-roll with `num_images` (up to 4), shorten the words, or try `seedream-5-pro`
  for that one poster. Never plan to overlay text afterward.
- **Format:** ask 9:16 vertical or 16:9 horizontal up front (1:1 is possible on Seedance and
  Kling if asked). Hold one aspect across every shot.
- **Prompts cap at 2000 characters** on both generation tools. The style block plus scene
  must fit; trim vocabulary, not the layering description.
- **Tools are deferred.** Load the exact Advibly tool schemas with tool search before the
  first call each session (search "advibly generate image", "advibly generate video",
  "advibly generate voiceover", "advibly generate music", "advibly stitch videos", "advibly
  get products", "advibly upload asset", "advibly add subtitles", "advibly create project",
  "advibly update project"). Confirm parameter names
  and supported durations against what loads rather than assuming.

## The Advibly asset workflow (memorize)

An image from `advibly_generate_image` returns a public `url` you can pass straight into
`start_image_url` or `reference_image_urls` on the next call. No re-upload. If a call returns
`status: pending`, the media still renders in chat; call `advibly_get_generation`
(`wait: true`) only when you need the finished URL downstream (for keyframes feeding motion,
you always do).

To bring in a file the user owns: `advibly_upload_asset` with `source_url` (public link) or
`data_base64` (small local files), plus `brand_id`. Returns a reusable `url`.

**The one reference you carry through the whole ad is the product photo:**

- Store brands (`brand_type: "ecom_store"`): `advibly_get_products`, pick the product with the
  user, note its image URL. Other brands: a product photo from `advibly_get_assets` or an
  upload. Pass this URL in `reference_image_urls` on every poster that shows the product and
  describe it as **"the real product photo, unchanged and photoreal, composited into the
  collage"**. Do NOT pass `product_id` to the generation tools: on an image call it forces
  edit mode against the raw photo and fights the collage; on a video call it replaces your
  start frame.
- **Pre-reveal and no-product beats get no product reference.** A hook poster or problem
  poster that should not show the product yet must not carry the photo reference, or the
  model leaks the product in early.

**Logo: keep the corners clean by default.** Do not stamp a corner logo tile, do not pass the
logo into `reference_image_urls` (it makes the model invent a watermark). Cohesion comes from
the shared theme: same style block, palette arc, type treatment. If the user explicitly wants
their mark, place it on the beat(s) they name, usually the close, never all of them.

---

## PHASE 1: INTAKE

One message, only what you still need:

1. **Brand**: `advibly_list_brands`. One brand: use it. Several: ask. None: send the user to
   advibly.com/onboarding (this skill reads a brand, it cannot create one).
2. **Product** to feature and its photo URL (asset workflow above). A pure brand-story
   explainer with no hero product is fine; skip the photo and the product-reveal beat.
3. **Format**: 9:16 or 16:9 (ask; no default).
4. **The one claim**: what does this ad argue ("not all creatine is equal", "your invoices
   should chase themselves"). No angle? Propose 2 or 3 from the brand brief and let them pick.
5. **Length**: default ~32s (6 to 8 shots). 60s is the ceiling: the stitcher takes at most 12
   clips, which is exactly 12 shots at ~5s.

Once the brand is resolved, create the run's project with `advibly_create_project` (`brand_id`
plus a deliverable-shaped name like "Acme Vox explainer") and pass the returned `project_id` on
every generate call of the pipeline (bake-off posters, keyframes, clips, voiceover, music, the
final composition) so the run lands as one tile in the user's library. If the user is continuing
an earlier run, find its project with `advibly_list_projects` instead of creating a duplicate.

## PHASE 2: BEAT MAP (the one mandatory approval gate)

Read `references/beat-layer.md` first. Pick the **narrative arc** that fits the claim
(`pas`/`bab`/`aida` for ads, `how_it_works` for product explainers, `timeline` for history,
`myth_buster` to correct a belief, `man_in_hole` for transformations). Then draft the full
beat map and show it for approval before generating anything.

Rules that make the map good (details and vocab in the reference):

- **Beat 1 is a hook that lands in under 3 seconds**, headline carrying the payoff promise.
  Never spend beat 1 on setup. Pick a hook pattern (`surprising_stat`, `mistake_callout`, ...).
- **Each beat gets 2 shots**: a *wide* establishing poster carrying the headline, then a
  *detail* cut-in without it. The narration line spans both shots; the visual cuts
  mid-sentence. Shots run 4 to 6s, never past 7.
- **Product ads place the product-reveal beat** where the arc turns (the Solve in `pas`, the
  Bridge in `bab`): the real photo arrives framed by a burst, converging arrows, or a glow.
- **Camera moves are varied**: no two adjacent beats share a move, `static` is reserved for
  the payoff beat. `element_motion` is written fresh per shot to fit that scene, rich,
  several things moving.
- **Narration lines together are the ad's script.** Write them as one continuous persuasive
  read, ~2.5 to 3 words per second inside each beat's total duration.

Deliver the map as JSON the user can edit field by field:

```json
{
  "project": "acme-creatine-vox",
  "topic": "not all creatine is equal",
  "brand_id": "<id>", "product": "Acme Creatine", "product_photo_url": "<url>",
  "aspect": "9:16", "language": "en",
  "arc": "pas", "theme": "<set in Phase 3>", "image_model": "<set in Phase 3, nano-banana-2 default>",
  "video_model": "gemini-omni-flash", "motion_style": "punchy", "constraints": "strict",
  "voice": "leo", "music": "driving cinematic percussion build, warm resolve, modern ad underscore",
  "beats": [
    {
      "id": 1, "role": "hook", "title": "NOT ALL EQUAL",
      "bg": "bold red", "feel": "urgent, confrontational", "hook": "mistake_callout",
      "product": false,
      "narration": "Most creatine on that shelf is not what the label says it is.",
      "shots": [
        {"id": "a", "dur": 5, "title": true, "shot_size": "WIDE", "camera_move": "push_in",
         "scene": "a wall of near-identical grey paper supplement pouches, one torn-open gap in the grid, question-mark scraps",
         "element_motion": "the grid of pouches shivers in place, one pouch peels off the wall and tumbles, halftone dots pulse"},
        {"id": "b", "dur": 4, "title": false, "shot_size": "CLOSE", "camera_move": "parallax",
         "scene": "close cut-in of one grey pouch with a magnifying glass cut-out over its label, torn newspaper scraps behind",
         "element_motion": "the magnifying glass slides across the label, scraps drift at different depths"}
      ]
    }
  ]
}
```

Per-beat `product: true` marks which posters composite the real photo. `voice` picks the
narrator (eve: energetic / ara: warm / rex: confident / sal: smooth / leo: authoritative)
and `music` describes the instrumental bed; both are generated in Phase 7. Ask: "Approve
this beat map, or edit any field?" Only proceed on a yes.

## PHASE 3: THEME + MODEL (the user picks by eye)

Do not reuse one house style for every brand. Read `references/prompt-guide.md` §5 and pick
**3 or 4 theme presets** that fit the claim's era, culture, and tone (`american-retro`,
`swiss-modern`, `punk-zine`, `soviet-constructivist`, `wpa-propaganda`, `70s-groovy`,
`chinese-ink`, `atomic-age`, `paper-craft-cream`), or compose a custom theme from the
dimension banks when none fit. Match the topic and brand, not the language.

Then run a **bake-off**: generate beat 1's wide poster once per candidate theme (3 or 4
`advibly_generate_image` calls, one image each, `model: "nano-banana-2"`, `on_brand: false`)
and let the user pick by eye. AI proposes, the preset library is the quality floor, the human
decides.

**The bake-off also settles the image model.** nano-banana-2 is the default because it holds
the paper texture; if the winning poster's headline looks weak or the topic is type-heavy,
re-render that one theme on `gpt-image-2` (or `seedream-5-pro`) beside it and let the user
compare texture against text. Lock the model that wins and use it for every poster (see
`models-and-gotchas.md`).

If the user wants to skip the spend, describe the candidates and let them pick from the
tables, but say the bake-off gives a much better read. Set the winner as `"theme"` (and the
chosen `"image_model"`) in the beat map; its style block, palette, type, and motion amplitude
drive every prompt from here.

## PHASE 4: KEYFRAMES (the collage look)

One poster per shot, in beat order. Compose every prompt with the 5-part structure in
`references/prompt-guide.md` §1: style block (identical on every poster), scene as separate
cut-out pieces, one bold flat background color, headline baked in on `"title": true` shots
only, aspect and resolution.

```
advibly_generate_image
  prompt: <5-part collage prompt>
  brand_id: <brand id>
  project_id: <project id>
  model: <the bake-off winner, "nano-banana-2" by default>
  resolution: "2K"                               # add quality: "high" only if the model is gpt-image-2
  on_brand: false
  aspect_ratio: <9:16 or 16:9>
  reference_image_urls: [<product photo URL>]   # only on beats with "product": true
```

- **Verify each poster is a real layered collage before animating**: distinct pieces, visible
  torn edges, drop shadows, crisp headline. Re-roll here; images are cheap next to clips.
- Product posters: the photo reference plus the "unchanged and photoreal" wording. Check the
  product did not get redrawn as paper; that is the most common failure.
- The style block stays verbatim across every poster; only scene, background color, and
  headline change. That is what makes 12 shots feel like one film.
- Show the set, ask "keep or change?", re-roll only the misses.

## PHASE 5: MOTION (living collage)

Animate each approved poster with the 5-axis motion prompt from
`references/prompt-guide.md` §2: goal, camera (the beat map's `camera_move`, one move only),
element movement (the beat map's `element_motion`, rich), aesthetic preservation, feel and
color, then the stability constraints.

```
advibly_generate_video
  prompt: <5-axis motion prompt, SFX-only audio direction, no spoken words>
  brand_id: <brand id>
  project_id: <project id>
  model: "gemini-omni-flash"
  aspect_ratio: <9:16 or 16:9>
  duration: <the shot's dur, 4 to 10>
  start_image_url: <that shot's poster URL>
```

- `constraints: "strict"` in the beat map means the defect guards stay in every prompt (flat
  2D, one continuous move, no morph, text anchored). `"loose"` drops them for a bold move
  (orbit, dolly zoom, whip) on a beat that earns it; expect to re-roll.
- **Omni Flash takes positive-only wording**: convert every "no X" into a positive ("the
  camera stays locked", "the lettering stays exactly as printed"). See §2 of the prompt
  guide.
- **The dramatic looks are motion prompts, not a separate engine.** Pieces assembling from
  an empty field, confetti and paper scatter, an impact shake, a whip: all reachable by
  pushing the element-motion line, phrased for the model. Recipes and the phrasing bank are
  in `references/motion-collage.md`. Use them on the beats that earn a punch (hook, product
  reveal, payoff, ending), never every shot.
- **Exact reveal landings and the hard assemble-from-empty:** when a beat must end on a
  precise revealed frame, or you want a true bare-field-to-finished-poster build, that needs
  `end_image_url`, which only Seedance supports; run the whole ad on `seedance-2.0`
  (`mode: "fast"`) in that case and keep one model across the ad. See `motion-collage.md` §1.
- Audio direction in every prompt: paper foley only, no voiceover, no spoken words, no music
  with lyrics.
- Show each clip: keep, re-edit (same poster, adjusted motion prompt), or re-roll.

## PHASE 6: VOICEOVER + MUSIC + FINAL COMPOSITION

1. **Voiceover.** Join the beats' narration lines into one continuous read and generate it:
   ```
   advibly_generate_voiceover
     brand_id: <brand id>
     project_id: <project id>
     text: <the full narration, beats joined in order; use [pause] between beats
            when a beat's line lands short of its window>
     voice: <the beat map's voice: eve / ara / rex / sal / leo>
     language: <only when auto-detection would get it wrong>
   ```
   If the read exceeds the scene total, tighten and regenerate it; never time-stretch.
2. **Music.** Generate the bed from the beat map's `music` description:
   ```
   advibly_generate_music
     brand_id: <brand id>
     project_id: <project id>
     prompt: <the beat map's music description + tempo + "modern ad underscore">
     instrumental: true
   ```
   Tracks may run longer than the ad; composition auto-trims with a tail fade. Keep it instrumental: lyrics fight the
   narration.
3. **Compose once.** Call `advibly_render_composition` with ordered clips as `scenes` (each
   `volume: 0.3`), `voiceovers: [{source: <VO generation id>}]`, the music generation as `music`,
   the chosen `aspect_ratio`, the run's `project_id`, and `keep_scene_audio: true`. The default static music level already
   sits correctly under narration. It returns `status: pending`, `generation_id`, and `edit_url`;
   let the chat widget poll the render.
4. **Captions (optional, only after composition).** Use `advibly_get_generation` with `wait: true`
   only here to obtain the finished render, then call `advibly_add_subtitles` with a dynamic preset
   (`glide`, `fusion`, `glass`). Never caption the SFX-only cut; there is nothing to
   transcribe. Add brand words to `vocabulary` so the transcriber spells them right.
5. Deliver the result and mention the `edit_url` so the user can fine-tune the ad in the Advibly video editor.
6. **Set the project cover.** Call `advibly_update_project` with `{ project_id,
   cover_generation_id: <the final composition's generation id> }` so the project tile shows the
   finished ad.

## PHASE 8: OPTIONAL PUBLISH

If the user wants to post it: `advibly_social_list_accounts`, then `advibly_social_create_post`
with the final video. Only offer after the user has seen the finished ad.

---

## Notes and rules

- **The beat map is approved before any generation.** The story is the product; the collage
  renders it.
- **Real product stays photoreal; everything else is printed paper.** Pass the photo
  reference on product beats, forbid it on pre-reveal beats, and say "unchanged and
  photoreal" in words every time.
- **Cut every 4 to 6 seconds.** Two shots per beat, wide plus detail, narration spanning
  both. One long static poster per beat reads as dead air.
- **No two adjacent beats share a camera move; `static` is the payoff.** Anti-monotony is
  the biggest quality lever after the posters themselves.
- **Element motion is where the energy lives.** Write it per shot, rich, several elements
  moving. A hero element flying across the frame is an occasional punch, not a formula.
- **One image model and one video model per delivered ad.** Swap the whole set or nothing.
  Omni Flash is the default motion model; move the ad to Seedance only for end-frame
  reveals, non-9:16/16:9 aspects, or shots past 10s, and to Kling for real people.
- **SFX-only clips.** The voiceover and music generate in Phase 7 and mix on top; never let
  a clip speak.
- **`on_brand: false` always; `brand_id` and `project_id` always; no logo watermark by default.**
- **Self-contained prompts.** Generators have no memory of earlier calls; the style block
  travels verbatim in every image prompt.
- **On failure:** `content_rejected` means the policy blocked the prompt; rework wording, and
  if the trigger is a real person or third-party mark, move the ad to `kling-v3`.
  `insufficient_credits`: call `advibly_buy_credits` and share the checkout link. A stubborn
  reveal: switch that beat to the Seedance start-plus-end-frame route.
- **No em dashes, minimal emoji** in any copy, label, or narration you draft.

## Reference files

- `references/beat-layer.md`: the STORY layer. Narrative arc library, hook patterns, beat
  counts per duration, shot sizes, the flat-safe camera-move vocabulary, element motion, and
  the anti-monotony move-rhythm presets. Read before writing any beat map.
- `references/prompt-guide.md`: the LOOK layer. The 5-part image prompt structure, the
  dimension vocabulary banks, the 5-axis video prompt with stability anchors, the advanced
  motion vocabulary, and the theme presets with the bake-off procedure. Read before writing
  any prompt.
- `references/motion-collage.md`: the dramatic looks (pieces assembling from an empty field,
  hero fly-across, confetti and scatter, impact shake, whip) and the image-model
  background-remover recipe, all done through the MCP with no local scripts. Read before
  reaching for a punch bigger than living-poster motion.
- `references/models-and-gotchas.md`: image and video model choice, content blocks, the
  positive-phrasing rule, the asset workflow, and the final composition contract
  (a static low music bed, tail protection, VO-overrun fix, whip assembly). Read before debugging
  a weak render or the audio mix.
