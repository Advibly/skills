---
name: stickman-animation
description: >
  Turn a brand into a finished 2D stick-figure comic ad on the Advibly MCP. Invent and approve
  an original concept and beat list, generate consistent flat black-outline storyboard stills
  on a white void with restrained brand accents, animate them with Gemini Omni Flash by default
  or Seedance 2.0 as fallback, apply an on-twos snap in the final composition, then compose voiceover, music, and comic
  SFX. Trigger for "stickman ad", "stick figure ad", "stick-figure animation", "2D stick
  figure explainer", "doodle animation ad", "minimalist line animation ad", "animate a stick
  figure", or a reference with the flat black stick-figure comic look. Use even without the word
  "skill" when that visual treatment is clearly requested.
---

# Advibly Stickman Animation Ad

Turn a brand into a finished **2D stick-figure comic ad**. This skill **locks the STYLE and lets
you invent the STORY** for each brand: you design an original narrated concept (a punchy
problem-to-solution pitch, a one-joke gag, a visual metaphor made literal, a running gag, a
slice-of-life, whatever fits the brief) and render it in the fixed stickman look. The look anchors
on **minimalist vector stick-figure comic animation**: uniform clean black outlines on a pure white
void, flat cartoon fills with zero shading, snappy limited animation, comic-book VFX, and a strict
two-accent color system. Everything generates on the Advibly MCP (images, clips, stitch, voiceover,
music and composition), including the final-render On Twos effect.

Speak in the user's language. No em dashes anywhere in output; use periods or line breaks.
Keep on-screen copy and labels free of emoji unless asked.

## The core idea (read this first)

The stick-figure look and the stick-figure motion are **two different steps**, and this genre
inverts two rules the other Advibly video skills follow. Read both twists before writing anything.

1. **The look is born in the IMAGE step.** Each beat is a finished flat 2D vector *still* made by
   the image model: uniform black outlines, a plain circle-head stick figure, a pure white
   background, and no shading anywhere. All the comic DNA (the characters, any recurring element, the accent
   glow, comic VFX) lives in that still. If the still comes back as a 3D
   render, a shaded cartoon, or a realistic figure on a gray studio backdrop, nothing downstream
   saves it. Re-roll cheap here rather than paying to animate a weak image.
2. **The motion is added after.** The video model animates the still while preserving its flat
   linework. The #1 risk is the video model **adding volume, shadows, or 3D shading** and
   **morphing the linework** mid-clip. The prompts fight that on every axis.
3. **TWIST 1: invent the story fresh every time; this skill locks the STYLE, not the plot.** The
   look below is fixed (that is what "stickman style" means), but the concept, characters, setting,
   beat count, and arc are yours to design bespoke for each brand. Do NOT default to a stock
   template. One device this style is famous for is an **anthropomorphized problem as a character**
   (a gray gremlin, a nagging blob, a literal "mood cloud" that rides on the head), but it is one
   option among many, not a requirement. Reach for it when it fits; invent something else when it
   does not. `references/cast-and-story.md` is a creative TOOLKIT (principles plus a device menu),
   not a script to fill in. Any worked example in these files is ONE illustration; treat it as a
   demonstration of the method, never as the thing to reproduce.
4. **TWIST 2: the brand color is the whole point of the accent.** The piece is near-monochrome:
   pure white, black line art, and exactly **two symbolic accents**: GRAY = the problem, and the
   **brand's primary color** = the product, its energy, its glow, its lightning, and the final
   burst. That single accent color is the brand-color slot (red in the classic reference). Still
   pass `on_brand: false` (the brand kit would recolor and stamp a logo); you inject the brand
   color manually as the accent, and nothing else in frame is ever colored.
5. **The product is a FLAT 2D PROP, not a photoreal object.** The product is re-drawn as a simple
   flat cartoon icon in the brand color with the label/logo text copied from the real photo. You
   pass the real product photo as a *reference* to copy the exact text and silhouette, then
   describe it "redrawn as a flat 2D vector cartoon icon". Never composite the raw photo in; a
   glossy real bottle in a flat line world breaks the illusion.
