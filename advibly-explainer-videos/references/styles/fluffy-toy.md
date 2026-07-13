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

A cozy, tactile needle-felted stop-motion diorama featuring soft pastel wool roving, anthropomorphized objects with bead-like eyes, and physicalized yarn visual effects.

### 2. Prose breakdown

The visual language is entirely rooted in macro stop-motion craft, specifically needle-felted wool and yarn. Every surface—from the layered, coiled terrain to the floating elements—is constructed from matted wool roving, displaying a high degree of stray, fuzzy fibers and a deeply matte, pillowy finish. The environment operates as a shallow physical diorama, utilizing physical depth of field to separate the foreground action from a flat, textured felt backdrop. There is no digital gloss or specular highlighting; even elemental effects like light rays are represented by twisted, physical strands of yellow wool.

Characters and subjects follow a strict, minimalist design logic to maintain the handmade illusion. Abstract concepts and inanimate objects are brought to life using tiny, wide-set black bead eyes, small curved single-stitch mouths, and perfectly circular pink felted blush marks. The color palette relies on soft, warm pastels (soft sky blues, muted grass greens) punctuated by primary craft colors, all unified by the diffusing nature of the wool texture and soft, warm overhead studio lighting.

Motion and audio heavily reinforce the physical miniature aesthetic. The animation runs on a deliberate "on-twos" cadence (approximately 12fps), exhibiting constant texture "boiling" where the loose wool fibers jitter slightly from frame to frame. Camera work is restrained, utilizing static lock-offs or very slow, mechanical pushes. The soundscape matches the visual softness with a warm, slow-paced female voiceover, a delicate music-box and acoustic guitar underscore, and subtle, soft-impact sound effects that avoid harsh transients.

### 3. Style spec

