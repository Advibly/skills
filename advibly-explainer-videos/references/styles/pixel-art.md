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

A vibrant, 16-bit retro video game pixel-art aesthetic featuring chunky chibi-proportioned characters, deep parallax side-scrolling environments, snappy low-framerate sprite animation, and a chiptune-inspired audio landscape.

### 2. Prose breakdown

This style perfectly replicates the golden era of 16-bit video games (akin to SNES or Genesis), utilizing a strict pixel-art medium. The fundamental visual rule is absolute sharpness: there is zero anti-aliasing, with every element constructed from hard-edged, distinct colored squares. Shading is achieved through stepped block colors (typically 2-3 shades per hue) and classic pixel dithering (checkerboard patterns) to create gradients, particularly in the skies or atmospheric transitions. The color palette is intensely saturated, relying on stark contrasts between bright, idyllic tones (vivid greens, bright blues) and dark, oppressive shades (deep purples, charcoal grays) to represent opposing forces.

Characters and subjects are built with exaggerated, chibi-style proportions—typically a 1:2 or 1:3 head-to-body ratio—ensuring readability even at low pixel resolutions. They feature oversized, expressive eyes and stubby, simplified limbs, all encased in a crisp, 1-pixel dark outline to pop against the detailed backgrounds. Environments are constructed as flat, orthographic, or 2D side-scrolling planes layered in deep parallax (foreground grass, midground structures, background mountains) to create a false sense of depth without using true 3D perspective. The camera remains mostly static, relying on hard, fast cuts between wide establishing shots and medium action shots.

Motion mimics traditional sprite-based animation, running at a simulated low framerate (animating "on-twos" or "on-fours" within a 24fps timeline). Movements are snappy, distinct, and lack smooth digital interpolation, relying on squash-and-stretch principles and explosive, blocky particle VFX for impacts and transitions. The audio perfectly complements the visuals, anchoring an energetic, fast-paced voiceover against a driving 140 BPM chiptune music track. Sound effects are inherently retro, utilizing synthesized white-noise crashes, square-wave bloops, and 8-bit arpeggios tightly synced to every on-screen action, hit, and text reveal.

### 3. Style spec