6. **The narration is always external.** Clips ship SFX-only (zaps, pops, thunder, comic
   explosion, room tone) and the narrator voiceover is generated separately and mixed on
   top. Baking a narrator into the video model forces a different voice every beat and lip-sync
   compromises. One `advibly_generate_voiceover` render is one consistent voice across every beat.

Two layers drive everything, each with its own reference file. The LOOK is locked; the STORY is
invented per brief. Read both before designing the concept or writing any prompt:

- **STORY toolkit** (open, you invent): `references/cast-and-story.md`. Storytelling principles, a
  menu of narrative devices and structures, how to design bespoke characters, settings, and a beat
  list for THIS brand, plus narration timing. It is a toolbox, not a template. The energy-drink arc
  inside it is one labeled illustration, not the shape to copy.
- **LOOK layer** (locked, reuse verbatim): `references/storyboard-prompts.md` (the stills) and
  `references/animate-prompts.md` (the motion). The generic per-shot prompt scaffold, the STYLE LOCK
  and COLOR SYSTEM and NEGATIVE blocks, the anti-3D constraints, QA checklists. These hold constant
  no matter what story you invent.

## Hard defaults (do not drift)

- **Image model:** `advibly_generate_image` with `model: "gpt-image-2"`, `quality: "high"`,
  `aspect_ratio: "9:16"`. gpt-image-2 renders clean flat vector linework when the STYLE LOCK is
  verbatim, holds the line weight and any recurring silhouette across beats when you re-feed the
  first style-plate still as a reference, and bakes the halftone POW / slogan burst. **Fallback
  to `nano-banana-2`** for a specific beat whose linework keeps picking up shading or whose recurring
  silhouette drifts: switch only that beat, never the whole ad.
- **Video model:** `advibly_generate_video` with `model: "gemini-omni-flash"` is the default for
  every clip: `aspect_ratio: "9:16"`, `duration: 8` (10 for the transformation beat), SFX-only
  prompt. Gemini has no end frame, so do not pass `end_image_url`, `mode`, or Seedance-only
  parameters. Use `seedance-2.0` as the secondary fallback only after two failed Gemini attempts
  on a clip or when a transformation beat needs a controlled before/after end frame; explain
  the switch and get approval first. With Seedance use `mode: "pro"`. If the user explicitly names
  either model, use it for every clip and never switch models automatically.
- **Snappy limited animation, not smooth 24fps.** This genre's signature is pose-to-pose limited
  animation on twos (~12 fps), the opposite of the claymation skill. AI video renders smooth, so
  **generate smooth, then pass `frame_cadence: "on_twos"` to `advibly_render_composition`** to get
  the authentic stick-figure snap. This is default-ON for this genre (see
  `references/audio-and-gotchas.md`); skip it only if the user wants fully smooth motion. Never ask
  the video model itself for "12fps" or "choppy" (it degrades the render); the snap is a post step.
- **Audio in clips is SFX only, no spoken words.** Comic foley (zap, pop, whoosh, thunder,
  gulp, comic explosion) and quiet room tone. Explicitly forbid narration, dialogue, and lyrics in
  every motion prompt: the voiceover is mixed on top in the final step and anything spoken in a
  clip collides with it. Set no `Narrator:` line in any video prompt.
- **Voiceover and music generate in-platform.** `advibly_generate_voiceover` narrates the story
  (xAI TTS; the punchy announcer `leo` is the default for this genre, `rex` for a harder-sell
  read, `ara` for a warmer read; ~0.03 credits per 1000 characters). `advibly_generate_music`
  composes the bed. The signature audio move is **two beds**: a tense ambient drone under beats 1
  to 6, hard-cutting to an upbeat energetic beat at the transformation (beat 7). Generate two
  short tracks and splice them at the transformation timestamp in the mix. Advibly has no compose tool
  uses `advibly_render_composition` for final assembly. Pick **one** narrator voice for the whole ad.
