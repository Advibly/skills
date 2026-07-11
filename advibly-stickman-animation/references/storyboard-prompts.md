# Storyboard prompts: the stickman stills (gpt-image-2)

The LOOK layer for stills. This holds constant no matter what story you invented in
`cast-and-story.md`. Every still is one flat 2D vector frame of one of your beats, made with
`advibly_generate_image` (`model: "gpt-image-2"`, `quality: "high"`, `on_brand: false`,
`aspect_ratio: "9:16"`, `brand_id` required). The `nano-banana-2` fallback is for a single beat
whose linework keeps picking up shading or whose recurring silhouette drifts (see QA).

`{BRAND_COLOR}` is the brand's primary color read in Phase 1. Write the literal color into every
prompt (for example "red (#E03131)").

## Universal prompt structure (six blocks, every beat)

```
[STYLE LOCK]            <- identical across all beats, paste verbatim
[ASPECT + FRAMING]
[CHARACTER(S)]         <- your protagonist / recurring element / product / whatever the beat holds
[SCENE / ACTION]       <- what this beat of YOUR story shows
[COLOR + VFX]          <- the two-accent system + whatever comic effects this beat needs
[NEGATIVE]             <- identical across all beats, paste verbatim
```

Keep the whole prompt under the 2000-character cap. Trim adjectives before you trim the
flat-linework or color-system description.

### STYLE LOCK (paste verbatim into every beat)

```
Flat 2D minimalist vector cartoon in the stick-figure comic style. Pure solid white (#FFFFFF)
background, an empty void with no floor line, no horizon, no gradient. Every character and prop
drawn in uniform clean black outlines of even stroke weight, hard corners, zero anti-aliasing
softness. The character is a simple stick figure: a perfect circle head with a flat white fill,
small black dot eyes and a simple line mouth, a single-line torso and single-line stick limbs with
small rounded hands and feet. Props are simple flat 2D line-art outlines, orthographic and
diagrammatic, with no shading. Absolutely flat: no cel-shading, no gradients, no drop shadows, no
volume, no depth, no texture. Comic-strip clarity, lots of white negative space.
```

### COLOR SYSTEM (paste verbatim into every beat, filling {BRAND_COLOR})

```
Strict limited palette. The entire scene is black-and-white line art on pure white, with only two
accent colors that carry meaning: {BRAND_COLOR} for the product and its positive energy (glow,
sparkle, the hero burst), and GRAY for a negative or "problem" element if this beat has one. Use
{BRAND_COLOR} nowhere except the product and its energy. Use gray nowhere except a problem element.
Everything else is pure black line on white.
```

### NEGATIVE (paste verbatim into every beat)

```
No 3D rendering, no shading, no gradients except the intentional colored glow described, no drop
shadows, no cross-hatching, no realistic human anatomy, no detailed face, no clothing detail, no
muscle or body volume, no textured or off-white or gray background, no photographic elements, no
thin sketchy pencil lines, no double outlines, no motion blur, no watermark, no on-screen text
unless explicitly described in this prompt.
```

## Comic VFX vocabulary (draw on these as your beats need them)

Flat, black-or-accent-colored comic effects, never rendered: "Z" sleep marks, motion/speed lines,
impact stars and starbursts, sweat drops, a radial {BRAND_COLOR} glow or aura, {BRAND_COLOR}
lightning bolts, internal energy squiggles, a classic halftone-dot comic burst (POW / slogan),
small puff/smoke clouds, question and exclamation marks, thought/speech bubbles, hearts, tiny
scribble-thought squiggles, drips, cracks, a white impact flash. Use gray versions for a problem
element, {BRAND_COLOR} versions for the product and its energy, black for neutral marks.

## Generic per-shot scaffold (fill for each of YOUR beats)

Every beat's still is the six blocks with STYLE LOCK, COLOR SYSTEM, and NEGATIVE pasted verbatim
and only the middle three written fresh:

```
{STYLE LOCK}

Aspect ratio 9:16, {framing: wide / medium / close / centered hero}. {CHARACTER(S): your
protagonist in the exact locked wording, plus any recurring element in its exact locked wording, or
the product prop, or whatever this beat holds}. {SCENE / ACTION: what is happening in this beat of
your story, one clear readable action, plus the flat line-art props of the setting}. {COLOR + VFX:
which of the comic effects appear this beat, and the strict reminder that {BRAND_COLOR} is only on
the product/energy and gray only on a problem element}.

{COLOR SYSTEM}

{NEGATIVE}
```

Rules that make the beats feel like one film:

- **Paste STYLE LOCK, COLOR SYSTEM, and NEGATIVE verbatim** in every beat. Do not paraphrase.
- **Reuse the exact character and recurring-element wording** every beat, word for word.
- **One clear action per still.** Stickman reads instantly; do not crowd the frame.
- **Product photo reference only on beats that show the product**; describe it as the flat prop,
  never the raw photo. Leave it off beats where it is not in frame.
- **Baked text only where intended** (a hero/CTA burst, a sign): describe it in the prompt. Every
  other beat forbids on-screen text via the NEGATIVE block.

## Worked example (ILLUSTRATION ONLY, do not reuse)

From the energy-drink reference: the opening "problem intro" beat, showing how the middle three
blocks get filled. This demonstrates the scaffold; invent your own beats.

```
{STYLE LOCK}

Aspect ratio 9:16, wide shot. A plain black-outline stick figure lies flat on its back in a simple
flat line-art bed, wide awake and sleepless, eyes open, tense. A small nightstand holds a lamp and
an alarm clock; a window with a crescent moon is behind. Directly above the head hovers a gray
lumpy storm-cloud blob with a uniform black outline and a manic black cartoon face, buzzing with
tiny gray scribble-thought squiggles (a racing mind). Lots of white space.

{COLOR SYSTEM}   (here: gray on the mood cloud only; no {BRAND_COLOR} yet, no product in frame)

{NEGATIVE}
```

Notice: STYLE LOCK / COLOR SYSTEM / NEGATIVE are boilerplate; only the framing, characters, action,
and VFX are authored. A different concept fills those three slots with entirely different content.

## Cross-beat continuity rules

1. **Generate the style plate first** (the beat that best establishes your character and any
   recurring element), approve it, and it anchors the whole ad's line weight and silhouettes.
2. **Pass the style plate** in `reference_image_urls` on every later beat. Add the product photo only
   on beats that show the product.
3. **Keep STYLE LOCK, COLOR SYSTEM, and NEGATIVE verbatim.**
4. **Reuse exact phrasing** for the protagonist and any recurring element across every beat.
5. **{BRAND_COLOR} discipline:** only on the product and its positive energy. Gray only on a problem
   element. Everything else black on white. Re-roll a still that colors a wall, a prop, or the figure
   with the accent for no reason.

## Image QA checklist (stickman-specific)

Before animating, verify each still:

- [ ] **Flat vector, zero shading**: uniform black outlines, no cel-shading, no gradients (except an
      intentional {BRAND_COLOR} glow), no drop shadows, no 3D volume.
- [ ] **Pure white background**, not gray, off-white, or a studio backdrop.
- [ ] **Stick-figure construction correct**: circle head with flat white fill, dot eyes, single-line
      limbs, no body volume, no clothing detail, no realistic anatomy.
- [ ] **Recurring elements hold**: any mascot or device matches the style plate's silhouette and line
      weight.
- [ ] **{BRAND_COLOR} only on the product and its energy**; gray only on a problem element.
- [ ] **Product (when present) is a flat prop** with legible copied logo/label text.
- [ ] **No burned-in text** except where you intentionally described it.
- [ ] **9:16 aspect ratio**, and clean negative space wherever a caption will overlay.

Standard 2-retry cap per beat. If a beat keeps shading the linework or drifting a silhouette, try
`nano-banana-2` for that one beat (it holds flat lines better). Switch that beat, not the whole ad.

## When the user already has a stickman character or hero image

Pass that image as the style plate `reference_image_url` for every beat, and match its line weight
and silhouettes in the wording.
