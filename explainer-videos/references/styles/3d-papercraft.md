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

A theatrical, multi-layered die-cut papercraft shadowbox diorama characterized by deep parallax, soft cast shadows, rich jewel-toned ambient lighting, and a central warm glowing focal point, paired with a warm storybook voiceover and rustling paper sound effects.

### 2. Prose breakdown

This style simulates a physical, multi-layered papercraft shadowbox or diorama. The primary visual medium is matte, heavy-weight cardstock, precision-cut with sharp, die-cut edges. The illusion of physical depth is achieved through distinct, stacked planes, each casting soft, diffused drop shadows onto the layer behind it. The texture is strictly matte with a subtle fibrous paper grain, completely devoid of specular highlights or glossy 3D reflections. Lighting logic relies on a central, warm, glowing light source that casts rim light on the inner edges of the concentric paper layers, while the outer edges fall into rich, highly saturated jewel-toned gradients.

Motion is characterized by smooth, deep parallax. As the camera pushes straight in, the foreground layers separate outward while the background layers scale up, creating a tunneling effect. Character or subject animation mimics jointed paper puppets—limbs and features rotate on invisible 2D pivot points rather than bending with organic fluidity. Transitions are driven by the environment itself, utilizing swirling paper-cut vortexes or theatrical elements like folding accordion pleats and sliding curtains to seamlessly bridge scenes.

The audio landscape is anchored by a warm, resonant, and slow-paced female voiceover that evokes a storybook narrator. The music bed is a whimsical, orchestral track featuring harp glissandos, light strings, and gentle chimes, conveying a magical and timeless mood. The sound effects (SFX) are crucial for selling the physical medium: tactile paper rustling, sliding cardboard, soft wind whooshes, and subtle thuds of thick cardstock locking into place, all mixed seamlessly beneath the voice and music.

### 3. Style spec

```json
{
  "style_id": "papercraft_shadowbox_diorama",
  "display_name": "Theatrical Die-Cut Papercraft",
  "one_line_signature": "A theatrical, multi-layered die-cut papercraft shadowbox diorama characterized by deep parallax, soft cast shadows, and rich jewel-toned ambient lighting.",
  "example_subject_only": "Various global myths about the moon, featuring a rabbit and a wolf.",
  "medium": "3D papercraft diorama / layered shadowbox illustration",
  "texture_and_render": "Matte heavyweight cardstock with subtle fibrous paper grain, visible paper thickness on edges, soft ambient occlusion, and distinct cast drop-shadows between overlapping layers.",
  "lighting": "Centralized warm, glowing light source providing inner rim lighting, surrounded by ambient, diffused lighting that darkens into rich jewel tones toward the outer edges of the frame.",
  "palette": {
    "logic": "High-contrast focal point with a warm, bright center fading outward into cool, deep, saturated perimeter layers.",
    "background": "#1A1525",
    "key_hexes": ["#FFD700", "#FFB6C1", "#483D8B", "#008080"],
    "accent_hexes": ["#C71585", "#FF8C00"],
    "saturation": "High saturation, specifically in the jewel tones.",
    "contrast": "High contrast between the central light source and the dark peripheral layers.",
    "color_arc": "Consistent throughout: warm luminous center, cool dark framing."
  },
  "linework_edges": "No drawn linework; edges are defined entirely by sharp, die-cut paper shapes and the resulting cast shadows.",
  "character_design": {
    "present": true,
    "construction": "Flat, 2D paper cutouts constructed from separate overlapping pieces connected at invisible pivot joints.",
    "proportions": "Stylized, simplified silhouettes with rounded features to emulate easy scissor-cuts.",
    "eyes_and_face": "Minimalist, often depicted as simple cut-out holes or small painted dots on the paper surface.",
    "consistency_tells": "Every limb or moving part maintains its flat paper plane, casting a micro-shadow on the body part beneath it."
  },
  "environment_design": "Concentric, theatrical framing (e.g., stage curtains, circular framing layers) building deep parallax toward a central vanishing point.",
  "typography": {
    "present": true,
    "headline_treatment": "Thick, embossed metallic or gold foil serif lettering, often set onto floating paper ribbon banners.",
    "font_character": "Classic, storybook serif with slight embossing.",
    "placement": "Center-aligned, often layered slightly in front of the primary focal point.",
    "text_animation": "Slides in smoothly with the paper ribbons or pops up like a fold-out book element.",
    "caption_style": "Not present in the visual frame; reliant on voiceover."
  },
  "composition_framing": "Symmetrical, center-weighted, theatrical shadowbox framing with heavy foreground occlusion at the borders.",
  "camera_language": "Slow, constant z-axis push-ins (dollying in) through the layers, with occasional slow panning across flat planes.",
  "motion": {
    "cadence": "Smooth 24fps interpolation, but the physics mimic physical paper.",
    "fps_feel": "Smooth 24fps",
    "easing": "Ease-in and ease-out on all sliding elements.",
    "physics": "Rigid 2D planes moving in 3D space; no organic squash and stretch, only rotation at joints.",
    "parallax_depth": "Extreme deep parallax; foreground layers move out of frame rapidly while background elements scale slowly.",
    "signature_moves": ["Concentric tunneling reveal", "Sliding paper puppet joints", "Accordion folding transitions"],
    "energy_level": "Calm, majestic, and steadily paced."
  },
  "transitions_and_cuts": {
    "cut_style": "Seamless environment transitions rather than hard cuts.",
    "avg_seconds_per_shot": "4 to 6 seconds",
    "transition_types": ["Z-axis tunnel-through", "Swirling paper vortex", "Sliding theatrical curtains"]
  },
  "vfx": ["Floating paper confetti stars", "Subtle glowing dust motes around the light source"],
  "pacing_structure": "Measured, storybook pacing. Hook establishes the stage -> slow push-ins reveal subsequent layers of the narrative -> final pull-back or curtain close.",
  "audio": {
    "voiceover": { "present": true, "voice_character": "Warm, maternal, storybook narrator", "tone": "Calm, mystical, authoritative", "pace_words_per_sec": "2.5" },
    "music": { "genre": "Orchestral fantasy", "tempo_feel": "Slow, floating (approx 70 BPM)", "mood": "Magical, wondrous, gentle", "instrumentation": "Harp, light string section, soft glockenspiel/chimes", "role": "Provides a bed of wonder and supports the pacing of the visual reveals." },
    "sfx": { "vocabulary": ["Thick paper sliding", "Cardstock rustling", "Soft wind whooshes", "Gentle cardboard thuds"], "sync_tightness": "Loose, atmospheric sync; tied to large environmental movements rather than micro-actions." },
    "mix": "Voiceover is front and center; music is kept quietly beneath to sit just below the voice; SFX are panned and EQ'd to sound like they are occurring inside a wooden box."
  },
  "format": { "aspect_ratio": "16:9", "example_total_duration_seconds": "15", "resolution_feel": "Crisp 4K to highlight paper texture." },
  "reproduce": {
    "recommended_image_model": "gpt-image-2",
    "image_style_block": "A multi-layered 3D papercraft shadowbox diorama. Matte heavyweight cardstock with subtle fibrous paper grain. Sharp die-cut edges with visible paper thickness. Deep concentric parallax layers casting soft ambient occlusion drop-shadows on the layers behind them. Symmetrical theatrical framing. Centralized warm glowing light source providing inner rim light, fading to deep, highly saturated jewel-toned lighting on the outer perimeter layers. Text elements should appear as thick, embossed gold foil serif lettering on flat paper ribbon cutouts.",
    "image_negative_prompt": "No glossy 3D renders, no specular highlights, no plastic textures, no organic fluid shapes, no drawn linework, no realistic physics, no photographic depth of field blur.",
    "recommended_video_model": "seedance-2.0",
    "motion_prompt_dna": "Smooth z-axis camera push-in through multi-layered paper cutouts. Deep parallax effect: foreground layers slide outward to the edges while background layers slowly scale up. Characters move as rigid, jointed paper puppets rotating on invisible flat hinges. Maintain crisp cardstock textures, sharp die-cut edges, and consistent soft cast drop-shadows on all moving layers throughout the animation.",
    "audio_recipe": {
      "voice_direction": "Female, warm, maternal, paced slowly like reading a classic fairytale to a child.",
      "music_prompt": "Gentle orchestral fantasy, slow tempo, featuring harp glissandos, sweeping light strings, and magical chimes. Instrumental, modern ad underscore.",
      "sfx_prompt": "Thick cardstock sliding, layered paper rustling, soft magical wind whoosh, close-mic."
    }
  },
  "failure_modes": [
    "Model applies smooth 3D CGI gloss or plastic textures instead of matte paper.",
    "Loss of cast drop-shadows, resulting in a flat 2D vector look rather than a 3D shadowbox.",
    "Animation model interpolates characters with organic, fleshy fluid bending rather than rigid paper-joint rotation.",
    "Depth of field blur is applied to background layers, ruining the sharp, macro-photography diorama aesthetic."
  ],
  "best_use_cases": "Brand storytelling, historical or mythological explainers, holiday campaigns, and whimsical product origin stories."
}

```

