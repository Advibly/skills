---
name: claymation-ad
description: >
  Turn a brand and product into a finished stop-motion claymation ad on the Advibly MCP. Lock
  a cast and narrated story, generate sequential hand-sculpted plasticine storyboard stills,
  animate them with Gemini Omni Flash by default or Seedance 2.0 as fallback, then compose
  voiceover, music, and clay foley. Trigger for "claymation ad", "Aardman-style ad",
  "stop-motion ad", "Wallace and Gromit style ad", "plasticine ad", "clay-style ad", a
  product ad in a hand-sculpted clay look, or a matching visual reference. Use even when the
  user does not name the skill but clearly wants a narrated clay stop-motion product story.
---

# Advibly Claymation Ad

Turn one brand and product into a finished **Aardman-style stop-motion claymation ad**: a
warm, narrated story where each beat is a hand-sculpted plasticine miniature that comes to
life, a quirky character arc that sells the product through a small human story. The look
anchors on **Aardman Animations** (Wallace & Gromit, Chicken Run) and **Laika** (Coraline,
Kubo): hand-sculpted clay with visible fingerprint impressions and tool marks, matte
plasticine surfaces, real knit fabric, wooden and ceramic miniature-set props, warm tungsten
light, shallow macro depth of field. Everything generates on the Advibly MCP (images, clips,
voiceover, music, and final composition).

Speak in the user's language. No em dashes anywhere in output; use periods or line breaks.
Keep on-screen copy and labels free of emoji unless asked.

## The core idea (read this first)

The claymation look and the claymation motion are **two different steps**:

1. **The look is born in the IMAGE step.** Each beat is a finished sculpted clay *still* made
   by the image model. All the claymation DNA (thumbprint impressions, tool marks, matte
   plasticine, real wool weave, hand-thrown ceramics) lives in that still. If the still is a
   smooth 3D render instead of sculpted clay, nothing downstream saves it. Re-roll cheap here
   rather than paying to animate a weak image.
2. **The motion is added after.** The video model animates the still while preserving its
   sculpted aesthetic. The #1 risk is the video model **smoothing the clay** into a glossy 3D
   render mid-clip. The prompts fight that on every axis.
3. **The Advibly twist: the product is a CLAY prop, not a photoreal object.** Unlike a UGC or
   collage ad, the product here is re-sculpted into the clay world: matte hand-painted label,
   slightly imperfect cylinder or jar, paint that looks brushed on. You pass the real product
   photo as a *reference* to copy the exact label text and shape, then describe it "rendered
   as a clay-stylized prop". Never composite the raw photo in; a glossy real bottle in a clay
   set breaks the illusion.
4. **The narration is always external.** Clips ship SFX-only (clay foley, room tone) and the
   warm storyteller voiceover is generated separately and mixed on top. Baking a narrator into
   the video model forces lip-sync compromises and gives a different voice every beat. One
   `advibly_generate_voiceover` render is one consistent voice across all 8 beats.

Two layers drive everything, each with its own reference file. Read both before writing the
beat map or any prompt:

- **STORY layer**: `references/cast-and-story.md`. The 8-beat arc, the cast-and-continuity
  sheet, category variations, the 5-beat short, narration timing.
- **LOOK layer**: `references/storyboard-prompts.md` (the stills) and
  `references/animate-prompts.md` (the motion). The prompt structures, the STYLE LOCK and
  MATERIAL DETAIL and NEGATIVE blocks, per-beat formulas, worked examples, QA checklists.

## Hard defaults (do not drift)

- **Image model:** `advibly_generate_image` with `model: "gpt-image-2"`, `quality: "high"`,
  `aspect_ratio: "9:16"`. gpt-image-2 renders the sculpted clay cleanly when the STYLE LOCK
  is verbatim, holds character identity across beats when you re-feed the prior still as a
  reference, and bakes the sculpted-clay letters of the beat-5 infographic. **Fallback to
  `nano-banana-2`** (better texture retention, up to 4K) for a specific beat whose clay
  texture keeps flattening on close-ups, or a product prop that keeps rendering glossy: switch
  only that beat, never the whole ad, because nano is slightly weaker on cross-beat identity.
  Keep one image model across the character-carrying beats.
