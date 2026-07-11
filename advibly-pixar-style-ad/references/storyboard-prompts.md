# Storyboard Still Prompts

Generate the finished look in the still first. Use `gpt-image-2`, `quality: "high"`,
`on_brand: false`, `aspect_ratio: "9:16"`, and one image per call. The model may render the still
in chat before it returns a reusable URL; call `advibly_get_generation` (`wait: true`) when the
next beat needs that URL.

## Prompt structure

Use the same five blocks in this order. Keep the style lock byte-identical across the board.

```text
[STYLE LOCK]
[ASPECT + FRAMING]
[CHARACTER / PRODUCT]
[SCENE + ACTION]
[CONSTRAINTS]
```

### Style lock

```text
Original expressive feature-film 3D animation, warm volumetric golden-hour window lighting,
large expressive eyes with several soft catchlights, stylized but believable proportions, tactile
material detail, gentle subsurface glow on skin, rich fabric and ceramic texture, shallow depth
of field with creamy bokeh, a warm cozy palette and painterly background. Vertical 9:16.
```

### Constraints

```text
Original characters only. No studio logo, existing franchise character, watermark, or unrelated
logo. Not live action, photorealistic, anime, 2D, cel-shaded, or flat illustration. No malformed
hands, extra fingers, extra eyes, merged features, warped package labels, or on-screen text unless
the plan explicitly requires it.
```

## Beat 1: anthropomorphized problem

```text
[STYLE LOCK]

Vertical 9:16 extreme close-up macro of <problem object> on <surface/location>. Embedded in the
object are two oversized expressive eyes with <emotion>, several small catchlights, and a small
<mouth shape>. The object has a <slumped / frazzled / wilted> posture. Soft-focus <setting> in
the background with warm ambient bokeh. The character looks directly at the camera.

[CONSTRAINTS]
```

Do not include the product reference in this call. The visual should earn curiosity before a
package appears.

## Beat 2: protagonist product reveal

```text
[STYLE LOCK]

Vertical 9:16 medium head-and-shoulders shot of <PROTAGONIST, copied verbatim> in <SETTING,
copied verbatim>. Soft window light from camera-left wraps the face. They hold <PRODUCT,
copied verbatim> at chest height with <hand position>, looking at it with <delighted / relieved
/ curious expression>. Include <recurring props> in soft focus. Preserve the exact hair, eyes,
skin details, outfit, and proportions from the protagonist reference image. Package label faces
camera and matches the supplied product reference.

[CONSTRAINTS]
```

Pass an approved protagonist anchor and product-photo URL in `reference_image_urls` when a hero
reference already exists. If this is the protagonist's first appearance, use the supplied product
photo only and treat the approved beat-2 still as the identity anchor for beat 4.

## Beat 3: friendly mechanism metaphor

```text
[STYLE LOCK]

Vertical 9:16 stylized cross-section or symbolic landscape of <APPROVED BENEFIT METAPHOR>, made
of <textures and palette>. Three to five <MASCOTS, copied verbatim> work together to <specific
approved action>. Gentle glowing <energy / moisture / organization> paths trace through the scene.
The design remains friendly and abstract, not clinical or anatomical. Warm interior glow and soft
depth of field.

[CONSTRAINTS]
```

Do not attach the product photo. The mascot language should show the mechanism without letting a
package distract from it.

## Beat 4: resolution and CTA still

```text
[STYLE LOCK]

Vertical 9:16 medium shot of <PROTAGONIST, copied verbatim> in the same <SETTING>. They face the
camera with a warm confident smile and hold <PRODUCT, copied verbatim> at chest height, label
cleanly facing the viewer. Keep the lower third calm and uncluttered for a post-production CTA.
Preserve exact protagonist identity and package details from the approved references.

[CONSTRAINTS]
```

Pass the approved protagonist reveal still and product-photo URL in `reference_image_urls`.

## Continuity and still QA

1. Generate beat 1 or a dedicated protagonist hero anchor first. Approve the identity anchor.
2. Reuse exact cast wording; never replace "chestnut side braid" with "brown hair tied back."
3. Carry only the necessary references. Product photo on beats 2 and 4. No product photo on
   beats 1 and 3.
4. Review before video: correct identity, aligned eyes, exactly five fingers per visible hand,
   legible unchanged package label, original feature-animation look, 9:16 framing, and clean
   CTA lower third.
5. Retry a still at most twice. If it still misses, ask the user whether to simplify the framing
   or change the visual plan.
