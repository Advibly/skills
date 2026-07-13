> **This is a STYLE reference, not a script.** It defines an art direction: the **look, motion,
> typography, and audio** of one visual style, and nothing about any particular story. Any subject,
> character, prop, scene, or on-screen wording mentioned below (including the `example_subject_only`
> line and any examples inside `character_design`, `environment_design`, or `vfx`) is there only to
> illustrate the technique, never to be reproduced. **Invent a fresh concept, cast, and script for
> THIS brand and topic.** Read `character_design` as construction logic (how this style builds a
> character: proportions, how eyes and edges are made) and design your own original character with
> it. Treat every value as directional guidance and a quality floor, not a spec to copy 1:1. You
> have full creative freedom on the idea; the style file only governs how it should look and sound.

### 1. One-line style signature

A tactile, stop-motion dry-erase whiteboard animation featuring thick, brightly colored marker doodles on a smudged off-white surface, characterized by jittering "boiling" lines, stroke-by-stroke draw-ons, and squeaky marker sound effects.

### 2. Prose breakdown

**Medium, Texture, and Environment:**
The visual style is a simulated physical whiteboard environment. The foundational texture is an off-white, slightly matte board surface (#F4F4F6) heavily characterized by "ghosting"—the faint, gray/black smudges and circular erase marks left behind by previous drawings. This imperfection is the single texture tell that grounds the medium. The artwork itself mimics thick, bullet-tip dry-erase markers. The ink is not perfectly opaque; it exhibits slight pooling, variable opacity, and rounded stroke ends, strictly adhering to a standard physical marker palette (primary blue, red, green, orange, and purple).

**Motion, Animation, and Camera:**
The animation cadence mimics 12fps stop-motion (animated on twos). The defining motion characteristic is "line boiling"—the subtle, continuous jitter of the drawn outlines even when the character is standing still, which breathes life into otherwise static frames. Elements are introduced either via rapid, stroke-by-stroke draw-on reveals (simulating the act of drawing) or instant single-frame pops for comedic effect (like a brain or a thermometer appearing). The camera is almost entirely a locked-off, straight-on medium shot of the whiteboard. Instead of cutting to new scenes, the camera executes rigid, horizontal 2D pans across the flat surface to reveal adjacent drawings, occasionally showing the physical metallic seam between two whiteboards.

**Audio and Typography:**
Typography is treated as diegetic art, appearing as handwritten, slightly slanted marker text that integrates directly into the doodles. It uses mixed casing and avoids perfect baselines, reinforcing the casual, extemporaneous feel. The audio mix heavily relies on Foley to sell the illusion: rapid, squeaky marker scribbles are tightly synced to the draw-on animations, complemented by distinct popping sounds for sudden visual additions. A light, upbeat, pizzicato-driven instrumental track sits low in the mix beneath a clear, casually paced voiceover, ensuring the educational or explanatory dialogue remains the focal point while the SFX provide the textural energy.

### 3. Style spec

```json
{
  "style_id": "whiteboard_marker_doodle",
  "display_name": "Whiteboard Stop-Motion",
  "one_line_signature": "A tactile, stop-motion dry-erase whiteboard animation featuring thick, brightly colored marker doodles on a smudged off-white surface.",
  "example_subject_only": "The physiological mechanics and contagiousness of yawning.",
  "medium": "Simulated physical dry-erase whiteboard animation.",
  "texture_and_render": "Matte whiteboard surface with distinct, faint gray circular smudges (erase ghosting). Linework exhibits variable opacity and slight edge bleed typical of bullet-tip wet-erase markers.",
  "lighting": "Flat, even, overhead fluorescent-style lighting with very subtle corner vignetting.",
  "palette": {
    "logic": "Standard multi-pack physical whiteboard markers on a dirty white background.",
    "background": "#F4F4F6",
    "key_hexes": [
      "#0033CC",
      "#009933",
      "#CC0000"
    ],
    "accent_hexes": [
      "#FF6600",
      "#660099",
      "#D3D6D9"
    ],
    "saturation": "High saturation for the marker ink against a desaturated background.",
    "contrast": "High contrast between the ink and the board.",
    "color_arc": "Static throughout."
  },
  "linework_edges": "Thick, uniform, rounded-cap strokes with slight internal opacity variations.",
  "character_design": {
    "present": true,
    "construction": "Simple, thick-lined blob-like humanoids with no necks and stubby, separated limbs.",
    "proportions": "Oversized heads, extremely simplified bodies.",
    "eyes_and_face": "Dot eyes, simple curved lines for mouths, no noses.",
    "consistency_tells": "Characters always face forward or slight 3/4 angle, constructed entirely of outlines with zero internal shading or fill color."
  },
  "environment_design": "A flat 2D whiteboard plane, occasionally featuring a vertical metallic seam splitting the board. Keep the board and its tray clear: no physical marker, pen, or eraser prop resting in the frame.",
  "typography": {
    "present": true,
    "headline_treatment": "Handwritten, slightly slanted whiteboard marker style.",
    "font_character": "Informal, mixed-case, uneven baseline, identical in thickness and texture to the character linework.",
    "placement": "Floating organically around the subjects to fill negative space.",
    "text_animation": "Rapid stroke-by-stroke draw-on reveal.",
    "caption_style": "None present."
  },
  "composition_framing": "Straight-on, flat, 2D locked-off framing.",
  "camera_language": "Static for individual beats, utilizing horizontal linear pans to move to new subjects rather than cutting.",
  "motion": {
    "cadence": "Stop-motion 12fps (on twos).",
    "fps_feel": "Jittery and tactile.",
    "easing": "Linear for pans, snappy for draw-ons.",
    "physics": "Rigid 2D, no organic squash and stretch, gravity does not affect the drawn elements.",
    "parallax_depth": "Zero parallax. Strictly 2D flat plane.",
    "signature_moves": [
      "Stroke-by-stroke draw-on reveals",
      "Instant one-frame element pops",
      "Line boiling jitter on static elements"
    ],
    "energy_level": "Bouncy and snappy."
  },
  "transitions_and_cuts": {
    "cut_style": "Spatial pans instead of hard cuts.",
    "avg_seconds_per_shot": "4 to 6 seconds between spatial pans.",
    "transition_types": [
      "Horizontal 2D camera pan",
      "Additive spatial composition (filling the board)"
    ]
  },
  "vfx": [
    "Drawn action lines (impact bursts, radiating heat lines, wavy cool air lines)",
    "Animated floating sleep Zs"
  ],
  "pacing_structure": "Hook question draws on -> rapid pop-in of supporting visuals -> spatial pan to new concept -> additive crowd buildup.",
  "audio": {
    "voiceover": {
      "present": true,
      "voice_character": "Casual, upbeat, curious, male.",
      "tone": "Educational but informal.",
      "pace_words_per_sec": "2.5"
    },
    "music": {
      "genre": "Quirky pizzicato underscore",
      "tempo_feel": "Mid-tempo, bouncy",
      "mood": "Lighthearted, inquisitive",
      "instrumentation": "Pizzicato strings, light plucky synths",
      "role": "Background texture, sits fully beneath VO and SFX"
    },
    "sfx": {
      "vocabulary": [
        "Squeaky whiteboard marker scribbles",
        "Dry pops (like a finger in a mouth)",
        "Airy whooshes for spatial pans"
      ],
      "sync_tightness": "Perfect frame-accurate sync with draw-on strokes and pop-ins."
    },
    "mix": "VO dominant, SFX highly prominent during visual changes, Music low and kept quiet."
  },
  "format": {
    "aspect_ratio": "16:9",
    "example_total_duration_seconds": "15",
    "resolution_feel": "Clean 1080p video of a dirty physical object."
  },
  "reproduce": {
    "recommended_image_model": "gpt-image-2",
    "image_style_block": "A flat, top-down view of a dirty dry-erase whiteboard surface (#F4F4F6) covered in faint, gray circular erase smudges. Hand-drawn doodle art using thick, bullet-tip dry-erase markers in vivid #0033CC blue, #009933 green, and #CC0000 red. The linework is thick with slight variable opacity mimicking real ink. Text is handwritten in the same marker style. The whiteboard and its tray are clear: no physical marker, pen, or eraser prop anywhere in the frame. No shading, no 3D elements, flat 2D layout.",
    "image_negative_prompt": "physical marker, expo marker, pen or eraser prop, real photographic objects on the board, 3D render, CGI, glossy, drop shadows, gradients, clean white background, paper texture, vector graphics, perfect lines, cinematic lighting, depth of field.",
    "recommended_video_model": "gemini-omni-flash",
    "motion_prompt_dna": "Simulate 12fps stop-motion animation. Add continuous 'line boiling' jitter to all drawn marker lines so they wiggle slightly frame to frame. Animate elements appearing through rapid, stroke-by-stroke draw-on reveals. Maintain the dirty whiteboard texture perfectly still while the drawings boil.",
    "audio_recipe": {
      "voice_direction": "Casual, upbeat, conversational explainer. Speak clearly but informally, like a friendly teacher.",
      "music_prompt": "Quirky, bouncy, pizzicato strings and light plucky synths, mid-tempo, instrumental modern ad underscore, lighthearted and inquisitive.",
      "sfx_prompt": "Squeaky dry-erase marker scribbling rapidly on a whiteboard, punctuated by distinct, cartoonish mouth-pops and a short, airy whoosh."
    }
  },
  "failure_modes": [
    "Model renders clean vector art instead of semi-transparent, slightly messy marker ink.",
    "Model forgets the dirty, smudged 'ghosting' background and makes it a pure #FFFFFF digital canvas.",
    "Video model applies smooth 24fps interpolation instead of the required 12fps jitter/boiling.",
    "Model attempts to add 3D depth, shading, or drop shadows to the characters instead of keeping them flat outlines."
  ],
  "best_use_cases": "Educational explainers, quick trivia hits, casual onboarding, or simplifying complex, abstract concepts."
}

```

### 4. Notes on the `reproduce` block

* **`image_style_block`:** We are utilizing `gpt-image-2` because of the heavy reliance on handwritten, baked-in text and the need for strict spatial adherence of labels and drawn elements. The prompt heavily emphasizes the *dirtiness* of the board, as models default to clean white backgrounds, and explicitly keeps the board clear of any physical marker or pen prop.
* **`image_negative_prompt`:** It is critical to forbid vectors, 3D, and gradients. AI models inherently want to clean up doodles or make them three-dimensional. Banning "paper texture" ensures we don't accidentally get a pencil sketch.
* **`motion_prompt_dna`:** Designed specifically for positive-phrasing image-to-video models like `gemini-omni-flash`. The instruction focuses entirely on the "boiling" (jitter) and the draw-on behavior while explicitly locking the background texture so it doesn't warp during generation.
* **Audio Recipe:** Separated clearly into the three composition layers. The SFX prompt targets the precise Foley (squeaks and pops) that sells the whiteboard illusion.

### 5. Failure modes

* **Vector Clean-up:** Generative models will aggressively try to turn "simple doodles" into pristine, scalable vector graphics. The guard is constantly referencing "variable opacity," "marker bleed," and "dry-erase ink" in the prompt.
* **Pristine Backgrounds:** Without extreme prompting, the background will default to pure #FFFFFF white. The guard is the explicit detailing of "#F4F4F6", "faint gray circular erase smudges", and "dirty".
* **Interpolation Smoothness:** Video models will try to make the motion fluid. If `gemini-omni-flash` fails to hold the 12fps stop-motion feel, the output will look like a Flash animation rather than a whiteboard doodle. The guard is the specific "Simulate 12fps stop-motion animation" and "continuous line boiling" instruction.
* **Shading/Volume:** The models may try to give the blob characters ambient occlusion or rounded shading. The negative prompt "no shading, no 3D elements, flat 2D layout" is the primary guard against this.
