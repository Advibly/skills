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

A tactile, vibrant plasticine stop-motion style featuring visible fingerprint impressions, chunky rounded character designs, in-camera sculpted typography, and a distinct 12fps "boiling" animation cadence.

### 2. Prose breakdown

The visual foundation is a physical, hand-sculpted plasticine stop-motion medium. Every surface exhibits a matte finish with visible, persistent thumbprint indentations, slight imperfections, and a lack of smooth specular highlights, grounding the aesthetic in tangible craft. Environments are constructed as miniature, multi-plane dioramas, utilizing forced perspective with relief-sculpted backdrops and physical substitute materials like cotton wool for smoke or clouds. The lighting mimics miniature studio setups, casting soft, directional shadows that emphasize the dimensional, bumpy texture of the clay.

Character and asset design relies on chunky, rounded geometry with zero sharp edges. Subjects are assembled from modular clay forms—stubby limbs attached to thick bodies—and feature exaggerated, protruding ping-pong ball eyes with small, flat black pupils. Typography is treated as a physical object within the world; letters are extruded, unevenly rolled clay shapes that interact with the scene's lighting and cast shadows onto the background, rather than flat graphic overlays.

The animation language is defined by a low-framerate, on-twos (roughly 12fps) stop-motion cadence. This introduces a signature "boiling" effect where the texture and micro-geometry of the subjects jitter slightly between frames, simulating the animator's manual manipulation. Transitions are straightforward hard cuts. The aesthetic relies heavily on physical VFX; impacts are represented by frozen, sculpted splashes of clay and suspended debris rather than digital particles.

Audio mirrors the physical miniature aesthetic. The voiceover is energetic and narrative-driven, sitting above a dynamic instrumental track that shifts instrumentation based on the scene's mood (whimsical pizzicato strings to dramatic low brass). Sound effects are tactile and material-specific—squishing, soft thuds, and organic rustling—syncing closely with the low-framerate physical movements on screen, avoiding overly realistic or aggressive cinematic impacts.

### 3. Style spec

```json
{
  "style_id": "plasticine_stop_motion_diorama",
  "display_name": "Tactile Claymation",
  "one_line_signature": "Vibrant plasticine stop-motion with visible fingerprints, chunky characters, sculpted text, and 12fps jitter.",
  "example_subject_only": "A green dinosaur facing a meteor impact.",
  "medium": "Physical plasticine stop-motion photography with mixed-media accents.",
  "texture_and_render": "Matte clay with visible fingerprint dents, tool marks, and slight surface smudges. No digital gloss.",
  "lighting": "Miniature studio lighting. Soft directional key light, warm fill, generating short, soft drop shadows.",
  "palette": {
    "logic": "High-saturation primary/secondary colors against contrasting backgrounds.",
    "background": "Textured flat blue (#2C82C9) or painted gradient backdrops.",
    "key_hexes": ["#4CAF50", "#FF9800", "#2C82C9", "#8D6E63"],
    "accent_hexes": ["#FFC107", "#E91E63"],
    "saturation": "High saturation, vibrant.",
    "contrast": "Medium-high, driven by soft shadows.",
    "color_arc": "Vibrant warm -> Monochromatic desaturated ash -> Vibrant warm/sunny."
  },
  "linework_edges": "No linework. Edges are rounded, soft, and slightly irregular.",
  "character_design": {
    "present": true,
    "construction": "Modular rolled clay pieces, chunky, stubby, no sharp angles.",
    "proportions": "Exaggerated heads/features, short thick limbs.",
    "eyes_and_face": "Protruding white spheres, small black dot pupils, sculpted mouths.",
    "consistency_tells": "Surface fingerprints shift per frame, overall volume remains stable."
  },
  "environment_design": "Miniature diorama. Multi-plane flat layers, sculpted relief details, physical prop elements.",
  "typography": {
    "present": true,
    "headline_treatment": "Physical 3D extruded clay letters baked into the set.",
    "font_character": "Chunky, bubbly, uneven baseline, hand-rolled.",
    "placement": "Floating in upper thirds, casting shadows on the backdrop.",
    "text_animation": "Static, but boils with the 12fps stop-motion texture jitter.",
    "caption_style": "None."
  },
  "composition_framing": "Center-weighted, medium shots, clear silhouetting against backdrops.",
  "camera_language": "Static lockdown or very slow, steady push-ins.",
  "motion": {
    "cadence": "On-twos (12fps), constant texture boiling.",
    "fps_feel": "Low framerate, slightly jittery.",
    "easing": "Linear or slight ease-in/out, physical limitation feel.",
    "physics": "Stiff clay physics, no natural fluid dynamics.",
    "parallax_depth": "Shallow parallax, mimicking closely spaced diorama layers.",
    "signature_moves": ["Texture boiling on static holds", "Stiff pivot rotations"],
    "energy_level": "Whimsical, deliberate, mechanical."
  },
  "transitions_and_cuts": {
    "cut_style": "Hard cuts.",
    "avg_seconds_per_shot": "4 to 6 seconds.",
    "transition_types": ["Hard cut"]
  },
  "vfx": ["Sculpted clay splash impacts", "Suspended clay debris", "Cotton wool smoke trails"],
  "pacing_structure": "Setup (vibrant) -> Inciting Incident (impact) -> Resolution (vibrant). Slow, deliberate beats.",
  "audio": {
    "voiceover": {
      "present": true,
      "voice_character": "Upbeat, warm, male storyteller.",
      "tone": "Narrative, educational, engaging.",
      "pace_words_per_sec": "2.5"
    },
    "music": {
      "genre": "Acoustic orchestral.",
      "tempo_feel": "Mid-tempo (90-100 BPM), shifts dynamically.",
      "mood": "Whimsical to dramatic to uplifting.",
      "instrumentation": "Pizzicato strings, marimba, low brass, acoustic guitar.",
      "role": "Drives the emotional arc, sits quietly under VO."
    },
    "sfx": {
      "vocabulary": ["Squishy clay impacts", "Soft thuds", "Airy whistles", "Bird chirps"],
      "sync_tightness": "Loose, hits key visual actions only."
    },
    "mix": "VO dominant, dynamic music kept quietly beneath, SFX layered lightly."
  },
  "format": {
    "aspect_ratio": "16:9",
    "example_total_duration_seconds": "15",
    "resolution_feel": "Crisp 4K, sharp focus on macro details."
  },
  "reproduce": {
    "recommended_image_model": "nano-banana-2",
    "image_style_block": "Macro photography of a plasticine stop-motion diorama. Matte clay textures with visible fingerprint indentations and tool marks. Chunky, rounded modular designs with no sharp edges. Protruding spherical eyes. Built as a miniature physical set with forced perspective. Lighting is soft, directional miniature studio lighting creating short drop shadows. If text is present, it must be physical, 3D extruded clay letters baked into the scene and interacting with the lighting.",
    "image_negative_prompt": "No 3D CGI render, no digital gloss, no smooth plastic, no realistic textures, no thin linework, no cinematic depth of field blurring, no flat graphic text overlays.",
    "recommended_video_model": "gemini-omni-flash",
    "motion_prompt_dna": "The camera is locked off with a very slow, steady push-in. The characters move with stiff, physical pivots and deliberate pose-to-pose actions. Maintain subtle manual texture repositioning in the clay, fingerprints, and micro-geometry. Any VFX should look like physically sculpted clay shapes holding in space. The final composition supplies the on-twos cadence.",
    "audio_recipe": {
      "voice_direction": "Warm, engaging, upbeat male storyteller, steady and clear narration.",
      "music_prompt": "Instrumental, modern ad underscore. Whimsical acoustic track transitioning from pizzicato strings and marimba to a gentle acoustic guitar.",
      "sfx_prompt": "Tactile, squishy physical clay handling sounds, soft dull thuds, organic rustling."
    }
  },
  "failure_modes": [
    "Model smoothes out the clay, resulting in a generic 3D CGI plastic look rather than hand-sculpted plasticine.",
    "Source motion may interpolate smoothly; this is expected. Apply frame_cadence: on_twos in the final composition to create the critical 12fps feel.",
    "Typography is rendered as a clean, digital 2D overlay rather than bumpy, physical 3D clay letters casting shadows.",
    "Introduction of realistic textures (e.g., real fur, realistic water) that break the miniature, mixed-media diorama illusion."
  ],
  "best_use_cases": "Playful brand storytelling, educational explainers, whimsical product introductions."
}

```

