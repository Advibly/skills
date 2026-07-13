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

A warm, whimsical, low-poly 3D world defined by un-smoothed, flat-shaded geometric facets, smooth continuous camera glides, and a childlike, acoustic storybook soundscape.

### 2. Prose breakdown

The visual anchor of this style is deliberate, un-smoothed low-poly 3D modeling. Every element—from the organic characters to the terrain and clouds—is constructed from visible, rigid polygons (primarily triangles and quads). The rendering engine utilizes flat shading without Phong or Gouraud smoothing, leaving every geometric edge crisp and distinct. Materials are overwhelmingly matte and non-reflective, mimicking the texture of crisp folded paper or unpainted matte plastic, which prevents the 3D from feeling hyper-realistic or overly computer-generated. The single exception to this matte rule is reserved for key narrative "liquid" elements (like nectar or honey), which utilize high gloss, translucency, and a subtle self-illuminating glow to draw the eye.

Lighting is soft but highly directional, simulating a perpetual "golden hour." A strong, warm key light casts sharp, geometric shadows that emphasize the faceted nature of the models, while high ambient global illumination ensures the shadows never fall to pure black, keeping the mood light and inviting. The color palette relies heavily on analogous warm tones—goldenrod, pale yellow, soft sage green, and warm pinks—with a very low-contrast, low-saturation sky that allows the foreground subjects to pop.

Motion is rendered at a buttery smooth 24/30fps, contrasting with the blocky visual style. There is no stop-motion stutter; character rigging allows for fluid, continuous movement, even if the body parts themselves are rigid geometric blocks. The camera language is equally smooth, relying on slow, motorized push-ins and gentle tracking shots that establish a deep sense of parallax within the expansive 3D environments. The audio completes the storybook aesthetic with a slow, innocent child voiceover, underpinned by a sparse, plucked-string acoustic track and gentle, non-intrusive sound effects that prioritize warmth over realism.

### 3. Style spec