- **Video model:** `advibly_generate_video` with `model: "gemini-omni-flash"` is the default
  for every clip: `aspect_ratio: "9:16"`, `duration: 10`, SFX-only prompt. Gemini has no end
  frame, so do not pass `end_image_url`, `mode`, or Seedance-only parameters. Use
  `seedance-2.0` as the secondary fallback only after two failed Gemini attempts on a clip or
  when the beat-7 transformation needs a controlled end frame; explain the switch and get
  approval first. With Seedance use `mode: "pro"` and `duration: 10`. If the user explicitly
  names either model, use it for every clip and never switch models automatically.
- **Smooth motion, not stop-motion judder.** AI video is smooth 24/30 fps; real stop-motion
  judders at ~12 fps. The reference clips are all smooth, so smooth is the default. Never ask a
  video model for "stop-motion judder" (it breaks the aesthetic). If the user wants the judder
  feel, pass `frame_cadence: "on_twos"` to `advibly_render_composition`; see
  `references/audio-and-gotchas.md`. This applies the temporal effect in the editor and final
  render without changing the source clips or their audio.
- **Audio in clips is SFX only, no spoken words.** Clay foley (soft press, fabric rustle,
  kettle pour, gentle settle) and quiet room tone. Explicitly forbid narration, dialogue, and
  lyrics in every motion prompt: the voiceover is mixed on top in the final step and anything
  spoken in a clip collides with it. Set no `Narrator:` line in any video prompt.
- **Voiceover and music generate in-platform.** `advibly_generate_voiceover` narrates the
  story (xAI TTS; warm storyteller `ara` is the default Aardman-tone voice, `sal` smooth,
  `leo` for a wry documentary narrator; ~0.03 credits per 1000 characters) and
  `advibly_generate_music` composes a gentle instrumental bed (0.3 credits per track). Advibly
  uses `advibly_render_composition` for the final assembly. Pick **one** narrator voice for the whole ad.
