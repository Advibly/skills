---
name: advibly-explainer-videos
description: >
  Turn a brand topic or product angle into a narrated animated explainer on the Advibly MCP
  using one of ten visual styles: cinematic 2D, gouache, whiteboard doodle, pixel art,
  claymation, felted wool, cozy kawaii, low-poly 3D, papercraft, or mixed-media collage.
  Recommend a style, approve a beat map, generate styled keyframes, animate 4 to 6-second shots
  with Gemini Omni Flash by default or Seedance for exact end-frame landings, then compose
  voiceover, music, and SFX. Trigger for "explainer video", "animated explainer", "explainer
  ad", "explain my product in a video", "turn this topic into a short video", or requests and
  references matching any supported style. Use even without the word "skill" when the user
  clearly wants a short narrated animated explainer in a distinct art style.
---

# Advibly Explainer Videos

Turn one brand topic or angle into a finished **narrated animated explainer ad** in a chosen
visual style. One shared pipeline drives every style; the look, motion cadence, typography
treatment, and audio character all come from a **style cartridge** in `references/styles/`.
Everything generates and assembles on the Advibly MCP. The optional on-twos snap remains a
clip-level preprocessing pass before composition.

Speak in the user's language. No em dashes anywhere in output; use periods or line breaks.
Keep labels and on-screen copy free of emoji unless asked.

## The core idea (read this first)

This skill is **one pipeline, ten cartridges**. The pipeline (intake, beat map, keyframes,
motion, stitch, audio) never changes. Everything that makes a claymation look like clay and a
pixel-art look like a SNES game lives in the chosen style file, which carries:

1. **The look** is born in the IMAGE step. Each style file has an `image_style_block` (the
   exact style paragraph you repeat verbatim on every keyframe) and an `image_negative_prompt`.
   If the keyframe is not convincingly in-style, nothing downstream saves it. Re-roll cheap here.
2. **The motion** is added after. Each style file has a `motion_prompt_dna` (positive-phrased
   for gemini-omni-flash) and a `motion.cadence`. A style either runs **smooth 24fps** (low-poly,
   3D mix, papercraft) or **on-twos ~12fps stop-motion** (claymation, fluffy toy, whiteboard,
   gouache, mixed-media, pixel). The stop-motion styles get a step-frame snap on each clip BEFORE
   composition; the video model always renders too smooth to hold 12fps on its own.
3. **The audio** completes it. Each style file has an `audio_recipe` (voice direction, one
   music prompt, one SFX description). Clips are SFX-only; the narration and music are generated
   separately and mixed on top.

**One style per delivered video. One image model and one video model per delivered video** (the
style file names them). Never mix styles or models within one explainer.

**Style, not story: you invent the idea.** Each style file defines an art direction and captures
only a style's **look, motion, typography, and audio**. Its **content is not a template**: any
subject, characters, story, on-screen words, the `example_subject_only` line, and any specific
creature or scene named inside fields like
`character_design`, `environment_design`, or `vfx` are illustrations of the technique, not
things to reproduce. For every brief you design a **fresh, original concept, cast, and script**
that fits the brand and topic. Read `character_design` as construction logic (how the style
builds a character: proportions, how eyes and edges are made) and then invent your own character
with it. You have full creative freedom on the idea; the style file only governs how it looks and
sounds. Two explainers in the same style for two different brands should share a look and share
nothing else.

Two layers you always read before generating:

- **STYLE layer**: `references/styles/<style>.md`. The chosen cartridge. All the look, motion,
  typography, and audio DNA for that style. Read the whole file once the style is picked.
- **STORY layer**: `references/story-and-beats.md`. Narrative arcs, hook patterns, beat and
  shot cadence, cut rhythm. Style-agnostic. Read before writing any beat map.

Two shared references:

- `references/style-catalog.md`: the menu of ten styles with one-line signatures, the
  recommend-from-brand logic, and the quick model / cadence / voice table. Read at style-pick.
