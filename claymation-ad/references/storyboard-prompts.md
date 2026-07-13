# Storyboard prompts: the claymation stills (gpt-image-2)

Generate the still-image storyboard before animating. Read `cast-and-story.md` first for the
8-beat arc and the cast-and-continuity sheet. Every still is made with `advibly_generate_image`
(`model: "gpt-image-2"`, `quality: "high"`, `on_brand: false`, `aspect_ratio: "9:16"`,
`brand_id` required). The `nano-banana-2` fallback is for a single beat whose clay texture keeps
flattening (see the QA section).

## Universal prompt structure

Each beat uses the same six-block structure, in this order:

```
[STYLE LOCK]            <- identical across all beats, paste verbatim
[ASPECT + FRAMING]
[CHARACTER(S)]         <- protagonist alone, two-shot, or product / infographic
[SCENE / ACTION]
[MATERIAL DETAIL]      <- clay / fabric / wood specifics; critical for the look
[NEGATIVE]
```

Keep the whole prompt under the 2000-character cap. Trim adjectives before you trim the clay
texture or identity description.

### STYLE LOCK (paste verbatim into every beat)

```
Aardman-style stop-motion claymation aesthetic. Hand-sculpted plasticine characters with
visible fingerprint impressions and sculpting-tool marks on clay surfaces, matte clay material
with subtle micro-bumps, slightly asymmetric facial features, painted-on or carefully sculpted
eyebrows. Real knit-fabric clothing with visible weave and stitch lines. Wooden and ceramic
miniature-set props with hand-painted finishes. Warm tungsten interior lighting, shallow macro
depth of field with soft photographic bokeh that reinforces the miniature-set illusion. Subtle
imperfection in every surface: slightly uneven paint, irregular fabric weave, asymmetric forms.
```

### NEGATIVE (paste into every beat)

```
Not photorealistic, not live-action, not Pixar style, not 3D rendered, not CGI, not anime, not
2D illustration, not smooth digital render, not glossy, no ray-traced reflections, no
subsurface scattering, no oversized Pixar-style eyes with multiple highlights, no extra
fingers, no merged features, no warped product labels, no on-screen text unless explicitly
requested.
```

### MATERIAL DETAIL (paste a relevant subset into every beat)

Adjust which lines apply to what is in frame:

```
- Skin: matte plasticine, visible thumbprint impressions on cheeks and forehead, small
  sculpting-knife creases at the corners of the eyes, slight left-right asymmetry
- Hair: sculpted in distinct ribbon-strands of plasticine, individual strand grooves carved
  with a tool, slightly stiff and not flowing
- Eyes: small matte clay or painted-resin orbs set into sculpted sockets, a single soft
  highlight, no wet shine
- Knitwear: real chunky wool yarn, individual stitches visible, slight wear at cuffs and hems
- Wood props: hand-painted matte finish, visible grain, small dents and scratches that suggest age
- Ceramic / pottery: hand-thrown irregular form, glaze pooling at the bottom edges, slight
  off-roundness
- Product packaging: rendered as a clay-shaded prop with a hand-painted label, paint slightly
  uneven and matte, exact label text copied from the reference photo
```

---

## Beat 1 - Setup (protagonist in their world)

Wide or medium shot establishing the protagonist in the primary miniature set. No product
reference. This is the identity anchor for the whole ad; approve it first.

```
{STYLE LOCK}

Aspect ratio 9:16, {WIDE_OR_MEDIUM} shot of {PROTAGONIST_FROM_CAST_SHEET} standing or sitting in
{PRIMARY_SETTING}. {POSTURE_AND_ACTION}. Warm morning tungsten light falls across the scene from
{LIGHT_SOURCE}, casting soft shadows on the wooden floor. Background includes {BACKGROUND_PROPS}
in soft macro bokeh; the miniature-set illusion is strong.

{MATERIAL DETAIL: skin, hair, knitwear, wood, ceramic}

{NEGATIVE}
```

Worked example (Diane in her kitchen): medium-wide shot of Diane, late 50s, shoulder-length
terracotta-brown wavy plasticine hair in ribbon-strands, deep sculpted laugh lines and hooded
eyelids, warm brown clay eyes; cream chunky knit cardigan over a rust-red blouse. She stands at
her sunlit kitchen counter pouring tea from a hand-thrown ceramic kettle. Warm window light on
camera-left; green-painted cabinets, red gingham tablecloth, potted herbs on the windowsill in
soft macro bokeh. Material detail: thumbprint impressions on cheeks, carved strand grooves in
the hair, real wool weave with cuff wear, visible wood grain, off-round glaze-pooled kettle.

## Beat 2 - Inciting moment (close-up, notices the problem)

Tight close-up on the face as they see the issue. Surprised or concerned expression. Anchor on
beat 1's approved still.