- **Two-accent color, brand color injected manually:** pass `on_brand: false` on every generation
  call. `on_brand: true` would recolor the whole scene from the brand kit and stamp a logo, which
  kills the black-and-white line look. Instead you read the brand's primary color in Phase 1 and
  write it into every prompt as the single accent (product + energy + glow + lightning + POW).
  `brand_id` is still **required** on every call, and the run's `project_id` rides along on every
  call (together they file the work as one project tile in the user's library). If the
  brand has no clear primary color, default the accent to a bold energetic red.
- **No text-overlay tool.** The only baked text is the beat-9 POW / slogan burst and the product
  logo (rendered by the image model) plus any burned-in captions, which go on in the final step
  via `advibly_add_subtitles`, never in a video prompt (the negative block tells the
  model "no captions").
- **Format:** 9:16 vertical (TikTok / Reels / Shorts) is the default and rarely changes for this
  genre. Hold one aspect across every beat.
- **Prompts cap at 2000 characters** on both generation tools. The STYLE LOCK plus scene plus
  COLOR SYSTEM must fit; trim adjectives, never the flat-linework or color-system description.
- **Tools are deferred.** Load the exact Advibly tool schemas with tool search before the first
  call each session (search "advibly generate image", "advibly generate video", "advibly generate
  voiceover", "advibly generate music", "advibly stitch videos", "advibly get products", "advibly
  upload asset", "advibly add subtitles", "advibly create project", "advibly update project").
  Confirm parameter names and supported durations against
  what loads rather than assuming.

## The Advibly asset workflow (memorize)

An image from `advibly_generate_image` returns a public `url` you pass straight into
`start_image_url` or `reference_image_urls` on the next call. No re-upload. If a call returns
`status: pending`, the media still renders in chat; call `advibly_get_generation` (`wait: true`)
only when you need the finished URL downstream (for a still feeding motion, or the beat-1 style
plate anchoring the next beat, you always do).

To bring in a file the user owns: `advibly_upload_asset` with `source_url` (public link) or
`data_base64` (small local files), plus `brand_id`. Returns a reusable `url`.

**The product photo is a flat-prop reference, not a composite:**

- Store brands (`brand_type: "ecom_store"`): `advibly_get_products`, pick the product with the
  user, note its image URL. Other brands: a product photo from `advibly_get_assets` or an upload.
  Pass this URL in `reference_image_urls` **only on the beats that actually show the product** and
  describe it as **"redrawn as a flat 2D vector cartoon icon in {BRAND_COLOR} with a uniform black
  outline, copy the exact label/logo text from the reference, simple flat silhouette, no photoreal
  shading"**. Do NOT pass `product_id` to the generation tools: on an image call it forces edit mode
  against the raw photo and pulls the output toward a glossy real product; on a video call it
  replaces your start frame.
- **Beats where the product is not in frame get no product reference,** or the model leaks it in
  early.
- **Style continuity is the other reference chain:** re-feed the approved first **style plate**
  (the character and any recurring element, at the correct uniform line weight) as a
  `reference_image_url` on each new beat so the linework and silhouettes hold. See Phase 3.

---

## PHASE 1: INTAKE

One message, only what you still need:

1. **Brand**: `advibly_list_brands`. One brand: use it. Several: ask which. None: send the user to
   advibly.com/onboarding (this skill reads a brand, it cannot create one).
2. **Brand context**: `advibly_get_brand` for identity, tone, and the research brief. **Read the
   brand's primary color here**; it becomes the single accent ({BRAND_COLOR}) for the whole ad. Use
   the brief to sharpen the announcer read and the pain point. `advibly_get_brand_dossier` only if
   you need objections or voice-of-customer lines for the story.
3. **Product or subject** to feature and its photo URL (asset workflow above). A pure brand-story
   stickman ad with no hero product is also valid.
4. **The angle or idea**, if the user has one (a pain point, a benefit, a feeling, a use case, a
   joke, a "what if"). If they do not, you propose one in Phase 2 from the brand brief. This seeds
   the story but does not dictate a template.
5. **Length**: whatever fits the idea, commonly ~20 to 70 seconds. Pick a beat count that serves the
   story (a tight 4-beat gag or a fuller 8-beat arc), not a fixed number. Clip durations run ~5 to
   10s each (Gemini caps at 10s).
6. **Format**: 9:16 (default; rarely changed for this genre).

As soon as the brand is resolved, create the run's project with `advibly_create_project`
(`brand_id` plus a deliverable-shaped name like "Acme stickman ad") and pass the returned
`project_id` on every generate call of the pipeline (stills, clips, voiceover, music, the final
composition) so the whole run lands as one tile in the user's library. If the user is continuing
an earlier run, find its project with `advibly_list_projects` instead of creating a duplicate.

Do not ask for everything at once. Brand plus subject plus a rough angle is enough to start; you
invent the concept, characters, setting, and beats in Phase 2.

## PHASE 2: INVENT THE STORY, THEN LOCK THE CAST (the one mandatory approval gate)

**You are the creative director. Design a concept bespoke to THIS brand.** Read
`references/cast-and-story.md` for the storytelling principles and the device menu, then invent an
original idea. Do not reach for a bed, a mood cloud, a commute, or a "problem to solution" energy
arc just because they appear in the example. Ask: what is the sharpest, most watchable way to make
THIS brand's point in flat stick-figure comedy? A relatable slice-of-life, an absurd exaggeration,
a visual metaphor made literal, a running gag, a fast before/after, a day-in-the-life, a mock
demo, a "what people think vs what actually happens." Let the brand's brief, audience, and product
truth drive it. Then commit to two things that stay fixed once approved:

**A. The cast and world.** Whatever recurring characters, props, and locations YOUR story needs,
described precisely enough to redraw identically. The stick figure itself is trivial to reproduce;
what drifts is the **uniform line weight** and the silhouette of any distinctive recurring element
(a mascot, an anthropomorphized object, a specific prop), so lock those in exact words and reuse
them. Include the flat product prop, the accent {BRAND_COLOR}, and the narrator voice.

**B. Your beat list.** As many or as few beats as the idea needs (a 4-beat gag, a 7-beat arc,
whatever serves it), each with a one-line action and one narration sentence written to time (~2.5
to 3 words per second so no clip rides silent). Give each beat a duration (~5 to 10s) and mark
which beats show the product. There is no required beat template; a setup and a payoff is the only
real rule, and even that bends for a pure vibe piece.

Deliver the cast and beat list as JSON the user can edit field by field. The **shape** below is the
contract (fields to fill); the **content** is invented per brand, not copied:

```json
{
  "project": "<brand>-stickman",
  "brand_id": "<id>", "subject": "<product or brand idea>", "product_photo_url": "<url or null>",
  "concept": "<one-line description of the ORIGINAL idea you invented for this brand>",
  "accent_color": "<brand primary hex>", "aspect": "9:16", "language": "en",
  "voice": "<leo / rex / ara / sal / eve>", "video_model": "gemini-omni-flash", "model_source": "default",
  "music": "<describe the bed(s) your story wants; one mood, or two with a switch point if the arc turns>",
  "cast": {
    "protagonist": "<precise flat stick-figure description + any state variants your story uses>",
    "recurring_elements": "<any mascot / device / prop your concept invents, described to redraw exactly, or omit>",
    "product_prop": "<the product redrawn as a flat 2D {BRAND_COLOR} cartoon icon, exact label text copied>",
    "world": "<the flat line-art settings and props your story needs>"
  },
  "beats": [
    { "id": 1, "action": "<what happens on screen>", "dur": 8, "product": false,
      "narration": "<one line, on time>" }
    // ...as many beats as the idea needs
  ]
}
```

> **Illustration only, do NOT reuse:** the reference this style was reverse-engineered from was an
> energy-drink ad using a gray "mood cloud" of fatigue, a bed-to-desk day, and a lightning-blast
> transformation. That is ONE execution of the style. `references/cast-and-story.md` walks through
> it as a worked example so you can see the method. Inventing a fresh concept for each brand is the
> whole point; a new brief should not produce that same arc with a different product.

`voice` picks the narrator (`leo` punchy, `rex` harder sell, `ara` warm, `sal` smooth/calm, `eve`
high-energy) matched to the tone YOUR story wants. Ask: "Approve this concept and beat list, or edit
any field?" Only proceed on a yes. Script edits are free; renders cost credits. Present the credit
cost (see `references/cast-and-story.md`) and wait for confirmation before firing any generation.

## PHASE 3: STORYBOARD STILLS (the stickman look, anchored on a style plate)

Read `references/storyboard-prompts.md`. One flat 2D vector still per beat, built from the generic
six-block structure (STYLE LOCK, ASPECT + FRAMING, CHARACTER(S), SCENE / ACTION, COLOR + VFX,
NEGATIVE). The stick figure is trivial to reproduce, but the **uniform line weight and any recurring
element's silhouette** drift, so anchor every beat on the approved first still (the style plate).

```
advibly_generate_image
  prompt: <the beat's six-block storyboard prompt>
  brand_id: <brand id>
  project_id: <project id>
  model: "gpt-image-2"                          # nano-banana-2 fallback for a shading/silhouette-drifting beat
  quality: "high"
  on_brand: false
  aspect_ratio: "9:16"
  reference_image_urls: [<style-plate URL>, <product photo URL on product beats>]
```

Order and reference chain (roles are generic; apply them to whatever beats your story has):

1. **Generate the style plate first:** the beat that best establishes your character and any
   recurring element (usually beat 1), with no product reference. Approve it; it anchors the whole ad
   by fixing the stick-figure proportions, the line weight, and the silhouettes.
2. **Every other beat** passes the style plate in `reference_image_urls`. Add the product photo URL
   only on the beats that show the product.
3. **A beat with baked text** (a hero/CTA burst, a sign, a label) describes that text in the prompt;
   it is the one place on-image text is intentional.

- **Verify each still is real flat vector art before animating**: uniform black outlines of even
  weight, a plain circle-head stick figure with no volume, a pure white background (not gray or
  off-white), zero shading or gradients (except any intentional {BRAND_COLOR} glow), consistent
  silhouettes for recurring elements, and {BRAND_COLOR} used *only* on the product and its energy.
  The full QA checklist is in the reference. Re-roll here; images are cheap next to clips.
- The STYLE LOCK and COLOR SYSTEM blocks travel **verbatim** across every beat; only the framing,
  scene, and action change. That is what makes 9 beats feel like one film.
- **Present the storyboard to the user** in beat order, each still with its narration line. This is
  the second gate. Re-roll only the misses. Do not animate until the user approves.

## PHASE 4: MOTION (snappy 2D)

Read `references/animate-prompts.md`. Animate each approved still with the six-block motion
structure (CLIP INTRO, SUBJECT LOCK, ACTION, CAMERA, STYLE ANCHOR, AMBIENT, CONSTRAINTS). The
SUBJECT LOCK fragments are lifted verbatim from the cast sheet.

Select the video model once before the render pass:

- **User names Gemini Omni Flash:** use `gemini-omni-flash` for every clip.
- **User names Seedance 2.0:** use `seedance-2.0` for every clip.
- **No model named:** use `gemini-omni-flash` for every clip. Consider Seedance only after two
  failed Gemini attempts on a clip or for a controlled before/after end frame; explain why
  and get approval before switching that clip.

```
advibly_generate_video
  prompt: <six-block motion prompt, SFX-only ambient, NO Narrator line>
  brand_id: <brand id>
  project_id: <project id>
  model: <selected model>
  mode: "pro"                                    # Seedance only; omit for Gemini
  aspect_ratio: "9:16"
  duration: 8                                    # ~5 to 10s to fit the beat; 10 max on Gemini
  start_image_url: <that beat's approved still URL>
```

- `start_image_url` is the approved still. Do NOT also pass `reference_image_urls`: that switches
  video mode and the still stops being the opening frame. The still already carries the flat
  linework, the characters, and any product prop.
- **A before/after transformation beat** (if your story has one) can use `end_image_url` only when
  Seedance was explicitly selected or approved: pass the "before" still as `start_image_url` and the
  "after" still as `end_image_url`. Generate the "after" still in Phase 3. Gemini performs the whole
  change within the clip, no end frame.
- **Forbidden Seedance words** (only when Seedance is selected): `cinematic`, `professional`,
  `stunning`, `8k`, `studio`, `perfect`. Substitute "flat 2D vector cartoon animation", "clean
  flat linework", "high fidelity".
- **The prompt describes motion, not composition** (the still owns composition): one primary
  limited-animation motion plus small secondary motions, snappy and pose-to-pose in feel. The
  camera is a **locked static 2D frame** with no zoom or pan by default; an intentional impact shake
  or a whip is fine when a specific beat calls for it.
- **Every prompt carries the anti-3D constraints**: "preserve the completely flat 2D vector look
  from the start frame, uniform flat black outlines, pure white background, no 3D shading, no added
  shadows, no gradients, no volume, no line flicker or wobble, no morphing of the characters or
  recurring silhouettes". The video model's tendency to add shading and morph the linework is the #1
  failure mode; the reference file has the reusable constraint block.
- **Audio direction in every prompt: comic foley only**, no voiceover, no spoken words, no music
  with lyrics. The narrator is added in the final phase.
- Show each clip. QA per `references/animate-prompts.md` (flat linework end-to-end, no shading
  creeping in, character and recurring silhouettes hold, {BRAND_COLOR} stays only on the product and
  its energy, no on-screen text). Keep, re-edit (same still, adjusted motion prompt), or re-roll.
  2-retry cap per beat; if a third attempt still picks up shading or morphs the lines, regenerate
  that still on `nano-banana-2` and re-animate.

## PHASE 5: CHOOSE FINAL CADENCE

Use On Twos by default. Do not modify the individual approved clips. Phase 6 applies
`frame_cadence: "on_twos"` once at composition time. Skip it only if the user wants fully smooth
motion.

## PHASE 6: VOICEOVER + MUSIC + FINAL MIX

Full recipe and gotchas in `references/audio-and-gotchas.md`.

1. **Voiceover, one line per beat.** Generate each beat's narration line as its own short
   `advibly_generate_voiceover` clip (same `voice` every time), so each can be placed exactly on its
   beat in the mix. A single combined read front-loads all the lines into the first ~15s and leaves
   later beats silent, so do not use one continuous render. Probe each line's length and confirm it
   fits its beat window; re-roll only the lines that overflow. Full reasoning in
   `references/audio-and-gotchas.md`.
2. **Music.** Generate the bed(s) your story wants (`instrumental: true`). Many arcs work with a
   single mood; if your story has a clear turn (a problem-to-relief pivot, a reveal), generate two
   beds and hard-cut between them at that turn's timestamp in the mix. This is optional, driven by
   the story, not a required drone-to-beat switch.
3. **Compose once.** Call `advibly_render_composition` with approved clips as ordered `scenes`
   (each `volume: 0.2`), one `voiceovers` entry per beat at its cumulative `start_seconds`, the
   bed as `music`, the chosen aspect, the run's `project_id`, `keep_scene_audio: true`, and
   `frame_cadence: "on_twos"`. Omit the cadence or pass `"smooth"` only when the user explicitly
   requests smooth motion. The default static music level is correct under VO. It returns
   `status: pending`, `generation_id`, and `edit_url`.
4. Mention the `edit_url` in final delivery so the user can fine-tune the ad in the Advibly video
   editor. On Twos appears under **Effects** and updates the preview in realtime.
5. **Set the project cover.** Call `advibly_update_project` with `{ project_id,
   cover_generation_id: <the final composition's generation id> }` so the project tile shows the
   finished ad.

## PHASE 7: CAPTIONS (optional, after the VO is mixed in)

Upload the final mix with `advibly_upload_asset` (`source_url`), then `advibly_add_subtitles` with
a preset. Two caption styles fit this genre: TikTok white-with-black-stroke, or white bold text on
a solid {BRAND_COLOR} rounded highlight block with a slight tilt (the comic feel). Add the brand
and product names to `vocabulary` so the transcriber spells them right. Never caption the SFX-only
cut; there is nothing to transcribe.

## PHASE 8: OPTIONAL PUBLISH

If the user wants to post it: `advibly_social_list_accounts`, then `advibly_social_create_post`
with the final video. Only offer after the user has seen the finished ad.

---

## Notes and rules

- **Invent the story; the STYLE is the only constant.** The concept, characters, setting, beat
  count, and any metaphor device are designed fresh per brand. A new brief must not reproduce the
  example's arc with a different product. Approve the concept and beat list before any generation.
- **Devices are optional, not defaults.** An anthropomorphized problem (a gremlin, a blob, a mood
  cloud) is one strong option this style is known for; use it when it fits and invent something else
  when it does not. Do not open every ad in a bed with a cloud.
- **Two-accent color, brand color injected.** Pure white, black lines. {BRAND_COLOR} marks the
  product and its positive energy; gray is available for a negative or "problem" element when your
  story has one. Nothing else is colored. `on_brand: false` always; you write the accent color into
  every prompt yourself.
- **The product is a flat 2D prop, never a photoreal composite.** Pass the photo as a reference only
  on the beats that show it, say "redrawn as a flat 2D vector cartoon icon in {BRAND_COLOR}, copy the
  exact label text", and leave it off the beats where the product is not in frame.
- **Anchor every beat on the first style plate.** The stick figure is easy; the line weight and any
  recurring element's silhouette drift. Re-feed the style plate as a reference on every beat.
- **Reuse the exact character and recurring-element wording every beat.** Whatever description you
  lock for a mascot or device appears identically in every prompt; never shorten it later.
- **Keep one video model unless a fallback is approved.** gpt-image-2 for stills (nano-banana-2
  fallback for a single shading-losing beat), Gemini Omni Flash for motion by default, Seedance
  only as the approved secondary fallback or when the user explicitly selected it.
- **Clips are SFX-only; the narrator is external.** No `Narrator:` line in any video prompt; the
  voiceover generates in Phase 6 and mixes on top.
- **The snap is a final-render effect.** Generate smooth motion; add the on-twos limited-animation feel with
  `frame_cadence: "on_twos"` on the final composition. Never ask the video model for "12fps" or "choppy".
- **`brand_id` and `project_id` always; no logo watermark from the kit.** The brand lives in the copied flat-prop
  label text and the accent color.
- **Self-contained prompts.** Generators have no memory of earlier calls; the STYLE LOCK and COLOR
  SYSTEM blocks travel verbatim in every image prompt, the SUBJECT LOCK fragments in every motion
  prompt.
- **On failure:** `content_rejected` means the policy blocked the prompt; rework wording (avoid real
  people and third-party marks). `insufficient_credits`: `advibly_buy_credits`, share the checkout
  link. Shading or 3D creeping into a beat: re-roll the still on `nano-banana-2` and re-animate.
- **No em dashes, minimal emoji** in any copy, label, or narration you draft.

## Honest limits

- **Shading and 3D creep is the constant fight.** Image and video models drift flat vector art
  toward shaded cartoon or 3D render. The prompts guard it hard, but watch every still and clip and
  re-roll the ones that pick up volume, shadows, or gradients. nano-banana-2 stills hold flat lines
  better when a beat keeps shading.
- **Recurring-element silhouettes drift.** Any mascot, anthropomorphized device, or signature prop
  is the hardest thing to keep identical across beats. Anchor on the first style plate and reuse the
  exact wording; if a beat's element looks different, regenerate that still (anchored), not the
  motion prompt.
- **Product logo text garbles** when the flat prop is small or the scene busy. Keep the product
  large and close (30 to 40 percent of frame) on beats that feature it; copy the label/logo text
  exactly in the prompt.
- **Line flicker in motion.** The video model can wobble the outlines frame to frame. The
  anti-flicker constraint helps; the final On Twos effect also masks minor wobble. Re-roll a clip whose
  lines visibly boil.
- **The announcer voice is one of five xAI voices**, not a specific casting. `leo` is the closest
  punchy-announcer match; pick one and keep it across the whole ad.

## Reference files

- `references/cast-and-story.md`: the STORY toolkit (open). Storytelling principles, the device menu
  and structure menu, how to design a bespoke cast, world, and beat list, narration voice and timing,
  one labeled worked example, and the cost shape. Read before inventing the concept.
- `references/storyboard-prompts.md`: the LOOK layer for stills (locked). The six-block image prompt
  structure, the STYLE LOCK / COLOR SYSTEM / NEGATIVE blocks, the comic-VFX vocabulary, the generic
  per-shot scaffold with one labeled example, the cross-beat continuity rules, and the image QA
  checklist. Read before writing any storyboard prompt.
- `references/animate-prompts.md`: the LOOK layer for motion (locked). Model selection, the six-block
  motion structure, the anti-3D / anti-flicker constraint block, the generic motion scaffold with one
  labeled example, cross-clip continuity, and the per-clip QA checklist. Read before writing any
  motion prompt.
- `references/audio-and-gotchas.md`: the voice map and timing, per-line VO placement, the one-or-two
  bed music guidance, the final composition contract (per-beat VO offsets, static music, automatic trim), the final-render On Twos effect, captions, and the failure-mode
  gotchas. Read before the audio mix or debugging a weak render.