```json
{
  "style_id": "needle_felt_diorama_01",
  "display_name": "Cozy Felted Wool Stop-Motion",
  "one_line_signature": "A cozy, tactile needle-felted stop-motion diorama featuring soft pastel wool roving, anthropomorphized objects with bead-like eyes, and physicalized yarn visual effects.",
  "example_subject_only": "A cloud and sun creating a rainbow from water drops.",
  "medium": "Needle-felted wool and yarn stop-motion miniature craft.",
  "texture_and_render": "Highly tactile, fuzzy wool roving with visible stray fibers, completely matte finish, pillowy forms with soft edges, distinct frame-to-frame texture boiling.",
  "lighting": "Soft, diffused miniature studio lighting simulating warm sunlight, creating gentle, soft-edged drop shadows to emphasize physical depth.",
  "palette": {
    "logic": "Warm, diffused pastels grounded by the natural off-white of raw wool, accented with bright primary craft colors.",
    "background": "#87b5c8",
    "key_hexes": [
      "#84a067",
      "#f7d147",
      "#d3d3d3"
    ],
    "accent_hexes": [
      "#d9534f",
      "#e6a15c",
      "#5bc0de"
    ],
    "saturation": "Moderate, slightly muted by the physical texture of the wool.",
    "contrast": "Low to medium, preventing harsh shadows and maintaining a soft, inviting tone.",
    "color_arc": "Consistent warm brightness throughout."
  },
  "linework_edges": "No linework; edges are defined by physical soft volumes and stray fuzz.",
  "character_design": {
    "present": true,
    "construction": "Simple, rounded geometric shapes tightly felted from dense wool.",
    "proportions": "Stout and compressed, lacks defined limbs unless absolutely necessary.",
    "eyes_and_face": "Tiny black bead-like eyes set wide apart, tiny single-stitch curved smile, perfect circular pink felted blush on cheeks.",
    "consistency_tells": "Uniform bead eye size and identical cheek blush application across all anthropomorphized elements."
  },
  "environment_design": "Shallow physical diorama constructed from coiled and stacked wool rolls for terrain, set against a flat felt backdrop.",
  "typography": {
    "present": true,
    "headline_treatment": "None present in example.",
    "font_character": "Standard clean sans-serif.",
    "placement": "Bottom center caption band.",
    "text_animation": "None, standard static subtitles.",
    "caption_style": "White text with a subtle, tight black drop-shadow for readability over bright textures."
  },
  "composition_framing": "Center-weighted macro photography framing with shallow depth of field blurring the distant background.",
  "camera_language": "Mostly static lock-offs with occasional, very slow, smooth mechanical pushes or pans.",
  "motion": {
    "cadence": "On-twos stop-motion feel.",
    "fps_feel": "12fps",
    "easing": "Linear or very soft ease-in/out, mimicking physical miniature movement.",
    "physics": "Rigid but bouncy; objects move as solid chunks of felt rather than squashing and stretching dynamically.",
    "parallax_depth": "Moderate physical parallax achieved through distinct foreground, midground, and background diorama layers.",
    "signature_moves": [
      "Texture boiling on stationary objects",
      "Stiff, hinge-like rotation of elements"
    ],
    "energy_level": "Calm, gentle, and deliberate."
  },
  "transitions_and_cuts": {
    "cut_style": "Straight hard cuts.",
    "avg_seconds_per_shot": "4 to 6 seconds",
    "transition_types": [
      "Hard cut"
    ]
  },
  "vfx": [
    "Physicalized light beams made of twisted yellow yarn",
    "Water drops rendered as semi-transparent plastic/glass beads containing yarn"
  ],
  "pacing_structure": "Slow and observational, matching the deliberate cadence of the voiceover.",
  "audio": {
    "voiceover": {
      "present": true,
      "voice_character": "Warm, gentle female voice, maternal and educational.",
      "tone": "Soothing and narrative.",
      "pace_words_per_sec": "2.5"
    },
    "music": {
      "genre": "Acoustic lullaby/folk.",
      "tempo_feel": "70 BPM",
      "mood": "Peaceful, wondrous, gentle.",
      "instrumentation": "Music box, glockenspiel, soft acoustic guitar picking.",
      "role": "Background emotional bed, sitting completely under the voiceover."
    },
    "sfx": {
      "vocabulary": [
        "Soft fabric rustles",
        "Gentle wind chimes",
        "Muffled pops"
      ],
      "sync_tightness": "Loose, atmospheric syncing rather than hard, literal impacts."
    },
    "mix": "Voiceover forward, music bed dipped significantly (-15db), SFX subtly mixed into the music bed to avoid harsh transients."
  },
  "format": {
    "aspect_ratio": "16:9",
    "example_total_duration_seconds": "18",
    "resolution_feel": "Sharp 4k macro photography."
  },
  "reproduce": {
    "recommended_image_model": "nano-banana-2",
    "image_style_block": "Macro photography of a needle-felted wool stop-motion diorama. Every surface, object, and landscape is made of fuzzy, tactile matted wool roving with visible stray fibers. Soft, diffused studio lighting casting gentle shadows. Pastel color palette. Characters are made of felted wool with tiny black bead eyes and stitched mouths. Physical miniature aesthetic, matte finish, shallow depth of field.",
    "image_negative_prompt": "3d render, CGI, glossy, shiny, specular highlights, digital glow, smooth plastic, sharp vector lines, realistic skin, flat 2d illustration.",
    "recommended_video_model": "gemini-omni-flash",
    "motion_prompt_dna": "Stop-motion animation on-twos, 12fps feel. Constant subtle texture boiling of the loose wool fibers on all objects. Slow, steady, smooth camera push forward. Objects move with stiff, rigid physical charm. Maintain all wool and yarn textures exactly.",
    "audio_recipe": {
      "voice_direction": "Female, soft, gentle, slow-paced, educational storytelling, warm tone.",
      "music_prompt": "Gentle acoustic lullaby, music box and acoustic guitar, 70 BPM, peaceful, wondrous, instrumental, modern ad underscore.",
      "sfx_prompt": "Soft fabric rustling, gentle wind chimes, muffled physical pops."
    }
  },
  "failure_modes": [
    "Model drifts into smooth 3D CGI instead of rough, fuzzy wool.",
    "Model adds digital glowing VFX instead of physical yarn representations.",
    "Video model smooths the frame rate to 24fps/60fps, losing the stop-motion charm.",
    "Faces become too detailed and human-like instead of simple bead eyes."
  ],
  "best_use_cases": "Explaining gentle, complex, or emotional topics; children's educational content; brand storytelling focusing on warmth and approachability."
}

```
