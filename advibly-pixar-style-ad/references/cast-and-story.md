# Cast and Story Layer

## Original feature-film 3D look

Use "Pixar-style" only as the user's shorthand. Make the actual cast and prompts original:
expressive oversized eyes with multiple catchlights, softened believable proportions, warm
volumetric window light, detailed tactile materials, shallow depth of field, and a hopeful
micro-story. Do not name a studio or request an existing character in generation prompts.

## The default 4-beat arc

The hook is an anthropomorphized pain point, not a package shot. Gemini Omni Flash is the default:
use four **8-second** beats for a roughly 32-second cut. Hold each shot to 8 seconds: a 10-second
beat goes too still and starts to morph, so one clear action carries each 8-second clip. Use
`duration: 8` for every clip, including an explicitly selected or fallback Seedance 2.0 render,
unless the user explicitly requests a different duration. Write the external narration to time,
roughly 2.2 to 2.6 words per second (about 18 to 20 words per 8-second beat).

| Beat | Duration | Role | Visual | Product | Motion / SFX |
|---|---:|---|---|---|---|
| 1 | 8s | Hook | Close macro of the sentient problem object looking at camera | No | blink, slouch, tiny sigh, room-specific ambience |
| 2 | 8s | Reveal | Original protagonist discovers or presents the product in a warm interior | Yes | look from product to camera, small smile, room tone |
| 3 | 8s | Friendly mechanism | Original helper mascots turn the approved mechanism into a visual metaphor | No | coordinated repair/sort/smooth action, soft sparkle/working sounds |
| 4 | 8s | Resolution and CTA | Same protagonist confidently holds the product; clean lower third | Yes | warm smile, small product lift, room tone |

Use a five-beat version only when the claim needs a separate proof or testimonial beat. Do not
stretch an 8-second visual beyond its story value merely to reach a conventional ad length.

## Cast-and-continuity sheet

Lock this wording before still generation. Copy it verbatim into every relevant still and motion
prompt.

```text
PROTAGONIST
- age range and build:
- hair, including color, length, and specific style:
- eyes, skin details, and expression cue:
- exact outfit and accessory:
- personality / gesture:

PROBLEM CHARACTER
- ordinary object and context:
- face placement, eye style, mouth, and emotion:
- posture and tiny behavior:

HELPER MASCOTS
- number and form:
- material and color:
- eyes, cheeks, limbs, and behavior:

SETTING
- beat 2 and 4 interior:
- beat 3 visual-metaphor landscape:
- lighting, palette, and recurring prop:

PRODUCT
- exact package shape, palette, label text, and reference-photo URL:
- how it is held and when it must not appear:

STYLE LOCK
- expressive feature-film 3D animation; warm volumetric window light; large expressive eyes
  with multiple catchlights; tactile materials; stylized believable proportions; creamy shallow
  depth of field; warm cozy palette; painterly background.
```

## Mechanism scenes and claim safety

Turn a benefit into a clear visual metaphor, not a clinical or guaranteed outcome. Use only a
mechanism approved in the brand context.

| Product area | Safer visual metaphor | Mascot action |
|---|---|---|
| Hair care | lifted cuticle-scale landscape becoming smoother | align soft overlapping tiles and place shimmering droplets |
| Skin care | dry-looking surface becoming comfortably dewy | guide translucent moisture beads through a friendly landscape |
| Supplement / wellness | a balanced everyday-energy garden | tend glowing pathways and arrange calm, evenly spaced leaves |
| Cleaning | a cluttered/smudged surface becoming orderly | lift, sort, and polish ordinary objects |
| Productivity software | a tangled task-map becoming clear | route colorful cards along a simple path |

Avoid internal organs, diagnoses, treatment claims, before/after medical imagery, or statements
that the product causes a biological outcome. For a regulated category, make the beat about the
approved everyday benefit and include required qualifiers in post-production copy, not inside the
generated scene.

## Approval map

Return this JSON after intake. Fill it with real values, keep the cast strings stable, and let the
user edit any field before generation.

```json
{
  "project": "<brand>-feature-3d-ad",
  "brand_id": "<id>",
  "product": "<name>",
  "product_photo_url": "<url or null>",
  "category": "<category>",
  "audience": "<audience>",
  "aspect_ratio": "9:16",
  "language": "en",
  "video_model": "gemini-omni-flash",
  "model_source": "default",
  "voice": "ara",
  "music": "gentle warm feature-film underscore, soft piano and light strings, hopeful and unhurried, ~85 BPM, storybook ad bed, instrumental",
  "style_lock": "expressive feature-film 3D animation, warm volumetric window light, large expressive eyes with multiple catchlights, tactile materials, stylized believable proportions, creamy shallow depth of field, warm cozy palette, painterly background",
  "cast": {
    "protagonist": "<exact reusable description>",
    "problem_character": "<exact reusable description>",
    "mascots": "<exact reusable description>",
    "setting": "<exact reusable description>",
    "product_description": "<exact reusable description>"
  },
  "beats": [
    {"id": 1, "role": "hook", "duration": 8, "product": false, "narration": "<line>", "sfx": "<sound>"},
    {"id": 2, "role": "reveal", "duration": 8, "product": true, "narration": "<line>", "sfx": "<sound>"},
    {"id": 3, "role": "mechanism", "duration": 8, "product": false, "narration": "<line>", "sfx": "<sound>"},
    {"id": 4, "role": "cta", "duration": 8, "product": true, "narration": "<line>", "sfx": "<sound>"}
  ]
}
```

`voice` picks the narrator (`ara` warm-storyteller default, `sal` smooth, `leo` authoritative,
`rex` confident, `eve` energetic) and `music` describes the instrumental bed; both are generated
and mixed in Phase 6. Ask: "Approve this cast sheet and beat map, or tell me what to change?" Do
not generate media before the answer is affirmative.
