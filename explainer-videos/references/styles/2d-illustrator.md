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

A lineless, textured digital gouache animation running at a choppy 12fps, featuring high-contrast geometric shading against a stark, monochromatic background void, driven by a relaxed lo-fi hip-hop beat.

### 2. Prose breakdown

The visual medium mimics mid-century traditional gouache or acrylic cel animation, executed digitally. Subjects are constructed entirely from flat, lineless shapes with hard-edged, geometric shadow planes. The defining texture is a persistent, toothy paper grain overlay combined with dry-brush stippling on the transitions between light and shadow, anchoring the digital art in a tactile, hand-painted reality. Backgrounds abandon spatial reality, acting as flat, deeply saturated monochromatic voids (a striking royal blue) that frame the action.

Motion is intentionally degraded to simulate animation "on twos" (around 12 frames per second), giving it a rhythmic, jittery stop-motion cadence rather than smooth digital interpolation. This choppy frame rate is paired with sudden, explosive background speed lines and stylized motion streaks that pop in for single frames. The audio contrasts this energetic, staccato visual style with a laid-back, vinyl-textured lo-fi hip-hop instrumental and a calm, authoritative voiceover, creating a grounded, effortlessly cool atmosphere.

### 3. Style spec

```json
{
  "style_id": "lineless_gouache_lofi_chop",
  "display_name": "Textured Gouache on Twos",
  "one_line_signature": "Lineless, textured digital gouache animation at 12fps with geometric shading against a stark monochromatic void.",
  "example_subject_only": "A young man demonstrating how to ride a skateboard.",
  "medium": "Lineless digital painting mimicking traditional gouache/acrylic cel animation.",
  "texture_and_render": "Heavy toothy paper grain, dry-brush stippled shading, hard-edged shadow planes, matte finish, zero specular highlights.",
  "lighting": "Strong directional key light creating sharp, unsoftened geometric shadow shapes across subjects.",
  "palette": {
    "logic": "High-contrast split complementary. Neutral/earthy subjects against a deeply saturated primary color background, punctuated by small neon accents.",
    "background": "#121A9E",
    "key_hexes": [
      "#68351B",
      "#EAE6DF",
      "#111111"
    ],
    "accent_hexes": [
      "#F12B7D",
      "#FFC629",
      "#008B8B"
    ],
    "saturation": "High saturation for background and accents; muted, earthy tones for the primary subject base.",
    "contrast": "Extremely high. Deep blacks and stark whites used for shading and highlighting.",
    "color_arc": "Static. The deep blue background and lighting logic remain constant."
  },
  "linework_edges": "Completely lineless. Forms are defined strictly by contrasting color planes and shadow edges.",
  "character_design": {
    "present": true,
    "construction": "Semi-realistic anatomical proportions built from flat painted shapes.",
    "proportions": "True-to-life, unexaggerated human proportions.",
    "eyes_and_face": "Simplified painted planes. No outlines for facial features; deep geometric shadows define the nose and brow.",
    "consistency_tells": "Harsh, jagged shadow shapes under the chin and on clothing folds remain locked to the light source."
  },
  "environment_design": "Infinite abstract void. No floor, no horizon line. Depth is implied only by the subject's cast shadow dropping onto the invisible floor plane.",
  "typography": {
    "present": true,
    "headline_treatment": "None present.",
    "font_character": "Clean, neutral, sans-serif (similar to Helvetica or Arial).",
    "placement": "Bottom-center lower third.",
    "text_animation": "Static pop-on.",
    "caption_style": "Standard white subtitle text with a subtle black drop shadow for readability against the blue."
  },
  "composition_framing": "Center-framed subjects. A mix of wide full-body shots and medium punch-ins.",
  "camera_language": "Static camera. The world and subject move through a locked frame to simulate speed.",
  "motion": {
    "cadence": "Staccato, jittery, animated on twos or threes.",
    "fps_feel": "12fps.",
    "easing": "Linear and sudden. No smooth modern easing or motion blur.",
    "physics": "Rigid. Minimal squash and stretch, focusing on pose-to-pose keyframes.",
    "parallax_depth": "None. Pure 2D flat staging.",
    "signature_moves": [
      "Rapidly shifting, jagged background shadow streaks in a darker background hue (#0A105C)",
      "Single-frame white motion-streak impact lines around fast-moving limbs"
    ],
    "energy_level": "Kinetic and snappy, contrasting the relaxed audio."
  },
  "transitions_and_cuts": {
    "cut_style": "Hard cuts. No dissolves or wipes.",
    "avg_seconds_per_shot": "4 to 6 seconds; pack rapid instructional beats inside each clip",
    "transition_types": [
      "Hard cut on action",
      "Hard cut on beat"
    ]
  },
  "vfx": [
    "Hand-drawn white action lines",
    "Flickering geometric background speed shadows"
  ],
  "pacing_structure": "Rapid-fire internal action beats inside 4 to 6-second generated shots. Hook lands by 0.5s, setup by 1.5s, then sequential action steps every 1 to 2s.",
  "audio": {
    "voiceover": {
      "present": true,
      "voice_character": "Deep, smooth, relaxed male.",
      "tone": "Casual, instructional, effortless.",
      "pace_words_per_sec": "2.5"
    },
    "music": {
      "genre": "Lo-fi hip-hop.",
      "tempo_feel": "85 BPM, laid-back boom-bap rhythm.",
      "mood": "Chill, focused, urban.",
      "instrumentation": "Sampled jazz electric piano, vinyl crackle, heavy kick, snappy rimshot.",
      "role": "Sets a relaxed emotional baseline to contrast the fast visuals."
    },
    "sfx": {
      "vocabulary": [
        "Hard urethane rolling on concrete",
        "Rhythmic clacking",
        "Subtle fabric rustles"
      ],
      "sync_tightness": "Frame-accurate to the subject's physical impacts."
    },
    "mix": "VO centered and dominant. Music heavy on the low-end, aggressively kept quietly beneath under the VO. SFX panned and mixed tight to the action."
  },
  "format": {
    "aspect_ratio": "16:9",
    "example_total_duration_seconds": "15",
    "resolution_feel": "Crisp 1080p but heavily textured to feel analog."
  },
  "reproduce": {
    "recommended_image_model": "nano-banana-2",
    "image_style_block": "Lineless gouache digital painting, retro 2D animation style. Heavy toothy paper grain texture throughout. Dry-brush stippled shading, sharp angular geometric shadow planes. Set against a stark, flat, deep royal blue (#121A9E) background void. High contrast lighting, matte finish, zero specular highlights. Subject has warm earthy tones, off-white and black details, with small vibrant magenta and yellow accents.",
    "image_negative_prompt": "Outlines, lineart, ink, smooth 3D render, glossy, CGI, soft gradients, photographic, photorealism, depth of field, cluttered background, realistic environment, horizon line.",
    "recommended_video_model": "gemini-omni-flash",
    "motion_prompt_dna": "Animate with a rigid, jittery 12fps stop-motion cadence on twos. Hold the grainy paper texture static over the entire frame. Generate sudden, angular, hand-painted white speed lines and dark geometric shadow streaks snapping rapidly across the flat background.",
    "audio_recipe": {
      "voice_direction": "Male voice, deep register, calm, effortless, casual instructional tone, speaking slowly at 2.5 words per second.",
      "music_prompt": "Lo-fi hip-hop instrumental, modern ad underscore, 85 BPM, boom-bap drums, vinyl crackle, mellow electric piano chords, relaxed and chill.",
      "sfx_prompt": "Rhythmic hard plastic rolling on rough concrete, sharp physical clacks, subtle heavy fabric movement."
    }
  },
  "failure_modes": [
    "Model attempts to add a realistic floor or horizon line; guard with 'flat infinite void' and negative prompt 'horizon line, realistic environment'.",
    "Video model introduces smooth 60fps interpolation or motion blur; guard by enforcing '12fps, stop-motion cadence, on twos' in motion prompt.",
    "Image model renders soft gradients for shading; guard with 'sharp angular geometric shadow planes' and negative prompt 'soft gradients, 3D smooth render'.",
    "Loss of tactile surface feel; guard by heavily weighting 'heavy toothy paper grain' and using nano-banana-2."
  ],
  "best_use_cases": "Fast-paced instructional content, product tutorials, or edgy lifestyle brand manifestos where you need to convey high energy while maintaining a calm, authoritative brand voice."
}

```

### 4. Notes on the `reproduce` block

* **`recommended_image_model`**: `nano-banana-2` is strictly required here. The entire style relies on holding organic dry-brush and paper grain textures. Models like `gpt-image-2` will likely smooth this out into vector art.
* **`recommended_video_model`**: `gemini-omni-flash` is best because it excels at maintaining intense, positive-prompted stylistic overlays (like the animated speed lines and 12fps jitter) without needing an exact end frame, allowing the action to flow naturally off the initial prompt.
* **`motion_prompt_dna`**: Relies entirely on positive phrasing to force the choppiness. Video models inherently want to smooth things out; instructing it to use "stop-motion cadence on twos" overrides the default interpolation.

### 5. Failure modes

* **The "Gradient Smoothing" Trap:** Generative image models often default to soft, 3D-like shading. If the hard, geometric shadow planes are lost, the vintage gouache feel is destroyed.
* **The "Spatial Reality" Trap:** The models will aggressively try to anchor the subject to a realistic floor or room. The background *must* remain an abstract color void.
* **The "Butter Smooth" Trap:** Video models will try to generate fluid 24fps/60fps motion with motion blur. This ruins the stylized "on twos" animation feel. Strict prompting for jittery, low-fps motion is required.