- `references/pipeline-and-audio.md`: the Advibly asset workflow, the model matrix, the
   clip-level step-frame snap, `advibly_render_composition`,
  captions, and failure modes. Read before the keyframe, motion, or audio steps.

## Hard defaults (do not drift)

- **The style file is law.** Once a style is picked, its `image_style_block`,
  `image_negative_prompt`, `recommended_image_model`, `motion_prompt_dna`,
  `recommended_video_model`, `motion.cadence`, `typography`, `transitions_and_cuts`, and
  `audio_recipe` drive every call. Do not substitute your own art direction.
- **Faithful style, not brand recolor:** pass `on_brand: false` on every generation. The
  style's palette is the whole point; the brand-kit board would recolor it. `brand_id` is
  still **required** on every call (it files the work in the user's library); with `on_brand`
  false it does not style the output. The brand shows through the topic, the product, the
  headline copy, and one accent color, never a stamped logo.
- **The product is re-rendered INTO the style by default.** Unlike a photoreal-product ad,
  these styles remake everything in their medium: the product becomes clay, a pixel sprite,
  felt, a paper cut-out. Pass the product photo in `reference_image_urls` on the shots that
  feature it and describe it as "re-rendered as [the style's medium], keeping the silhouette
  and label readable". The exceptions are the collage styles (**mixed-media** uses a real
  greyscale photo cut-out; a real-product composite is on-style there) and any case the user
  asks for a real product. Never pass `product_id` to a generation tool (it forces edit mode
  on images and replaces the start frame on video).
- **Image model:** the one named in the chosen style's `recommended_image_model`
  (`gpt-image-2` with `quality: "high"` for text-heavy / spec-faithful styles, `nano-banana-2`
  at `2K` for the organic-texture styles: clay, felt, gouache). One model across every keyframe.
