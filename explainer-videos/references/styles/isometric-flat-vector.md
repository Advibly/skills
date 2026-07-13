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

A monochromatic, high-contrast, vintage block-print 2D illustration style featuring heavy distress textures, limited motion-comic animation, and stark white focal glows.

### 2. Prose breakdown

**Medium, Texture, and Color:** The visual foundation is built on 2D digital illustration designed to mimic traditional silkscreen or block printing. Every layer is heavily treated with a uniform, distressed grunge texture—visible scratches, dry-brush edges, and paper-grain noise—giving the entire frame a rough, matte, tactile quality. The color logic is strictly monochromatic, restricted to deep navy blues, cyan midtones, solid blacks for deep shadows, and pure white. Lighting is highly localized and dramatic, originating from a single glowing white focal object that casts hard, black shadows across the stylized environment.

**Character, Environment, and Composition:** Subject construction relies on angular, 2D vector-style shapes with thick, irregular outlines that reinforce the traditional print aesthetic. Characters feature simplified, expressive facial geometry—large, unshaded eyes with solid pupils—and flat shading broken only by the overarching texture overlay. The environment is constructed using flat, overlapping 2D planes to create depth without perspective, often featuring jagged, abstract architectural shapes. Compositions are highly graphic, utilizing a stark contrast between the dark, textured backgrounds and the blindingly bright, pure white focal points.

**Motion, Audio, and Pacing:** The motion language is intentionally constrained, mirroring a "motion comic" or animatic feel. Animation relies on linear sliding of flat layers, digital scale push-ins, and hard cuts rather than fluid, frame-by-frame in-betweens. Occasional glowing bursts or stark white flashes provide visual punctuation. The audio landscape is anchored by a dramatic, close-mic voiceover, supported by a tense, pulsing synthetic underscore. Sound effects are sparse but heavy, utilizing deep cinematic sub-booms for structural movements and high-pitched, ethereal ringing for glowing elements, perfectly synced to the rigid animation cadence.

### 3. Style spec