```
{STYLE LOCK}

Aspect ratio 9:16, tight close-up of {PROTAGONIST_FROM_CAST_SHEET}'s face as she
{NOTICING_ACTION}. Her expression is {CONCERNED_EXPRESSION}: {SPECIFIC_FACIAL_CUE}.
{REFLECTION_OR_FOCAL_OBJECT} is partially visible in frame. Soft directional tungsten light
wraps her face from {LIGHT_DIRECTION}. Shallow depth of field, miniature-set bokeh behind her.

{MATERIAL DETAIL: emphasize skin texture, eye sockets, sculpted brow}

{NEGATIVE}
```

Variable examples: `NOTICING_ACTION` = "leans toward a small wood-framed bathroom mirror,
fingertip lifted to her upper lip" / "looks down at the bathroom scale at her feet".
`CONCERNED_EXPRESSION` = "softly furrowed brow, mouth slightly open" / "eyes widening with quiet
alarm". `SPECIFIC_FACIAL_CUE` = "small carved lines visible above her lip".

## Beat 3 - Social validation (two-shot)

Protagonist with the supporting character in the secondary setting. Attach beat 1 or 2 as the
protagonist reference; the supporting character is generated fresh from the cast sheet.

```
{STYLE LOCK}

Aspect ratio 9:16, medium two-shot of {PROTAGONIST_FROM_CAST_SHEET} on the {LEFT_OR_RIGHT} and
{SUPPORTING_CHARACTER_FROM_CAST_SHEET} on the opposite side, both seated at
{SECONDARY_SETTING_PROP}. {SHARED_ACTIVITY}. The supporting character is mid-remark, mouth
slightly open, sculpted eyebrows raised in curiosity, looking at the protagonist. The
protagonist {REACTION}. Background includes {SECONDARY_SETTING_DETAIL} in soft macro bokeh. Warm
tungsten light from above.

{MATERIAL DETAIL: both characters' skin / hair / knit; secondary-setting props}

{NEGATIVE}
```

## Beat 4 - Quiet despair (solo reflection)

Protagonist alone, at a mirror or window. The narrator carries the emotional beat. Dimmer,
sparser than the other beats to emphasize solitude. Anchor on the prior protagonist still.

```
{STYLE LOCK}

Aspect ratio 9:16, {MEDIUM_OR_WIDE} shot of {PROTAGONIST_FROM_CAST_SHEET} standing alone in
{INTROSPECTIVE_LOCATION}, {REFLECTIVE_POSE}. Her expression is {SUBDUED_EMOTION}. Soft dim
tungsten light enters from {LIGHT_SOURCE}, casting long sculpted shadows. The background is
sparse, quiet, and slightly shadowed compared to other beats.

{MATERIAL DETAIL: skin / hair / knit emphasized in the dimmer light}

{NEGATIVE}
```

## Beat 5 - Clay infographic (no characters, independent still)

A hand-sculpted clay chart explaining the mechanism. The sculpted-clay letters ARE the baked
text (the one beat with intentional on-image text). No character reference; can fire in
parallel with beat 1.

```
{STYLE LOCK}

Aspect ratio 9:16, head-on shot of a {CHART_TYPE} sculpted entirely from clay and plasticine on
a {FRAME_DESCRIPTION}. Title at top reads "{TITLE_TEXT}" in chunky hand-sculpted clay letters
with slight asymmetry, each letter individually shaped by hand. The chart shows {CHART_CONTENT}.
{INDICATOR_OR_ANNOTATION}. Soft tungsten light falls across the chart from camera-left, casting
subtle sculpted shadows that reveal the depth of each clay element. The wall behind the frame is
plain cream-painted clay with a slight texture.

{MATERIAL DETAIL: clay letters and lines, hand-shaped imperfection}

{NEGATIVE}
```

Worked example (Calcium-in-skin chart): a line graph sculpted from clay on a hand-carved wooden
frame. Title "CALCIUM IN SKIN" in chunky hand-sculpted clay letters. A high horizontal line on
the left labeled "HIGH" drops sharply to "LOW" near the right, x-axis tick marks "30 40 50 60"
as clay buttons, a small clay arrow at the drop labeled "MENOPAUSE" on a sculpted rounded tag.

## Beat 6 - Discovery (product close-up + reach)

Close to medium shot of the product as a clay prop on a wooden surface, protagonist's hand
reaching in. **First product beat: pass the product photo URL in `reference_image_urls`** and
point at it descriptively; the product is re-sculpted as clay, not composited photoreal.

```
{STYLE LOCK}

Aspect ratio 9:16, {CLOSE_OR_MEDIUM} shot of {PRODUCT_AS_CLAY_PROP: rendered as a clay-stylized
prop, matte hand-painted label copying the exact text from the reference photo, slightly
imperfect cylinder} sitting on {SURFACE}. {PROTAGONIST_FROM_CAST_SHEET}'s hand enters frame from
{HAND_DIRECTION}, sculpted fingers reaching toward the product. Surrounding props include
{SUPPORTING_PROPS} in soft macro bokeh. Warm tungsten light from {LIGHT_SOURCE} catches the
product label, making the hand-painted text readable.

{MATERIAL DETAIL: product prop, surrounding wood / ceramic / cloth, sculpted hand}

{NEGATIVE}
```

