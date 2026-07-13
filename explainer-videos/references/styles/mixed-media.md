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

A tactile, 2.5D paper-cut collage animation featuring greyscale photographic clippings with thick white scissor-cut borders, animated on-twos with harsh drop shadows over textured construction-paper backgrounds, punctuated by marker scribbles, sticky notes, and mechanical sound design.

### 2. Prose breakdown

The visual foundation is a physical mixed-media paper collage mimicking a multiplane camera setup. Subjects are constructed from high-contrast, desaturated greyscale photographs cut out with intentionally uneven, thick white paper borders. These sit atop flat, heavily textured backgrounds resembling coarse construction paper or painted canvas. Depth is achieved entirely through stark, hard-edged black drop shadows offset diagonally, creating a 2.5D orthographic layering effect without any actual 3D perspective or foreshortening.

Motion is characterized by a low-framerate, stop-motion aesthetic, mimicking 12fps (animated on-twos) with a slight, continuous jitter or "boil" to the elements, even when holding still. Visual effects and infographics are highly physicalized: motion lines are drawn as rough, bleeding red and blue marker strokes; impact moments use jagged, brightly colored paper-cut starbursts; and typography is delivered via physical objects like yellow sticky notes with stamped black text, torn masking tape with red marker handwriting, and chunky 3D cardboard blocks for numbers.

The audio profile heavily reinforces the tactile, handcrafted visual medium. The sound effects vocabulary is entirely diegetic to the medium—loud paper sliding, masking tape tearing, marker squeaks, heavy cardboard thuds, and mechanical clunks for text reveals. This sits atop a driving, tension-building percussive underscore. The voiceover is a fast-paced, authoritative yet energetic female voice, cutting through the mix to deliver punchy educational beats at roughly 3 words per second, keeping the music slightly on hard consonants.

### 3. Style spec