- **Faithful clay look, not brand recolor:** pass `on_brand: false` on every generation call.
  The claymation aesthetic owns its warm palette; the brand-kit board would recolor it.
  `brand_id` is still **required** on every call (it files the work in the user's library);
  with `on_brand` false it does not style the output. The brand shows through the clay product
  prop's copied label text, never through a stamped logo.
- **No text-overlay tool.** The only baked text is the beat-5 clay infographic (sculpted-clay
  letters, rendered by the image model) and any burned-in captions, which go on in the final
  step via `advibly_add_subtitles`, never in a video prompt (the negative block tells
  the model "no captions").
- **Format:** 9:16 vertical (TikTok / Reels / Shorts) is the default and rarely changes for
  this genre. Hold one aspect across every beat.
- **Prompts cap at 2000 characters** on both generation tools. The STYLE LOCK plus scene plus
  MATERIAL DETAIL must fit; trim adjectives, never the clay-texture or identity description.
- **Tools are deferred.** Load the exact Advibly tool schemas with tool search before the
  first call each session (search "advibly generate image", "advibly generate video", "advibly
  generate voiceover", "advibly generate music", "advibly stitch videos", "advibly get
  products", "advibly upload asset", "advibly add subtitles"). Confirm parameter names and
  supported durations against what loads rather than assuming.

## The Advibly asset workflow (memorize)

An image from `advibly_generate_image` returns a public `url` you pass straight into
`start_image_url`, `end_image_url`, or `reference_image_urls` on the next call. No re-upload.
If a call returns `status: pending`, the media still renders in chat; call
`advibly_get_generation` (`wait: true`) only when you need the finished URL downstream (for a
still feeding motion, or a prior still anchoring the next beat, you always do).

To bring in a file the user owns: `advibly_upload_asset` with `source_url` (public link) or
`data_base64` (small local files), plus `brand_id`. Returns a reusable `url`.

**The product photo is a clay-prop reference, not a composite:**

- Store brands (`brand_type: "ecom_store"`): `advibly_get_products`, pick the product with the
  user, note its image URL. Other brands: a product photo from `advibly_get_assets` or an
  upload. Pass this URL in `reference_image_urls` on the beats that show the product (discovery,
  transformation, resolution) and describe it as **"rendered as a clay-stylized prop with a
  matte hand-painted label, copy the exact label text from the reference, slightly imperfect
  cylinder shape"**. Do NOT pass `product_id` to the generation tools: on an image call it
  forces edit mode against the raw photo and pulls the output toward a glossy real bottle; on a
  video call it replaces your start frame.
- **Character and no-product beats get no product reference.** Beats 1 to 5 (setup through
  infographic) carry no product photo, or the model leaks the bottle in early.
- **Character continuity is the other reference chain:** re-feed the prior approved
  protagonist still as a `reference_image_url` on each new protagonist beat so the sculpted
  face holds. See Phase 3.

---

## PHASE 1: INTAKE

One message, only what you still need:

1. **Brand**: `advibly_list_brands`. One brand: use it. Several: ask which. None: send the
   user to advibly.com/onboarding (this skill reads a brand, it cannot create one).
2. **Brand context**: `advibly_get_brand` for identity, tone, and the research brief; use it
   to sharpen the narrator voice and the character. `advibly_get_brand_dossier` only if you
   need objections or voice-of-customer lines for the story.
3. **Product** to feature and its photo URL (asset workflow above). A pure brand-story
   claymation with no hero product is possible; skip the photo and the discovery beat.
4. **Product category** (decides the story variation): health/supplement, beauty/skincare,
   office/B2B, food/kitchen, or other. See `references/cast-and-story.md`.
5. **Length**: default the full **8-beat story** at 10 seconds per beat, roughly 80 seconds.
   Offer the **5-beat short** (setup, inciting, discovery, transformation, CTA, roughly 50s)
   when the user wants tighter. Use `duration: 10` for every clip unless the user explicitly
   requests another duration.
6. **Format**: 9:16 (default; rarely changed for this genre).

Do not ask for everything at once. Brand plus product plus category is enough to start; fill
the character and settings from the brand data and sensible defaults, then confirm in Phase 2.

## PHASE 2: CAST + STORY BEAT MAP (the one mandatory approval gate)

Read `references/cast-and-story.md` first. Two things get locked here and never drift after:

**A. The cast-and-continuity sheet.** Claymation ads feature two or three named characters and
one or two miniature sets. Lock all of them up front: protagonist (name the narrator uses, age
range, distinctive sculpted feature, build, eye color, outfit, posture), supporting character
(beat 3), the narrator voice persona, the primary and secondary settings, and the product as a
clay prop. Middle-aged and older characters suit the look best. Reuse the **exact same
wording** for hair, eyes, outfit, and skin texture in every beat; drift compounds otherwise.

**B. The 8-beat narrator script.** One narrator sentence per beat, written to time (~2.5 to 3
words per second so the clip is never riding silent under a short line). The protagonist drives
the whole arc; there is no anthropomorphized-problem character. The eight beats:

| Beat | Purpose | Duration |
|---|---|---|
| 1 Setup | protagonist in their everyday world, named by the narrator | 10s |
| 2 Inciting moment | close-up, they notice the problem | 10s |
| 3 Social validation | two-shot, someone else remarks on it | 10s |
| 4 Quiet despair | solo reflection at a mirror or window | 10s |
| 5 Clay infographic | a sculpted-clay chart explains the mechanism (optional) | 10s |
| 6 Discovery | the product (clay prop) is found, hand reaches for it | 10s |
| 7 Transformation | weeks pass, product used, subtle visible improvement | 10s |
| 8 Resolution + CTA | confident protagonist holds the product, clean lower third | 10s |

Drop beats 3, 4, and 5 for the 5-beat short. Beat 5 is also optional in the full arc when the
product needs no mechanism explained (most beauty/skincare). Category variations (office/B2B
cool light, food/kitchen family beats) are in the reference.

Deliver the sheet plus the beat map as JSON the user can edit field by field:

```json
{
  "project": "refirm-claymation",
  "brand_id": "<id>", "product": "refirm", "product_photo_url": "<url or null>",
  "category": "health", "aspect": "9:16", "language": "en",
  "length": "full-8", "voice": "ara", "video_model": "gemini-omni-flash", "model_source": "default",
  "music": "gentle warm music-box and soft strings, unhurried, storybook underscore, instrumental",
  "cast": {
    "protagonist": "Diane, a woman in her late 50s, shoulder-length terracotta-brown wavy plasticine hair in ribbon-strands, deep sculpted laugh lines, hooded eyelids, warm brown clay eyes; cream chunky knit cardigan over a rust-red blouse, dark wool trousers, brown leather slippers; soft rounded shoulders",
    "supporting": "Margaret, a woman in her 60s, silver curly plasticine hair, round wire glasses, sage-green cable-knit sweater",
    "primary_setting": "a small sunlit miniature kitchen: green-painted wooden cabinets, red gingham tablecloth, hand-thrown ceramic cups, copper kettle, potted herbs on the windowsill, warm tungsten window light on camera-left",
    "secondary_setting": "a neighborhood cafe: wooden tables, potted plants on shelves, hanging brass pendant lights",
    "product_prop": "a small dusty-purple cylindrical bottle labeled 'refirm' in hand-painted cream lettering, rendered as a matte clay prop"
  },
  "beats": [
    { "id": 1, "role": "setup", "dur": 10, "product": false,
      "narration": "Diane had lived in this little kitchen for thirty years." },
    { "id": 2, "role": "inciting", "dur": 10, "product": false,
      "narration": "One morning, the mirror showed her something new." }
  ]
}
```

`voice` picks the narrator (`ara` warm-storyteller default, `sal` smooth, `leo` authoritative
/ wry, `rex` confident, `eve` energetic) and `music` describes the instrumental bed; both are
generated in the final phase. `product: true` marks the beats (6, 7, 8) that carry the clay
product prop. Ask: "Approve this cast sheet and beat map, or edit any field?" Only proceed on a
yes. Script edits are free; renders cost credits.

## PHASE 3: STORYBOARD STILLS (the claymation look, generated sequentially)

Read `references/storyboard-prompts.md`. One sculpted-clay still per beat, built from the
six-block structure (STYLE LOCK, ASPECT + FRAMING, CHARACTER(S), SCENE / ACTION, MATERIAL
DETAIL, NEGATIVE). **Identity continuity is the whole game, so generate protagonist beats
sequentially, not in parallel**, each anchored on the prior approved still.

```
advibly_generate_image
  prompt: <the beat's six-block storyboard prompt>
  brand_id: <brand id>
  model: "gpt-image-2"                          # nano-banana-2 fallback for a texture-losing beat
  quality: "high"
  on_brand: false
  aspect_ratio: "9:16"
  reference_image_urls: [<prior protagonist still URL>, <product photo URL on product beats>]
```

Order and reference chain:

1. **Beat 1 hero still** first (no references, or the product photo left off). Approve it: this
   is the identity anchor for the whole ad.
2. **Beats 2, 4, 6, 7, 8** (protagonist beats) sequentially, each passing the prior approved
   protagonist still in `reference_image_urls`. Add the product photo URL on beats 6, 7, 8.
3. **Beat 3** (two-shot) attaches beat 1 or 2 as the protagonist reference; the supporting
   character is generated fresh from the cast sheet.
4. **Beat 5** (clay infographic) is independent, no character reference, and can fire in
   parallel with beat 1. Bake the sculpted-clay chart text into the prompt.

- **Verify each still is real sculpted clay before animating**: thumbprint and tool marks on
  faces and hands, matte eyes (single soft highlight, no wet Pixar shine), real wool weave (not
  painted-on stripes), hand-thrown ceramic irregularity, a product label that looks
  hand-painted. The full QA checklist is in the reference. Re-roll here; images are cheap next
  to clips.
- The STYLE LOCK and MATERIAL DETAIL blocks travel **verbatim** across every beat; only the
  framing, scene, and action change. That is what makes 8 beats feel like one film.
- **Present the storyboard to the user** in beat order, each still with its narration line.
  This is the second gate. Re-roll only the misses. Do not animate until the user approves.

## PHASE 4: MOTION (living clay)

Read `references/animate-prompts.md`. Animate each approved still with the six-block motion
structure (BEAT INTRO, SUBJECT LOCK, ACTION, CAMERA, STYLE ANCHOR, AMBIENT, CONSTRAINTS). The
SUBJECT LOCK fragments are lifted verbatim from the cast sheet.

Select the video model once before the render pass:

- **User names Gemini Omni Flash:** use `gemini-omni-flash` for every clip.
- **User names Seedance 2.0:** use `seedance-2.0` for every clip.
- **No model named:** use `gemini-omni-flash` for every standard clip. Consider Seedance only
  after two failed Gemini attempts on a clip or for a necessary beat-7 end-frame transition;
  explain why and get approval before switching that clip.

```
advibly_generate_video
  prompt: <six-block motion prompt, SFX-only ambient, NO Narrator line>
  brand_id: <brand id>
  model: <selected model>
  mode: "pro"                                    # Seedance only; omit for Gemini
  aspect_ratio: "9:16"
  duration: 10
  start_image_url: <that beat's approved still URL>
```

- `start_image_url` is the approved still. Do NOT also pass `reference_image_urls`: that
  switches video mode and the still stops being the opening frame. The
  still already carries the sculpted character and clay prop.
- **The beat-7 transformation reveal** can use `end_image_url` only when Seedance was explicitly
  selected or approved as the fallback: pass the "before" still as `start_image_url` and a
  subtly-improved "after" still as `end_image_url` for a controlled interpolation. Generate the
  "after" still in Phase 3 as a light edit of the beat-7 still (only the specific improved area
  changes). Gemini performs the transformation within the start-frame clip, without an end frame.
- **Forbidden Seedance words** (only when Seedance is selected): `cinematic`, `professional`,
  `stunning`, `8k`, `studio`, `perfect`. Substitute "stop-motion claymation film aesthetic",
  "polished hand-sculpted", "high fidelity", "evenly hand-painted".
- **The prompt describes motion, not composition** (the still owns composition): one primary
  motion plus small secondary motions with degree adverbs, one camera move (often locked with a
  subtle handheld macro breath, the miniature-set feel).
- **Every prompt carries the anti-smoothing constraints**: "preserve all clay texture from the
  start frame, matte plasticine surfaces, visible thumbprint impressions, no Pixar smoothing,
  no 3D ray-traced materials, no morphing". The video model's smoothing tendency is the #1
  failure mode; the reference file has the per-beat constraint blocks.
- **Audio direction in every prompt: clay foley only**, no voiceover, no spoken words, no
  music with lyrics. The narrator is added in the final phase.
- Show each clip. QA per `references/animate-prompts.md` (clay texture end-to-end, matte eyes,
  identity holds, label paint stays hand-applied, mirror reflections correct). Keep, re-edit
  (same still, adjusted motion prompt), or re-roll. 2-retry cap per beat; if a third attempt
  still loses clay texture, regenerate that still on `nano-banana-2` and re-animate. When Gemini
  was the default, offer Seedance only as the approved secondary fallback; never switch if the
  user explicitly selected Gemini.

## PHASE 5: VOICEOVER + MUSIC + FINAL COMPOSITION

Full recipe and gotchas in `references/audio-and-gotchas.md`.

1. **Voiceover.** Join the beats' narration lines into one continuous warm storyteller read and
   generate it:
   ```
   advibly_generate_voiceover
     brand_id: <brand id>
     text: <the full narration, beats joined in order; [pause] between beats when a line lands
            short of its window>
     voice: <the beat map's voice: ara / sal / leo / rex / eve>
     language: <only when auto-detection would get it wrong>
   ```
   The script was written to time in Phase 2. If it exceeds the scene total, tighten the lines and
   regenerate (cheap), never time-stretch the voice.
   - **Character dialogue (beat 3):** by default fold the supporting character's remark into the
     narrator's read ("Her friend leaned in. You look so rested lately, she said."). If the user
     wants a distinct second voice, generate that one line as a separate `advibly_generate_voiceover`
     call with a different `voice` and drop it at the beat-3 timestamp in the mix.
2. **Music.** Generate a gentle instrumental bed from the beat map's `music` description
   (`instrumental: true`). Composition auto-trims it with a tail fade.
3. **Choose the final cadence.** Smooth is the default. If the user requests stop-motion judder,
   set `frame_cadence: "on_twos"` on the composition call. Do not preprocess the individual clips.
4. **Compose once.** Call `advibly_render_composition` with clips as ordered `scenes` (each
   `volume: 0.3`), narration as `voiceovers`, the bed as `music`, the chosen aspect, and
   `keep_scene_audio: true`. Include `frame_cadence: "on_twos"` only when judder was requested.
   Give any separate character line its beat's cumulative offset.
   It returns `status: pending`, `generation_id`, and `edit_url`.
5. Mention the `edit_url` in final delivery so the user can fine-tune the ad in the Advibly video
   editor. On Twos appears under **Effects** and updates the preview in realtime.

## PHASE 7: CAPTIONS (optional, after the VO is mixed in)

Upload the final mix with `advibly_upload_asset` (`source_url`), then `advibly_add_subtitles`
with a preset. Two caption styles fit this genre: TikTok white-with-black-stroke, or a white-on
solid-orange rounded highlight block with a slight tilt (the handmade feel). Add the brand and
product names to `vocabulary` so the transcriber spells them right. Never caption the SFX-only
cut; there is nothing to transcribe.

## PHASE 8: OPTIONAL PUBLISH

If the user wants to post it: `advibly_social_list_accounts`, then `advibly_social_create_post`
with the final video. Only offer after the user has seen the finished ad.

---

## Notes and rules

- **The cast sheet and beat map are approved before any generation.** The story is the product;
  the clay renders it.
- **The product is a clay prop, never a photoreal composite.** Pass the photo as a reference on
  beats 6 to 8, say "rendered as a clay-stylized prop, copy the exact label text" in words, and
  forbid the reference on beats 1 to 5.
- **Generate protagonist stills sequentially, each anchored on the prior approved still.**
  Parallel generation drifts the sculpted face. Beat 5 (infographic) is the only independent
  still.
- **Reuse the exact character wording every beat.** "Terracotta-brown wavy plasticine hair in
  ribbon-strands" appears identically in every prompt; never shorten to "brown clay hair" later.
- **Keep one video model unless a fallback is approved.** Use gpt-image-2 for stills
  (nano-banana-2 fallback for a single texture-losing beat), Gemini Omni Flash for motion by
  default, and Seedance only as the approved secondary fallback or when the user explicitly
  selected it.
- **Clips are SFX-only; the narrator is external.** No `Narrator:` line in any video prompt;
  the voiceover generates in Phase 6 and mixes on top.
- **Smooth motion is the default.** Never ask a video model for stop-motion judder; apply the
  final composition's `frame_cadence: "on_twos"` effect only if requested.
- **`on_brand: false` always; `brand_id` always; no logo watermark.** The brand lives in the
  copied clay-label text.
- **Self-contained prompts.** Generators have no memory of earlier calls; the STYLE LOCK and
  MATERIAL DETAIL blocks travel verbatim in every image prompt, the SUBJECT LOCK fragments in
  every motion prompt.
- **On failure:** `content_rejected` means the policy blocked the prompt; rework wording (avoid
  real people and third-party marks). `insufficient_credits`: `advibly_buy_credits`, share the
  checkout link. Clay flattening on a beat: re-roll the still on `nano-banana-2` and re-animate.
- **No em dashes, minimal emoji** in any copy, label, or narration you draft.

## Honest limits

- **Clay smoothing is the constant fight.** Video models drift sculpted clay toward glossy 3D
  render mid-clip. The prompts guard it hard, but watch every clip end-to-end and re-roll the
  ones that flatten. nano-banana-2 stills survive animation better on close-ups.
- **Identity drift across beats.** The sequential-generation-with-reference chain holds the
  face, but a beat can still drift; the fix is usually regenerating that still (anchored on the
  prior), not the motion prompt.
- **Product label text garbles** when the prop is small or the scene busy. Keep the clay bottle
  large and close (30 to 40 percent of frame) on discovery and resolution beats; copy the label
  text exactly in the prompt.
- **Complex hand-object interaction warps.** One simple action per beat: reach, pick up, rotate,
  set down. Never a two-handed manipulation.
- **The narrator voice is one of five xAI voices**, not a specific casting. `ara` is the closest
  warm-storyteller match; pick one and keep it across the whole ad.

## Reference files

- `references/cast-and-story.md`: the STORY layer. The 8-beat arc in full, the
  cast-and-continuity sheet template, category variations, the 5-beat short, narration voice
  and timing. Read before writing any beat map.
- `references/storyboard-prompts.md`: the LOOK layer for stills. The six-block image prompt
  structure, the STYLE LOCK / MATERIAL DETAIL / NEGATIVE blocks, per-beat formulas with worked
  examples, the cross-beat continuity rules, and the image QA checklist. Read before writing
  any storyboard prompt.
- `references/animate-prompts.md`: the LOOK layer for motion. Gemini-first model selection,
  Seedance fallback rules, reusable SUBJECT LOCK fragments, per-beat animation formulas, the
  anti-smoothing constraint blocks, and the per-clip QA checklist. Read before writing any motion
  prompt.
- `references/audio-and-gotchas.md`: the voiceover voice map and timing, the music brief, the
  final composition contract (static music, automatic tail fade, VO-overrun fix), the
  optional final-render On Twos effect, captions, and the model / failure-mode gotchas. Read before
  the audio mix or debugging a weak render.