```json
{
  "style_id": "warm-low-poly-storybook",
  "display_name": "Warm Low-Poly Storybook",
  "one_line_signature": "A warm, whimsical, low-poly 3D world defined by un-smoothed, flat-shaded geometric facets, smooth continuous camera glides, and a childlike acoustic soundscape.",
  "example_subject_only": "A bee collecting nectar from a flower and returning to a geometric hive.",
  "medium": "Low-poly 3D animation",
  "texture_and_render": "Flat-shaded polygons with no edge smoothing. Overwhelmingly matte surfaces resembling folded paper or unpainted matte plastic, with selective translucent/glossy materials used strictly for liquid or magical accents.",
  "lighting": "Warm, directional golden-hour key light casting sharp, geometric shadows, balanced by high-intensity warm ambient light to keep shadows soft and colorful.",
  "palette": {
    "logic": "Analogous warm hues with soft pastel environments to make saturated foreground subjects pop.",
    "background": "#FCEEB5",
    "key_hexes": [
      "#E2B02A",
      "#D87093",
      "#8FBC8F",
      "#2F4F4F"
    ],
    "accent_hexes": [
      "#FFD700",
      "#FFFFFF"
    ],
    "saturation": "Moderate-high for subjects, low-moderate for environments.",
    "contrast": "Moderate; relies on color separation rather than deep shadow contrast.",
    "color_arc": "Maintains a consistent, warm, golden wash throughout."
  },
  "linework_edges": "No drawn linework. Edges are defined purely by the sharp intersections of flat-shaded 3D polygons.",
  "character_design": {
    "present": true,
    "construction": "Assembled from distinctly visible, un-smoothed geometric blocks and triangles.",
    "proportions": "Stylized and slightly chunky, with oversized heads and simplified appendages.",
    "eyes_and_face": "Large, flat, faceted eyes integrated directly into the low-poly mesh, using contrasting colors rather than detailed textures.",
    "consistency_tells": "Every body part, no matter how small (e.g., antennae), maintains the faceted, polygonal construction."
  },
  "environment_design": "Expansive, undulating terrain made of large triangular facets. Deep Z-space with distinct foreground, midground, and background layers to maximize parallax.",
  "typography": {
    "present": true,
    "headline_treatment": "None present in reference.",
    "font_character": "Clean, modern, sans-serif (similar to Roboto or Arial), white with a very subtle drop shadow for readability.",
    "placement": "Bottom center, standard subtitle positioning.",
    "text_animation": "Hard cuts or simple dissolves synced exactly with the spoken words.",
    "caption_style": "Standard closed-caption style, no kinetic typography."
  },
  "composition_framing": "Wide establishing shots moving into tight macro close-ups. Center-weighted framing for primary subjects.",
  "camera_language": "Smooth, motorized continuous movement. Gentle forward push-ins, slow tracking pans, and slight orbits. No handheld shake.",
  "motion": {
    "cadence": "Continuous and fluid.",
    "fps_feel": "Smooth 24fps or 30fps interpolation.",
    "easing": "Gentle ease-in and ease-out on all camera and character movements.",
    "physics": "Rigid bodies. No squash or stretch. Movement comes from pivoting joints rather than mesh deformation.",
    "parallax_depth": "High. Environments are built deep to showcase relative motion during camera tracking.",
    "signature_moves": [
      "The slow, continuous forward push-in.",
      "Smooth lateral tracking alongside a moving subject."
    ],
    "energy_level": "Calm, deliberate, and relaxing."
  },
  "transitions_and_cuts": {
    "cut_style": "Hard cuts, occasionally utilizing match cuts on action or subject position.",
    "avg_seconds_per_shot": "4 to 6 seconds.",
    "transition_types": [
      "Hard cut",
      "Match cut"
    ]
  },
  "vfx": [
    "Subtle glowing translucency on specific liquid/energy elements to contrast the matte world."
  ],
  "pacing_structure": "Slow, methodical build. Establish environment -> show action -> reveal consequence/destination.",
  "audio": {
    "voiceover": {
      "present": true,
      "voice_character": "Young female child, innocent, clear, storybook-narrator tone.",
      "tone": "Gentle and educational.",
      "pace_words_per_sec": "1.5 to 2"
    },
    "music": {
      "genre": "Acoustic whimsical instrumental.",
      "tempo_feel": "Slow, relaxing, approx 80-90 BPM.",
      "mood": "Warm, innocent, uplifting.",
      "instrumentation": "Plucked acoustic guitar or ukulele, light glockenspiel or bells.",
      "role": "Underscore, providing emotional warmth without distracting from the VO."
    },
    "sfx": {
      "vocabulary": [
        "Stylized, soft insect buzzing",
        "Gentle ambient wind",
        "Magical, warm shimmering chime for liquid appearance"
      ],
      "sync_tightness": "Loose for ambiance, tight for specific actions (like the nectar appearing)."
    },
    "mix": "VO is front and center. Music is heavily kept quiet, sitting comfortably in the background. SFX are subtle and blended into the music track."
  },
  "format": {
    "aspect_ratio": "16:9",
    "example_total_duration_seconds": "15",
    "resolution_feel": "Crisp 1080p or 4K, zero film grain or degradation."
  },
  "reproduce": {
    "recommended_image_model": "gpt-image-2",
    "image_style_block": "Low-poly 3D render, un-smoothed flat-shaded geometric facets, crisp polygonal edges. Matte materials like folded paper or unpainted plastic. Warm golden-hour directional lighting, sharp geometric shadows, high ambient warm illumination. Analogous warm pastel color palette. Clean, crisp, high resolution, no film grain.",
    "image_negative_prompt": "No smooth shading, no curved surfaces, no realistic textures, no photorealism, no glossy materials (unless specified), no subsurface scattering, no film grain, no depth of field blur, no 2D illustration.",
    "recommended_video_model": "gemini-omni-flash",
    "motion_prompt_dna": "Smooth, fluid, continuous 24fps motion. Slow, gentle forward camera push-in and lateral tracking. Deep parallax in the environment. Maintain absolute structural consistency of the crisp, flat-shaded low-poly geometric facets and sharp polygonal edges throughout the motion. Calm, relaxing cadence.",
    "audio_recipe": {
      "voice_direction": "Young child female voice, innocent, slow pace, storybook narrator, gentle tone.",
      "music_prompt": "Gentle acoustic whimsical instrumental, slow tempo, plucked ukulele, light glockenspiel, warm, innocent, modern ad underscore.",
      "sfx_prompt": "Soft stylized buzzing, gentle wind ambiance, warm magical shimmer."
    }
  },
  "failure_modes": [
    "The model applies Phong or Gouraud smoothing to the 3D meshes, losing the flat-shaded faceted look.",
    "The lighting becomes too realistic, introducing complex subsurface scattering or HDR reflections that ruin the matte, paper-like aesthetic.",
    "The video model introduces stop-motion jitter, misunderstanding the 'low-poly' prompt as 'claymation' or 'stop-motion'. Guard this by explicitly prompting for 'smooth fluid 24fps motion'."
  ],
  "best_use_cases": "Educational content for children, eco-friendly product explainers, or brand stories emphasizing simplicity, warmth, and grassroots origins."
}

```
