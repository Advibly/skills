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

A cozy, pastel-toned digital illustration style with pervasive soft bloom lighting, featuring rounded, limbless kawaii subjects, floating four-pointed sparkles, and gentle squash-and-stretch looping animation.

### 2. Prose breakdown

**Medium, Texture, and Lighting:**
This style mimics a high-polish, cozy indie game aesthetic achieved through smooth 2D digital painting with soft airbrushed shading. There are no harsh, dark outlines; objects are separated by contrasting pastel values and soft, diffused rim lighting. A strong "bloom" or soft-focus glow effect is applied to highlights and particle effects, giving the entire frame a dreamy, magical quality. Textures are perfectly smooth, resembling soft matte plastic or velvet, devoid of grit, grain, or harsh specular reflections.

**Character Design and Environment:**
Subjects follow strict kawaii conventions: rounded, plump shapes devoid of limbs, featuring wide-set, pure black dot eyes, tiny curved mouths, and soft pink blush marks directly under the eyes. Environments are constructed using deep parallax layers. Foregrounds are in sharp, glowing focus, while backgrounds feature a shallow depth-of-field blur to establish scale and keep attention on the primary subjects.

**Color Palette and Motion:**
The palette is dominated by soft pastels—baby blues, blush pinks, and warm creams—contrasted against warm, golden-hour ambient light in the background. The motion cadence is a smooth 24-30fps, characterized by gentle, rhythmic squash-and-stretch bouncing for the characters. Secondary animations include smooth fluid dynamics and slow, drifting four-pointed star sparkles. The pacing is deliberate and calming, matched by a soothing, slow-paced voiceover and a plucky, music-box instrumental track.

### 3. Style spec

```json
{
  "style_id": "cozy_kawaii_bloom_2d",
  "display_name": "Cozy Kawaii Soft Bloom",
  "one_line_signature": "Soft digital pastel illustration with heavy bloom lighting, kawaii facia, and gentle bouncing animation.",
  "example_subject_only": "An educational breakdown of how a refrigerator works with living food.",
  "medium": "Polished 2D digital painting/vector hybrid with soft-focus glow.",
  "texture_and_render": "Smooth matte surfaces, airbrushed shading, zero grain, high global bloom on highlights.",
  "lighting": "Diffused, soft ambient lighting with gentle rim lights and glowing particle highlights.",
  "palette": {
    "logic": "Cool pastel foregrounds contrasted against warm, golden-hour blurred backgrounds.",
    "background": "#FDFBF7",
    "key_hexes": ["#A6C8E0", "#F4C2C2", "#98FF98"],
    "accent_hexes": ["#FFD700", "#FF8C00"],
    "saturation": "Moderate-high but brightened into pastels.",
    "contrast": "Low harsh contrast; forms defined by color shifts rather than deep shadows.",
    "color_arc": "Consistent cozy pastel warmth throughout."
  },
  "linework_edges": "No black linework. Edges defined by color blocking and soft rim lighting.",
  "character_design": {
    "present": true,
    "construction": "Limbless, rounded geometric blobs.",
    "proportions": "Short, wide, bottom-heavy for stability.",
    "eyes_and_face": "Wide-set black dot eyes, tiny curved mouth, soft pink oval blush marks.",
    "consistency_tells": "Every subject has the exact same facial ratio and lacks appendages."
  },
  "environment_design": "Layered 2D planes with simulated 3D depth of field; background heavily blurred.",
  "typography": {
    "present": true,
    "headline_treatment": "None present.",
    "font_character": "Clean, rounded sans-serif.",
    "placement": "Bottom center subtitle band.",
    "text_animation": "Static per phrase.",
    "caption_style": "White text with a soft dark drop shadow for readability."
  },
  "composition_framing": "Symmetrical, wide medium shots keeping all subjects fully in frame.",
  "camera_language": "Locked-off static camera, allowing internal frame motion to carry energy.",
  "motion": {
    "cadence": "Smooth 24fps.",
    "fps_feel": "Fluid and continuous.",
    "easing": "Sine ease-in-out.",
    "physics": "Buoyant, low-gravity feel. Soft squash and stretch.",
    "parallax_depth": "Minimal camera parallax, high focus-blur depth.",
    "signature_moves": ["Gentle vertical bobbing", "Floating sparkling particles", "Smooth liquid flow paths"],
    "energy_level": "Calm, soothing, ambient."
  },
  "transitions_and_cuts": {
    "cut_style": "Straight hard cuts.",
    "avg_seconds_per_shot": "4 to 6 seconds",
    "transition_types": ["Hard cut"]
  },
  "vfx": ["Four-pointed twinkling star sparkles", "Soft glowing directional arrows", "Translucent rising steam bubbles"],
  "pacing_structure": "Slow, methodical introduction leading to a cyclic resolution.",
  "audio": {
    "voiceover": {
      "present": true,
      "voice_character": "Warm, gentle, educational male narrator.",
      "tone": "Calm, reassuring, lullaby-like.",
      "pace_words_per_sec": "2.5"
    },
    "music": {
      "genre": "Ambient cozy acoustic.",
      "tempo_feel": "Slow, drifting.",
      "mood": "Relaxing, magical, safe.",
      "instrumentation": "Plucked acoustic guitar, music box, glockenspiel.",
      "role": "Background mood-setter, kept quiet under VO."
    },
    "sfx": {
      "vocabulary": ["Magical chimes", "Soft bubbling", "Gentle whooshes"],
      "sync_tightness": "Loose, ambient sync rather than hard frame-matched hits."
    },
    "mix": "VO highly dominant. Music low and static-leveled. SFX sparse and bright."
  },
  "format": {
    "aspect_ratio": "16:9",
    "example_total_duration_seconds": "27",
    "resolution_feel": "1080p crisp digital."
  },
  "reproduce": {
    "recommended_image_model": "gpt-image-2",
    "image_style_block": "Cozy kawaii 2D digital illustration. Soft pastel color palette, smooth matte textures with airbrushed shading. High bloom and soft-focus glow. No harsh outlines. Subjects are rounded, limbless, with wide-set black dot eyes and blush. Warm blurred background depth-of-field. Magical four-pointed star sparkles in the air.",
    "image_negative_prompt": "Harsh shadows, black outlines, 3D render, glossy plastic, gritty texture, complex realistic faces, limbs, high contrast.",
    "recommended_video_model": "gemini-omni-flash",
    "motion_prompt_dna": "Smooth fluid animation. The main subjects gently bounce with soft squash-and-stretch. Magical four-pointed star sparkles slowly drift and twinkle in the foreground. Maintain the soft bloom lighting and blurred background depth. Calm, soothing ambient movement.",
    "audio_recipe": {
      "voice_direction": "Male narrator, slow pace, soft and reassuring tone, like reading a bedtime story.",
      "music_prompt": "Ambient cozy acoustic instrumental, slow tempo, relaxing, featuring plucked guitar and music box glockenspiel, modern ad underscore.",
      "sfx_prompt": "Soft magical twinkling chimes, gentle water bubbling."
    }
  },
  "failure_modes": [
    "Models adding limbs or complex human features to the kawaii subjects.",
    "Over-rendering the textures into glossy 3D CGI instead of flat soft 2D.",
    "Losing the soft-focus bloom lighting, resulting in harsh, clinical visuals.",
    "Motion models generating chaotic, fast movement instead of gentle, looping bobs."
  ],
  "best_use_cases": "Gentle educational explainers, cozy product introductions, mental health or wellness apps."
}

```