```json
{
  "style_id": "paper_collage_stop_motion_01",
  "display_name": "Tactile Paper-Cut Multiplane",
  "one_line_signature": "A tactile, 2.5D paper-cut collage animation featuring greyscale photographic clippings with thick white borders, harsh drop shadows, marker scribbles, and sticky-note typography.",
  "example_subject_only": "An airplane explaining aerodynamic lift.",
  "medium": "Mixed media paper collage and cut-out animation",
  "texture_and_render": "Heavy paper grain, visible torn edges with white paper fiber showing, matte finish with absolutely no specular highlights, rough marker bleed on paper.",
  "lighting": "Flat, even lighting on the textures themselves, but with intense, hard-edged black cast shadows offset to the bottom right to create deep 2.5D multiplane separation.",
  "palette": {
    "logic": "Desaturated, greyscale focal subjects against muted, textured pastel/earth-tone backgrounds, interrupted by highly saturated primary-color accents for data/VFX.",
    "background": "#7AB2E1 (mottled sky blue) to #4A4A4A (dark stormy grey) or #C8A278 (warm cardboard brown)",
    "key_hexes": ["#FFFFFF", "#000000", "#EAEAEA"],
    "accent_hexes": ["#D9423E", "#4A90E2", "#FAD02C"],
    "saturation": "Low saturation for subjects and environments, 100% saturation for informational accents (markers/sticky notes).",
    "contrast": "Extremely high contrast due to the stark black drop shadows and pure white clipping borders.",
    "color_arc": "Shifts dynamically based on mood, e.g., bright pastel blue to dark stormy grey to warm sunset orange."
  },
  "linework_edges": "No drawn outlines on subjects; edges are defined by physical, uneven, chunky white paper borders simulating sloppy scissor cuts.",
  "character_design": {
    "present": false,
    "construction": "N/A",
    "proportions": "N/A",
    "eyes_and_face": "N/A",
    "consistency_tells": "N/A"
  },
  "environment_design": "Composed of flat, overlapping 2D shapes (like clouds) on distinct z-depth planes, colored with construction paper textures and separated by heavy drop shadows.",
  "typography": {
    "present": true,
    "headline_treatment": "Red marker handwriting on torn strips of beige masking tape, or chunky 3D cardboard block letters for numbers.",
    "font_character": "Rough, hand-drawn sans-serif marker fonts and vintage typewriter stamp fonts.",
    "placement": "Pinned directly next to the subject with physical connector lines, or floating as separate paper elements.",
    "text_animation": "Snaps in instantly via hard cuts or pop-on scaling with no motion blur.",
    "caption_style": "N/A"
  },
  "composition_framing": "Orthographic, flat 2D perspective, centered framing, side-profile orientation for subjects.",
  "camera_language": "Mostly locked-off static shots, relying on the elements moving through the frame, or very slow, perfectly linear 2D pans.",
  "motion": {
    "cadence": "Stop-motion animated on-twos",
    "fps_feel": "12fps with subtle element jitter/boil",
    "easing": "Linear mechanical movements, snappy pops, zero organic squash-and-stretch.",
    "physics": "Rigid paper physics; elements slide, snap, or drop in as stiff boards.",
    "parallax_depth": "High 2.5D multiplane parallax; foreground elements move significantly faster than background elements.",
    "signature_moves": ["Pop-on sticky notes", "Sliding paper cutouts", "Marker drawing on instantly"],
    "energy_level": "High-paced and mechanical"
  },
  "transitions_and_cuts": {
    "cut_style": "Hard flash cuts or fast sliding replacements.",
    "avg_seconds_per_shot": "4 to 6 seconds; pack fast collage swaps inside each clip",
    "transition_types": ["Hard cut", "Full-frame slide-in replacement", "Environment color swap behind static subject"]
  },
  "vfx": ["Jagged yellow/red paper-cut starbursts for impacts", "Squiggly red/blue marker lines for wind/force vectors", "Torn paper edge overlays"],
  "pacing_structure": "Rapid-fire hook establishing scale -> Problem introduction via environment shift -> Solution breakdown via colored infographics -> Hero payoff shot holding steady.",
  "audio": {
    "voiceover": {
      "present": true,
      "voice_character": "Energetic, educational, slightly dramatic female voice.",
      "tone": "Authoritative but accessible, urgent.",
      "pace_words_per_sec": "3.5"
    },
    "music": {
      "genre": "Tension-building percussive orchestration",
      "tempo_feel": "120 BPM, driving forward momentum",
      "mood": "Urgent, scientific, mildly epic",
      "instrumentation": "Staccato strings, heavy taiko/tympani drum hits, ticking hi-hats.",
      "role": "Drives the edit, builds underlying tension before the final reveal."
    },
    "sfx": {
      "vocabulary": ["Thick paper sliding", "Masking tape tearing", "Marker squeaks on paper", "Mechanical clunks", "Heavy cardboard thuds"],
      "sync_tightness": "Frame-accurate to every visual pop-on and text reveal."
    },
    "mix": "SFX are prioritized extremely high in the mix, almost matching the VO in volume during transitions. Music is kept quiet heavily by the VO."
  },
  "format": {
    "aspect_ratio": "16:9",
    "example_total_duration_seconds": "15",
    "resolution_feel": "Crisp 4K to highlight paper grain and textures"
  },
  "reproduce": {
    "recommended_image_model": "gpt-image-2",
    "image_style_block": "A flat 2.5D mixed-media paper-cut collage. The main subject is a desaturated greyscale photograph isolated with a thick, rough, uneven white paper border, as if hastily cut out with scissors. The subject is placed on a flat, heavily textured, grainy construction-paper background. There is a harsh, stark black drop shadow cast directly behind the subject and environment elements, creating a multiplane depth effect. Include accents of bright red and blue rough marker scribbles on the paper. High contrast, orthographic perspective, tactile craft aesthetic.",
    "image_negative_prompt": "No 3D rendering, no smooth gradients, no specular highlights, no glossy materials, no depth of field, no realistic perspective, no soft shadows, no digital glow, no smooth vector art.",
    "recommended_video_model": "gemini-omni-flash",
    "motion_prompt_dna": "Animate with rigid pose-to-pose papercraft movement. Maintain absolute stiffness in the paper elements with no organic bending. Slide elements linearly across the screen. Keep the heavy drop shadows locked to the elements. Introduce subtle manual placement jitter in the paper. The final composition supplies the on-twos cadence.",
    "audio_recipe": {
      "voice_direction": "Female, fast-paced, educational, punchy, dramatic articulation on key numbers and statistics.",
      "music_prompt": "Instrumental, modern ad underscore, driving staccato strings, heavy ticking percussion, building tension, 120 BPM.",
      "sfx_prompt": "Diegetic physical sounds: tearing thick masking tape, squeaky wet marker on dry paper, heavy cardboard dropping on a wooden table, mechanical typewriter clunks."
    }
  },
  "failure_modes": [
    "AI generates 3D volumetric shadows instead of flat, duplicate-layer drop shadows.",
    "Source motion may interpolate smoothly; this is expected. Apply frame_cadence: on_twos in the final composition for the tactile cadence.",
    "Image model blends the subject into the background rather than keeping the distinct, sloppy white scissor-cut border.",
    "AI attempts to add standard particle VFX (smoke, fire, sparks) instead of physical paper-cut representations of those effects."
  ],
  "best_use_cases": "Explaining hidden mechanics, physics, data-heavy comparisons, or historical timelines where physicalizing abstract concepts aids comprehension."
}

```

### 4. Notes on the `reproduce` block

* **`image_style_block`**: This block forces the model to prioritize the physical edges (the "uneven white paper border") and the lighting style ("stark black drop shadow") which are the two most critical tells of this style.
* **`image_negative_prompt`**: Crucial for stripping out AI's default bias towards soft lighting, bokeh, and volumetric rendering. We need it absolutely flat and matte.
* **`motion_prompt_dna`**: We must use `gemini-omni-flash` for its positive-phrasing adherence to image-to-video. The prompt focuses heavily on the *stiffness* of the elements and the *jitter* of the frame, ensuring it doesn't try to warp or melt the paper cutouts as they move.
* **Models chosen**: `gpt-image-2` is mandatory for the base frames because it can accurately render the requested typography (like numbers on blocks or words on sticky notes) without generating gibberish. `gemini-omni-flash` is best for the video pass because it can handle continuous action from a single start frame while enforcing the stop-motion texture clause.

### 5. Failure modes

1. **The "Melt" Effect:** Video models will instinctively try to morph or squash/stretch the elements as they move. Guard against this by explicitly demanding "rigid paper physics" and "no organic bending."
2. **Loss of the White Border:** Image models often try to seamlessly composite subjects into their environments. The rigid negative prompt ("no realistic perspective") helps, but ensuring the prompt emphasizes "hastily cut out with scissors" forces the AI to treat the border as part of the object itself.
3. **Smooth Framerates:** AI interpolation makes the source motion smooth. Pass `frame_cadence: "on_twos"` to `advibly_render_composition`; the editor and export then share the same posterized-time effect.
4. **Soft Shadows:** Generative models love ray-traced, soft falloff shadows. The style fails if the shadows aren't hard, duplicated shapes offset from the main layer.