### 4. Notes on the `reproduce` block

* **`image_style_block`**: Use this exact string as a prefix for all shot generations in `gpt-image-2`. This model is chosen because it successfully follows dense texture specifications and can accurately bake the "gold foil serif lettering on paper ribbons" without losing the overarching papercraft aesthetic.
* **`image_negative_prompt`**: Crucial for preventing the model from defaulting to standard 3D rendering engines like Octane or Unreal. The keywords "no glossy" and "no specular" protect the matte paper feel.
* **`motion_prompt_dna`**: `seedance-2.0` is recommended here because you can supply a start frame and an end frame (a zoomed-in crop of the start frame's center), forcing the model to calculate the deep z-axis parallax tunneling effect perfectly without warping the paper textures.
* **`audio_recipe`**: The SFX are essential. Generating "sliding paper" sounds and laying them under the transitions sells the physical medium illusion even if the video model's motion is slightly too smooth.

### 5. Failure modes

1. **The "Plastic Render" Drift:** Image models will try to add specular highlights (shiny spots) to the 3D shapes. *Guard:* Heavily enforce "matte cardstock" and "no gloss" in the negative prompt.
2. **Loss of Layer Separation (Flatness):** The model might merge the distinct layers into a single flat illustration. *Guard:* Always specify "deep concentric parallax layers" and "soft ambient occlusion drop-shadows" to force the engine to calculate depth.
3. **Organic Motion:** Video models naturally want to make moving subjects bend (e.g., a limb curving like rubber). *Guard:* In the motion prompt, explicitly demand "rigid, jointed paper puppets rotating on invisible flat hinges."
4. **Unwanted Depth of Field:** AI models love to blur the background to create depth. In a shadowbox, everything is typically in focus to emphasize the miniature scale. *Guard:* Add "no photographic depth of field blur" and "keep all layers in sharp focus" to the negative prompt.