```json
{
  "style_id": "retro_16bit_arcade_pixel",
  "display_name": "16-Bit Arcade Pixel Art",
  "one_line_signature": "Vibrant 16-bit pixel-art with chibi characters, stepped shading, snappy sprite motion, and chiptune audio.",
  "example_subject_only": "A microscopic battle between a knight (white blood cell) defending a castle from virus monsters.",
  "medium": "16-bit digital pixel art.",
  "texture_and_render": "Sharp, rigid pixel squares with zero anti-aliasing. Flat block shading using 2-3 tones per color, with prominent checkerboard dithering for gradients and atmospheric transitions.",
  "lighting": "Flat, uniform 2D lighting, typically top-down or top-left, defined purely by stepped pixel color values rather than rendered light passes.",
  "palette": {
    "logic": "High-contrast, dual-toned fantasy palette contrasting hyper-saturated idyllic colors against deep, dark antagonistic colors.",
    "background": "Deep layered tones ranging from bright sky blue to dark moody purple.",
    "key_hexes": ["#1A52C9", "#4B9A2F", "#F7D841", "#D82126"],
    "accent_hexes": ["#38244B", "#222222", "#E0E0E0"],
    "saturation": "Extremely high, vibrant and punchy.",
    "contrast": "High contrast, utilizing 1-pixel dark outlines to separate elements from the background.",
    "color_arc": "Shifts dynamically based on scene tone, moving from split-screen contrast to unified bright victory palettes."
  },
  "linework_edges": "Strict 1-pixel dark brown or black outlines around all interactive or foreground subjects; background elements are lineless.",
  "character_design": {
    "present": true,
    "construction": "Chibi proportions, built from distinct pixel blocks.",
    "proportions": "1:2 or 1:3 head-to-body ratio. Oversized heads, stubby limbs, compact bodies.",
    "eyes_and_face": "Large, 2-to-4 pixel eyes, highly expressive with minimal detail.",
    "consistency_tells": "Rigid adherence to the 1-pixel outline and stepped 3-tone shading system across all character models."
  },
  "environment_design": "Layered 2D side-scrolling perspective. Distinct planes for foreground, midground action, and deep background layers (mountains/sky) to simulate parallax depth.",
  "typography": {
    "present": true,
    "headline_treatment": "Chunky, all-caps, 16-bit arcade pixel font in gold/yellow, resting on a decorative pixelated banner.",
    "font_character": "Blocky, retro, serif or heavy sans-serif pixel font.",
    "placement": "Top center, dominating the upper third of the screen.",
    "text_animation": "Hard pop-in or simple pixelated scale-up; no smooth easing.",
    "caption_style": "None present in the core visual, but would be a crisp, white pixel font with a black drop-shadow if needed."
  },
  "composition_framing": "Action-oriented central framing. Wide establishing shots heavily split down the middle, followed by medium-wide combat shots.",
  "camera_language": "Static camera placements. No pan, tilt, or zoom during a shot. All motion happens within the frame.",
  "motion": {
    "cadence": "Stepped, low-framerate sprite animation (on-twos or on-fours).",
    "fps_feel": "Simulated 8 to 12 frames per second feel within a modern wrapper.",
    "easing": "Linear and snappy. No digital bezier easing. Instant acceleration and deceleration.",
    "physics": "Exaggerated squash and stretch on impacts; gravity is heavy and fast.",
    "parallax_depth": "High. Background layers remain static or move at a distinct, slower rate than the foreground.",
    "signature_moves": ["Multi-frame weapon swings", "Squash-and-stretch jump impacts", "Instant pop-in text"],
    "energy_level": "Frenetic, highly active, and aggressive."
  },
  "transitions_and_cuts": {
    "cut_style": "Hard, instantaneous cuts.",
    "avg_seconds_per_shot": "4 to 6 seconds; pack rapid action beats and snap transitions inside each clip.",
    "transition_types": ["Hard cut on action", "Screen wipe via pixelated effect (optional)"]
  },
  "vfx": ["Blocky pixel particle bursts on impact", "Thick 1-pixel action lines", "Screen shake", "Confetti/debris falling in rigid pixel blocks"],
  "pacing_structure": "Rapid escalation through internal beats: the hook lands within the first 2s of its 4 to 6-second shot, followed by fast action, explosive resolution, and a victory banner across later valid-length shots.",
  "audio": {
    "voiceover": {
      "present": true,
      "voice_character": "Energetic, clear, slightly booming male announcer.",
      "tone": "Urgent, informative, triumphant.",
      "pace_words_per_sec": "3.5"
    },
    "music": {
      "genre": "Chiptune / 8-Bit Retro Video Game.",
      "tempo_feel": "Fast, driving (approx 140 BPM).",
      "mood": "Epic, heroic, action-oriented.",
      "instrumentation": "Square waves, sawtooth synths, synthesized basic percussion.",
      "role": "Drives the rhythm and energy of the cuts."
    },
    "sfx": {
      "vocabulary": ["Synthesized white-noise crashes", "High-pitched square wave bloops", "Crushed bit-rate explosions", "8-bit fanfare arpeggios"],
      "sync_tightness": "Extremely tight. Every impact, step, and text reveal has a corresponding retro SFX."
    },
    "mix": "SFX heavily prominent and punchy, punching through a compressed music bed. VO sits clearly on top."
  },
  "format": {
    "aspect_ratio": "16:9",
    "example_total_duration_seconds": "11",
    "resolution_feel": "Upscaled low-resolution (looks like 320x180 upscaled cleanly to 1080p)."
  },
  "reproduce": {
    "recommended_image_model": "gpt-image-2",
    "image_style_block": "16-bit retro video game pixel art, strict crisp pixel edges, zero anti-aliasing. High contrast saturated palette. 2D side-scrolling perspective with deep parallax layers. Characters have chibi proportions (1:3 head-to-body ratio), oversized eyes, and a strict 1-pixel dark outline. Shading is stepped block colors with checkerboard dithering for gradients. No smooth shading or 3D elements.",
    "image_negative_prompt": "No 3D renders, no smooth gradients, no anti-aliasing, no realistic lighting, no high-poly, no vector art, no blurry edges, no depth of field.",
    "recommended_video_model": "gemini-omni-flash",
    "motion_prompt_dna": "Animate as a 16-bit video game. Snappy, low-framerate sprite animation on twos. Add blocky pixel particle explosions and screen shake on impact. Maintain rigid, sharp pixel edges without blurring. Characters move with fast squash and stretch. Keep camera completely static.",
    "audio_recipe": {
      "voice_direction": "Energetic retro video game announcer, urgent and triumphant.",
      "music_prompt": "Fast 140 BPM chiptune, epic 8-bit retro video game battle music, driving square wave melody, instrumental.",
      "sfx_prompt": "8-bit retro video game sound effects, white noise sword crashes, bit-crushed explosions, synthesized jumping bloops, 8-bit victory fanfare."
    }
  },
  "failure_modes": [
    "Model introduces anti-aliasing or blurring, ruining the crisp pixel aesthetic. Guard: Heavily enforce 'zero anti-aliasing' and 'strict pixel edges' in prompts.",
    "Model attempts 3D lighting or smooth gradients instead of dithering. Guard: Specify 'stepped block shading' and 'checkerboard dithering'.",
    "Video model interpolates frames too smoothly, losing the retro feel. Guard: Specify 'low-framerate sprite animation on twos'."
  ],
  "best_use_cases": "Gamified explainers, tech or cybersecurity metaphors, nostalgic brand activations, and high-energy educational shorts."
}

```