```json
{
  "style_id": "distressed_monochrome_print_comic",
  "display_name": "Monochrome Distress Print",
  "one_line_signature": "A monochromatic, high-contrast, vintage block-print 2D illustration style featuring heavy distress textures, limited motion-comic animation, and stark white focal glows.",
  "example_subject_only": "A boy playing chess and making a decisive move.",
  "medium": "2D digital illustration mimicking traditional silkscreen or block print.",
  "texture_and_render": "Heavy, uniform distressed grunge, scratch marks, dry-brush edges, and paper grain noise. Entirely matte.",
  "lighting": "Dramatic, localized stark white glow acting as the sole light source, casting hard black shadows.",
  "palette": {
    "logic": "Strictly monochromatic blue with high-contrast black and pure white.",
    "background": "Textured deep navy and cyan.",
    "key_hexes": ["#052340", "#16588C", "#000000", "#FFFFFF"],
    "accent_hexes": ["#36A9D9"],
    "saturation": "High saturation in the cyan midtones, surrounded by desaturated blacks/navys.",
    "contrast": "Extreme contrast.",
    "color_arc": "Static throughout."
  },
  "linework_edges": "Thick, irregular, dark blue/black outlines with dry-brush roughness.",
  "character_design": {
    "present": true,
    "construction": "Angular 2D vector shapes, flat shading with texture overlay.",
    "proportions": "Slightly stylized, oversized head and facial features.",
    "eyes_and_face": "Large simple eyes, pure white sclera, dark round pupils, flat graphical expressions.",
    "consistency_tells": "Thick stylized glasses frames, messy hair silhouette."
  },
  "environment_design": "Flat overlapping 2D planes, jagged abstract background silhouettes, no vanishing point perspective.",
  "typography": {
    "present": true,
    "headline_treatment": "None present.",
    "font_character": "Simple, clean sans-serif for captions.",
    "placement": "Bottom center.",
    "text_animation": "Static pop-on.",
    "caption_style": "Pure white, plain text."
  },
  "composition_framing": "Graphic, low-angle mediums cutting to tight close-ups.",
  "camera_language": "Static shots with slow digital push-ins, cutting directly on action.",
  "motion": {
    "cadence": "Limited motion-comic style.",
    "fps_feel": "12fps or lower for character elements, smooth digital camera moves.",
    "easing": "Linear slides and scales.",
    "physics": "Rigid, no squash or stretch.",
    "parallax_depth": "Minimal, flat layered sliding.",
    "signature_moves": ["Digital camera push-in", "Pulsing white glow scale", "Hard cut to extreme close-up"],
    "energy_level": "Tense and deliberate."
  },
  "transitions_and_cuts": {
    "cut_style": "Hard cuts on dramatic beats.",
    "avg_seconds_per_shot": "4 to 6 seconds",
    "transition_types": ["Hard cut", "White impact flash"]
  },
  "vfx": ["Stark white radial glow", "Screen-filling impact flash"],
  "pacing_structure": "Tense setup -> dramatic action -> confident payoff.",
  "audio": {
    "voiceover": { "present": true, "voice_character": "Young male, slightly raspy, confident", "tone": "Dramatic, serious", "pace_words_per_sec": "2.5" },
    "music": { "genre": "Cinematic synth underscore", "tempo_feel": "Slow, pulsing", "mood": "Tense, building", "instrumentation": "Low synths, subtle strings", "role": "Tension building" },
    "sfx": { "vocabulary": ["Deep sub-bass boom", "Ethereal high-pitched ringing", "Subtle whoosh"], "sync_tightness": "Perfectly synced to visual impacts and cuts." },
    "mix": "VO sits high, SFX punches through music heavily on visual impacts."
  },
  "format": { "aspect_ratio": "16:9", "example_total_duration_seconds": "8", "resolution_feel": "Crisp 1080p with intentional grit" },
  "reproduce": {
    "recommended_image_model": "gpt-image-2",
    "image_style_block": "2D digital illustration mimicking vintage silkscreen or block print. Strictly monochromatic blue color palette (navy, cyan) with pure black shadows and pure white highlights. Heavy distressed grunge texture, visible scratch marks, and dry-brush edges uniformly covering the image. Flat, angular 2D shapes with thick, irregular outlines. Dramatic, stark white localized glowing light source casting hard shadows.",
    "image_negative_prompt": "3d render, glossy, gradients, realistic lighting, full color, depth of field, photography, smooth gradients, clean digital vectors, soft shadows.",
    "recommended_video_model": "gemini-omni-flash",
    "motion_prompt_dna": "Slow digital camera push-in. Keep elements rigidly flat like overlapping 2D planes. Maintain heavy static distressed grunge texture over the entire frame. Limit animation to linear sliding and pulsing white glowing light effects.",
    "audio_recipe": { "voice_direction": "Young male, dramatic and confident, pacing with intentional pauses.", "music_prompt": "Instrumental, modern ad underscore, tense pulsing cinematic synth, slow tempo, ominous.", "sfx_prompt": "Deep cinematic sub-bass impact boom, ethereal high-pitched magical ringing." }
  },
  "failure_modes": ["Model introduces colors outside the strictly blue/black/white palette.", "Model creates smooth 3D shading instead of flat 2D block-print shapes.", "Video model smooths out the motion too much, losing the limited motion-comic feel.", "Video model loses the static scratchy texture overlay during movement."],
  "best_use_cases": "Dramatic reveals, strategic planning analogies, tense storytelling, cybersecurity concepts."
}

```

### 4. Notes on the `reproduce` block

* **`image_style_block`:** This prompt explicitly locks the color constraints ("Strictly monochromatic blue") and the physical craft ("vintage silkscreen", "heavy distressed grunge texture"). This is critical because base image models will default to full color and clean vector shading for 2D prompts.
* **`image_negative_prompt`:** Essential for stripping out the default AI gloss. "3d render", "smooth gradients", and "full color" must be actively suppressed to maintain the flat, printed look.
* **`motion_prompt_dna`:** Designed for `gemini-omni-flash`, emphasizing positive actions like "Slow digital camera push-in" and "Keep elements rigidly flat". Mentioning the "heavy static distressed grunge texture" helps the model attempt to bake the noise in rather than smoothing it over time.
* **`recommended_image_model`:** `gpt-image-2` is recommended because it adheres well to strict color palette constraints and dense stylistic phrasing.
* **`recommended_video_model`:** `gemini-omni-flash` is best here as it excels at subtle camera push-ins and can handle limited motion from a single start frame without needing an exact end frame, fitting the motion-comic aesthetic.

### 5. Failure modes

* **Color Bleed:** The image model will naturally want to introduce skin tones or warm highlights. **Guard:** The negative prompt ("full color") and strictly defining the palette in the `image_style_block` are mandatory.
* **Accidental 3D:** Text prompts mentioning "glow" or "lighting" often trigger 3D rendering engines in the latent space. **Guard:** Emphasize "2D flat overlapping planes" and "block print" in every prompt, penalizing "3D render" and "realistic lighting".
* **Texture Smoothing in Video:** Video models (especially AI interpolation) tend to denoise frames, which will destroy the scratchy print aesthetic. **Guard:** Explicitly command the video model to "maintain heavy static distressed grunge texture" in the `motion_prompt_dna`.
* **Over-Animation:** Generating too much fluid movement will break the stiff, intentional animatic style. **Guard:** Keep motion prompts constrained to "slow push-in" or "linear sliding".