- **Video model:** the one named in the style's `recommended_video_model`. `gemini-omni-flash`
  is the default (4 to 10s, 9:16 or 16:9, positive-phrasing only, no end frame, most stable
  baked text). Switch to `seedance-2.0` (`mode: "pro"`, start+end frame, any aspect, up to 15s)
  when a shot needs an exact end-frame landing (papercraft's deep z-tunnel), a non-standard
  aspect, or a shot past 10s. Use `kling-v3` only if the ad must show a recognizable real
  person or third-party mark. One video model per delivered ad.
- **Shot duration:** recommend **4 to 6 seconds for every generated shot**. Omni Flash has a
  hard 4-second minimum, so never encode a shorter shot even when a style calls for fast cuts.
  Express a faster style through action beats, internal transitions, and editing within a 4 to
  6-second clip. Treat 7 to 10 seconds as an explicit exception for a beat that genuinely needs
  more room, not the default.
- **Cadence and the step-frame snap:** if the style's `motion.cadence` is on-twos / ~12fps
  stop-motion, apply the `fps=12,fps=24` snap to every clip BEFORE composition (recipe in
  `pipeline-and-audio.md`). Never ask the video model for 12fps; it renders smooth and the snap
  is what sells the stop-motion. Smooth-24fps styles skip the snap.
- **Audio in clips is SFX only, no spoken words.** Forbid narration, dialogue, and lyrics in
  every motion prompt; the voiceover is mixed on top and anything spoken in a clip collides
  with it.
- **Voiceover and music generate in-platform**, then assemble with
  `advibly_render_composition`. Map the style's voice character to one of the five
  xAI voices (eve / ara / rex / sal / leo, table in `pipeline-and-audio.md`); pick one voice
  for the whole ad.
- **Baked text vs captions.** Some styles bake headline text into the world (clay letters,
  marker handwriting, a pixel banner, a gold-foil paper ribbon, a sticky note) via the image
  model; the style file's `typography.headline_treatment` says how. Advibly has no
  text-overlay tool, so in-world headlines are always baked at image generation. Spoken-word
  subtitle captions (optional) are added at the end with `advibly_add_subtitles`.
- **Format:** ask 9:16 vertical or 16:9 horizontal up front; **default 16:9** (these styles are shown at 16:9). Hold one aspect across every shot.
- **Prompts cap at 2000 characters** on both generation tools. The style block plus scene must
  fit; trim the scene, never the style block.
- **Tools are deferred.** Load the Advibly tool schemas with tool search before the first call
  each session (search "advibly generate image", "advibly generate video", "advibly generate
  voiceover", "advibly generate music", "advibly stitch videos", "advibly compose video",
  "advibly get products", "advibly upload asset", "advibly add subtitles", "advibly list
  brands"). Confirm parameter names and supported durations against what loads.

## The Advibly asset workflow (memorize)

An image from `advibly_generate_image` returns a public `url` you pass straight into
`start_image_url`, `end_image_url`, or `reference_image_urls` on the next call. No re-upload. If
a call returns `status: pending`, the media still renders in chat; call `advibly_get_generation`
(`wait: true`) only when you need the finished URL downstream (feeding a keyframe into motion,
you always do). Full detail in `references/pipeline-and-audio.md`.

---

## PHASE 1: INTAKE

One message, only what you still need:

1. **Brand**: `advibly_list_brands`. One brand: use it. Several: ask. None: send the user to
   advibly.com/onboarding (this skill reads a brand, it cannot create one).
2. **Topic / claim**: what does this explainer teach or argue ("how our refrigerator keeps food
   fresh", "why creatine timing matters"). No angle? Propose 2 or 3 from the brand brief.
3. **Product** to feature and its photo URL (asset workflow). A pure topic explainer with no
   hero product is fine; skip the photo and the product beat.
4. **Format**: 9:16 or 16:9 (ask; default 16:9).
5. **Length**: default ~15 to 30s. 60s is the ceiling (the stitcher takes at most 12 clips).

## PHASE 2: STYLE PICK (the first gate)

Read `references/style-catalog.md`. **Recommend one style** that fits the brand, topic, and
audience, with a one-line reason (a wellness app leans cozy-kawaii or felt; a security or gaming
topic leans pixel or cinematic-2D; a kids or origin story leans low-poly or papercraft; a
data-heavy explainer leans mixed-media). Then **show the full menu of ten** so the user can
override by eye. Confirm the pick before anything else.

Optional bake-off: if the user is unsure between two styles, generate beat 1's first keyframe in
each (one `advibly_generate_image` per candidate) and let them choose by eye. AI proposes, the
human decides.

Once picked, **read the full `references/styles/<style>.md`**. That cartridge now drives every
prompt.

## PHASE 3: BEAT MAP (the mandatory approval gate)

Read `references/story-and-beats.md`. Pick the **narrative arc** that fits the topic
(`how_it_works` for product explainers, `pas`/`bab`/`aida` for hard-sell ads, `timeline` for
history, `myth_buster` to correct a belief, `man_in_hole` for transformations). Draft the full
beat map and show it for approval before generating anything past the style bake-off.

Use **4 to 6 seconds per generated shot**. Pull the visual rhythm from the chosen style's
`transitions_and_cuts.cut_style`: fast styles pack several action beats or internal cuts into the
clip, held styles sustain one move, and whiteboard pans across a board instead of hard-cutting.

Rules that make the map good (detail in the reference):

- **Beat 1 is a hook that lands in under 3 seconds**, carrying the payoff promise. Never spend
  beat 1 on setup.
- **Narration lines together are the script.** Write them as one continuous read, timed to the
  style's `voiceover.pace_words_per_sec` inside each beat's duration.
- **Camera and element motion are written per shot** from the style's vocabulary (its
  `camera_language` and `motion.signature_moves`). No two adjacent beats share a camera move.
- **Mark which shots bake an in-world headline** (per the style's `typography`) and which show
  the product.

Deliver the map as editable JSON (schema in `story-and-beats.md`): `project`, `topic`,
`brand_id`, `product`, `product_photo_url`, `style`, `aspect`, `language`, `arc`, `voice`,
`music`, and a `beats[]` array with per-shot `dur`, `shot_size`, `camera_move`, `scene`,
`element_motion`, `headline` (baked text or null), `product` (bool), and `narration`. Ask:
"Approve this beat map, or edit any field?" Proceed only on a yes.

## PHASE 4: KEYFRAMES (the look)

One keyframe per shot, in beat order. Every prompt = the style's `image_style_block` verbatim +
the shot's `scene` + the baked `headline` (only on headline shots, treated per the style's
`typography.headline_treatment`) + aspect. Pass the style's `image_negative_prompt`.

```
advibly_generate_image
  prompt: <image_style_block verbatim> + <scene> + <baked headline if any>
  brand_id: <brand id>
  model: <style.recommended_image_model>
  resolution: "2K"            # or quality: "high" if the model is gpt-image-2
  on_brand: false
  aspect_ratio: <9:16 or 16:9>
  negative_prompt: <style.image_negative_prompt>   # if the tool accepts it; else fold into prompt
  reference_image_urls: [<product photo URL>]      # only on product shots
```

- **Verify each keyframe is convincingly in-style before animating** (the style file's
  `failure_modes` list the exact tells to check: clay must show fingerprints, pixel must have no
  anti-aliasing, felt must show stray fibers, papercraft must show cast drop-shadows). Re-roll
  here; images are cheap next to clips.
- The style block stays verbatim across every keyframe; only scene and headline change. That is
  what makes the shots read as one film.
- Product shots: the photo reference plus the "re-rendered as [medium]" wording (or the real
  photo cut-out for mixed-media). Check the product kept its silhouette and label.
- Show the set, ask "keep or change?", re-roll only the misses.

## PHASE 5: MOTION (animate each keyframe)

Animate each approved keyframe with the style's `motion_prompt_dna` + the shot's `camera_move`
and `element_motion` + SFX-only audio direction.

```
advibly_generate_video
  prompt: <motion_prompt_dna> + <this shot's camera_move + element_motion> + <SFX-only, no spoken words>
  brand_id: <brand id>
  model: <style.recommended_video_model>
  aspect_ratio: <9:16 or 16:9>
  duration: <the shot's dur, recommend 4 to 6; Omni Flash supports 4 to 10>
  start_image_url: <that shot's keyframe URL>
  # end_image_url: <only on seedance-2.0, for an exact landing>
```

- **Omni Flash takes positive-only wording**: the style's `motion_prompt_dna` is already phrased
  this way. Convert any "no X" you add into a positive ("the texture stays exactly as printed").
- **Do not ask for 12fps.** The stop-motion cadence is added by the step-frame snap in Phase 5.5,
  not the video model. Ask the model only for the style's element and camera motion.
- Audio direction in every prompt: the style's SFX character only, no voiceover, no spoken
  words, no music with lyrics.
- Show each clip: keep, re-edit (same keyframe, adjusted motion prompt), or re-roll. Shading /
  3D creep and texture-smoothing are the top failures; the style's `failure_modes` list the guard.

## PHASE 5.5: STEP-FRAME EACH CLIP (conditional)

Only if the style's `motion.cadence` is on-twos / ~12fps, apply the step-frame snap to every
individual approved clip before Phase 6. The composition tool cannot decimate frames.
Smooth-24fps styles skip it.

## PHASE 6: VOICEOVER + MUSIC + FINAL MIX

Follow `pipeline-and-audio.md`. In short:

1. **Voiceover: one line PER SHOT, never one combined read.** Same voice throughout, via
   `advibly_generate_voiceover`. This is the sync rule: a single continuous read laid from t=0
   drifts once any line runs off its shot length, and a read shorter than the video leaves the
   payoff shot silent. Write the whole script, split it at the shot boundaries, and generate each
   line separately. Map the style's `voiceover` character to a voice from the table. Probe each
   line against its shot's duration; tighten and regenerate any overrun, never time-stretch.
2. **Music**, one instrumental bed from the style's `audio_recipe.music_prompt` via
   `advibly_generate_music` (`instrumental: true`). A second bed only if the arc clearly turns.
3. **Compose once.** Call `advibly_render_composition` with ordered clips as `scenes` (use
   `volume: 0.2` for clip SFX), one `voiceovers` entry per shot with `start_seconds` equal to its
   cumulative offset, the bed as `music`, the chosen aspect, and `keep_scene_audio: true`. **Leave
   `music_volume` unset.** The renderer ducks the bed against the narration automatically (it dips
   to ~35% while a line plays and swells back between them), so a hand-set level is not needed and
   a low one (0.08) would duck into inaudibility. It returns `status: pending`,
   `generation_id`, and `edit_url`; let the chat widget poll it.
4. Mention the `edit_url` in final delivery so the user can fine-tune the ad in the Advibly video editor.

## PHASE 7: CAPTIONS (optional)

Only after the VO is mixed in: upload the final with `advibly_upload_asset` (`source_url`), then
`advibly_add_subtitles` with a dynamic preset. Never caption the SFX-only cut. Add brand and
product names to `vocabulary`. Some styles already carry a caption band in their design; match it.

## PHASE 8: OPTIONAL PUBLISH

If the user wants to post it: `advibly_social_list_accounts`, then `advibly_social_create_post`
with the final video. Only offer after the user has seen the finished ad.

---

## Notes and rules

- **The style pick and the beat map are the two gates.** Pick the style, approve the story, then
  generate.
- **The style file is the single source of truth for the look, motion, typography, and audio.**
  Pull its fields; do not improvise the art direction.
- **The style file is NOT a source for the story.** Invent an original concept, cast, and script
  every time. Never lift a style file's example subject or characters (its `example_subject_only` line, or a creature named only to illustrate the look); those are technique illustrations. Same style, different brand should share nothing but
  the look.
- **One style, one image model, one video model per delivered ad.** Swap the whole set or nothing.
- **Faithful, not on-brand:** `on_brand: false` always, `brand_id` always, no logo watermark by
  default.
- **Stop-motion styles get the `fps=12,fps=24` snap on each clip before composition; smooth styles do not.** The
  video model never holds 12fps on its own.
- **SFX-only clips.** VO and music generate separately and mix on top; never let a clip speak.
- **Self-contained prompts.** Generators have no memory of earlier calls; the style block travels
  verbatim in every image prompt.
- **On failure:** `content_rejected` means the policy blocked the prompt; rework wording, and move
  to `kling-v3` only if the trigger is a real person or third-party mark. `insufficient_credits`:
  `advibly_buy_credits` and share the checkout link. A stubborn keyframe: re-roll, and switch the
  image model only per the style file's guidance (usually to nano-banana-2 for texture). A stubborn
  clip: two Omni Flash retries, then that style's Seedance route if it needs an end frame.
- **No em dashes, minimal emoji** in any copy, label, or narration you draft.

## Reference files

- `references/style-catalog.md`: the ten-style menu, one-line signatures, recommend-from-brand
  logic, and the quick model / cadence / voice / text-treatment table. Read at style-pick.
- `references/styles/<style>.md`: the ten cartridges. Each holds the full look, motion,
  typography, and audio DNA plus paste-ready `image_style_block`, `image_negative_prompt`,
  `motion_prompt_dna`, `audio_recipe`, and `failure_modes`. Read the whole chosen file after the
  pick.
- `references/story-and-beats.md`: the narrative arc library, hook patterns, beat and shot
  cadence, the beat-map JSON schema, and anti-monotony rules. Read before writing any beat map.
- `references/pipeline-and-audio.md`: the asset workflow, the image and video model matrix, the
  clip-level step-frame snap, `advibly_render_composition`, captions, and failure modes. Read before the
  keyframe, motion, or audio steps.