### 4. Notes on the `reproduce` block

* **`image_style_block`**: Use this exact string for `gpt-image-2` as it aggressively forces the soft-focus, line-less kawaii aesthetic. It ensures the environment blur and the character facial ratios remain consistent across generations.
* **`image_negative_prompt`**: Crucial for preventing the image models from defaulting to standard 3D Pixar-style renders. The term "black outlines" prevents it from looking like a standard anime cell.
* **`motion_prompt_dna`**: Designed for `gemini-omni-flash`. It focuses entirely on the "squash-and-stretch" and "drifting sparkles" because these are the sole drivers of life in this otherwise static style.
* **`audio_recipe`**: The music prompt emphasizes "music box" and "cozy acoustic" to nail that lullaby tone.
* **Recommended Models**: `gpt-image-2` is chosen for the base images because it handles the specific face ratios (dot eyes, no limbs) better than texture-heavy models. `gemini-omni-flash` is best for the video because the motion is strictly ambient and looping; we do not need precise end-frame target matching for these shots.

### 5. Failure modes

1. **Anatomy hallucination**: Generative image models will desperately try to add arms and legs to the characters. The prompt must strictly enforce "limbless, rounded geometric blobs."
2. **Texture drift**: Image models may output glossy, clay, or 3D-rendered textures. The guard is the negative prompt blocking "3D render, glossy, CGI" and enforcing "2D digital painting."
3. **Harsh lighting**: The magical, cozy vibe relies entirely on the soft-focus bloom. If the model generates sharp shadows, the video will feel clinical. Ensure "soft bloom" and "airbrushed" are heavily weighted.
4. **Hyperactive motion**: Video models may make the characters move too fast or warp. The motion prompt must emphasize "gentle," "slow," and "soft squash-and-stretch" to maintain the calm pacing.