Worked example (refirm bottle): a small dusty-purple cylindrical bottle labeled "refirm" in
hand-painted cream lettering, rendered as a clay prop (slightly imperfect cylinder, hand-applied
matte paint with subtle brush texture) on a wooden kitchen table. Diane's terracotta-clay hand
enters from camera-right. Ceramic cup, red gingham corner, tin kettle in bokeh. The prompt
carries the product photo URL as reference so the label text matches exactly.

## Beat 7 - Transformation (montage / weeks later)

Time passes, the product is used, a subtle improvement shows. Reuse the beat-1 or beat-2
location. Reference the beat-6 product prop still (to keep the bottle identical) plus the prior
protagonist still. **If the clip will use an end-frame reveal, generate two stills here:** a
"before" and a lightly-edited "after" where only the improved area changes.

```
{STYLE LOCK}

Aspect ratio 9:16, {FRAMING} of {PROTAGONIST_FROM_CAST_SHEET} {USING_OR_AFTER_USING_PRODUCT}.
{SUBTLE_TRANSFORMATION_CUE}. Her expression is {POSITIVE_EMOTION}: {SPECIFIC_FACIAL_CUE}. Soft
natural tungsten light, slightly brighter and warmer than earlier beats to signal positive
change. Background is the {PRIMARY_SETTING} from Beat 1, lit a touch more openly.

{MATERIAL DETAIL: note the subtle improvement, e.g. slightly smoother clay skin only in the
specific area, slightly more open eyes, posture upright; everything else unchanged}

{NEGATIVE}
```

The improvement must be **localized and same-identity**: "the carved lines above her lip are
subtly less pronounced, the clay surface there a touch smoother, but the same character, same
hair, same outfit". Never smooth the whole face (that reads as a different, Pixar render).

## Beat 8 - Resolution + CTA

Confident protagonist with the product, lower third kept clean for the burned-in CTA caption.
Reference the beat-7 protagonist still and the beat-6 product still.

```
{STYLE LOCK}

Aspect ratio 9:16, medium shot of {PROTAGONIST_FROM_CAST_SHEET} in {PRIMARY_SETTING}, facing
camera directly with a {CONFIDENT_SMILE}. She holds {PRODUCT_AS_CLAY_PROP} at chest height with
sculpted clay hands, the hand-painted label rotated cleanly toward camera. Warm tungsten light
from camera-left wraps her face, a soft rim light from camera-right. {BACKGROUND_PROPS} in soft
macro bokeh. The lower third of frame remains visually clean and uncluttered, leaving room for a
post-production caption overlay.

{MATERIAL DETAIL: clay skin, hair, knitwear, product prop, surrounding props}

{NEGATIVE}
```

---

## Cross-beat continuity rules

1. **Generate sequentially, not in parallel.** Identity drift compounds. Order: beat 1 (anchor)
   > beats 2, 4, 6, 7, 8 each anchored on the prior approved protagonist still > beat 3 anchored
   on beat 1 or 2 > beat 5 independent (parallel with beat 1 is fine).
2. **Always pass the prior approved protagonist still** in `reference_image_urls` on the next
   protagonist beat. Missing it = the sculpted face drifts.
3. **Keep the STYLE LOCK and MATERIAL DETAIL blocks verbatim.** Do not paraphrase.
4. **Reuse exact phrasing** for hair color, eye color, clothing, and skin texture across every
   protagonist beat. "Terracotta-brown wavy plasticine hair in ribbon-strands" appears
   identically every time; never shorten to "brown clay hair" later.
5. **Beat 6's product prop still is the product reference** for beats 7 and 8; pass it so the
   clay bottle stays identical.
6. **Product photo reference only on beats 6, 7, 8.** Beats 1 to 5 carry no product reference or
   the model leaks the bottle in early.

## Image QA checklist (claymation-specific)

Before sending a still to the video model, verify:

- [ ] **Clay texture preserved**: thumbprint and tool marks visible on faces and hands, no
      smooth Pixar render leaking in.
- [ ] **Knit fabric reads as real wool weave**, not painted-on stripes.
- [ ] **Eyes are matte**, a single soft highlight max, no Pixar wet-eye multi-catchlight.
- [ ] **Hair shows individual carved strand grooves.**
- [ ] **Wooden and ceramic props show hand-painted finish** and slight irregularity.
- [ ] **Character identity holds** across every protagonist beat: same face proportions, same
      hair color and style, same outfit.
- [ ] **Product label paint looks hand-applied** (slightly uneven, matte) and the copied text is
      legible.
- [ ] **No burned-in text** except beat 5's sculpted-clay chart.
- [ ] **9:16 aspect ratio.**
- [ ] **Lower third has clean negative space** on beat 8 for the caption overlay.

Standard 2-retry cap per beat. If the third attempt still loses clay texture or identity, **try
`nano-banana-2` for that specific beat** (it holds texture better on close-ups and product
props). Switch that one beat, not the whole ad.

## When the user already has a claymation hero image

If they provide an existing clay-style hero still, skip building the protagonist from scratch:
pass that hero as the `reference_image_url` for every protagonist beat. Beat 5 (infographic)
does not need it.