### 4. Notes on the `reproduce` block

* **`image_style_block`**: Designed to force physical imperfections. By explicitly demanding "fingerprint indentations" and "miniature studio lighting", it prevents models from defaulting to Pixar-style smooth 3D rendering.
* **`image_negative_prompt`**: Crucial for stripping out digital sheen. Forbidding "cinematic depth of field" ensures the background layers remain readable, mimicking the physical diorama backdrop seen in the reference.
* **`motion_prompt_dna`**: Focuses on stiff physical pivots and manual texture behavior. The source may remain smooth; `frame_cadence: "on_twos"` on the final composition supplies the stable stop-motion cadence.
* **`audio_recipe`**: Breaks the audio into discrete layers. The SFX prompt specifically asks for "tactile, squishy" sounds to ground the visual medium in the audio mix.
* **Model Selection**: `nano-banana-2` is chosen for image generation because holding organic, imperfect textures like clay grain and fingerprints is its primary strength. `gemini-omni-flash` is selected for video because image-to-video from a single frame is ideal for establishing a static diorama scene with subtle, continuous texture boiling.

### 5. Failure modes

1. **Digital Smoothing (The "Toy Story" effect):** AI image models often "clean up" noise. If the prompt lacks heavy emphasis on fingerprints and tool marks, the result will look like slick, mass-produced plastic toys instead of hand-sculpted clay. *Guard: Over-index on words like "smudged", "thumbprints", and "uneven" in the prompt.*
2. **Smooth Interpolation:** Video models naturally create fluid source motion. *Guard: Keep "stiff, physical pivots" in the motion prompt and pass `frame_cadence: "on_twos"` to the final composition.*
3. **Floating Typography:** Models struggle to integrate text physically into scenes. They often default to superimposing clean vector fonts over the image. *Guard: Use the phrase "physical 3D extruded clay letters baked into the scene" and demand drop shadows.*
4. **Scale Confusion:** The illusion relies on everything looking miniature. If the model introduces expansive landscapes or atmospheric haze, the diorama feel is lost. *Guard: Reinforce "miniature set", "macro photography", and "flat background layers".*
