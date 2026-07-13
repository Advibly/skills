# Style catalog

The ten explainer styles, how to recommend one from the brand, and the quick reference table.
Read this at the style-pick gate (Phase 2). Once a style is picked, read its full file in
`styles/<file>.md`; this catalog is only the menu and the routing logic.

Each style is a cartridge. The filename matches the style's label; the display name
describes what it actually looks like (some of these labels are loose: "Isometric Flat Vector" is
really a monochrome distressed print look, "Fluffy Toy" is needle-felted wool, "3D Mix" is cozy
kawaii). Recommend and describe by the display name and signature, not the filename.

## The menu (show all ten at the pick gate)

| # | Style (say this) | File | One-line signature |
|---|---|---|---|
| 1 | **Cinematic 2D Print** | `isometric-flat-vector` | Monochromatic high-contrast block-print illustration, heavy distress texture, stark white focal glows, tense motion-comic cuts. |
| 2 | **Textured Gouache** | `2d-illustrator` | Lineless digital gouache on twos, toothy paper grain, hard geometric shadows on a flat saturated color void, lo-fi cool. |
| 3 | **Whiteboard Doodle** | `whiteboard-doodle` | Dry-erase marker doodles on a smudged whiteboard, stroke-by-stroke draw-ons, line boiling, squeaky-marker foley. |
| 4 | **Pixel Art** | `pixel-art` | Vibrant 16-bit arcade pixel art, chibi sprites, deep parallax, snappy on-twos sprite motion, chiptune. |
| 5 | **Claymation** | `claymation` | Vibrant plasticine stop-motion, visible fingerprints, chunky characters, sculpted clay text, 12fps boil. |
| 6 | **Felted Wool** | `fluffy-toy` | Cozy needle-felted wool diorama, fuzzy stray fibers, bead eyes, physicalized yarn VFX, gentle stop-motion. |
| 7 | **Cozy Kawaii** | `3d-mix` | Soft pastel bloom illustration, limbless kawaii blobs, drifting sparkles, gentle squash-and-stretch, lullaby audio. |
| 8 | **Low-Poly 3D** | `low-poly` | Warm low-poly 3D world, un-smoothed flat-shaded facets, smooth camera glides, childlike acoustic storybook. |
| 9 | **Papercraft Shadowbox** | `3d-papercraft` | Theatrical die-cut papercraft diorama, deep parallax tunnels, jewel-tone rim light, warm storybook narration. |
| 10 | **Mixed-Media Collage** | `mixed-media` | 2.5D paper-cut collage, greyscale photo cut-outs with white scissor borders, hard drop shadows, sticky-note infographics. |

## Recommend from the brand (pick one, then show the menu)

Match the style to the brand's category, topic, and audience, then name one with a reason. Group
heuristics:

- **Warm, gentle, wellness, kids, health, food, "explain something softly"** → Felted Wool,
  Cozy Kawaii, or Low-Poly. Felt and kawaii for coziest emotional topics; low-poly for a
  slightly more premium storybook.
- **Playful, whimsical, story-driven brand origin, history, "make it charming"** → Claymation,
  Papercraft Shadowbox, or Low-Poly. Claymation for tactile fun; papercraft for a magical
  fairytale reveal; low-poly for a clean modern storybook.
- **Tech, gaming, security, dev tools, "make it cool / retro / edgy"** → Pixel Art (gamified,
  nostalgic), Cinematic 2D Print (dramatic, tense reveals), or Textured Gouache (lo-fi, high
  energy with a calm voice).
- **Education, data, mechanics, "how it works", comparisons, science** → Mixed-Media Collage
  (physicalized infographics, fast punchy), Whiteboard Doodle (classic explainer, approachable),
  or Cozy Kawaii (gentle educational).
- **Lifestyle, sport, tutorials, "energetic but grounded"** → Textured Gouache (kinetic visuals,
  chill voice) or Pixel Art.

Say something like: "For [brand], I'd go **Whiteboard Doodle**: your topic is a how-it-works
explainer and the draw-on reveal makes each step land. Here are all ten if you'd rather pick by
eye: ..." Then list the menu. The user's choice wins.

## Quick reference table (routing per style)

Pull the authoritative values from the style file; this table is the at-a-glance map.

| Style | Image model | Video model | Cadence | On-twos snap | In-world headline text | Suggested voice |
|---|---|---|---|---|---|---|
| Cinematic 2D Print (`isometric-flat-vector`) | gpt-image-2 | gemini-omni-flash | Limited motion-comic | No (rely on hard cuts) | No (captions only) | leo |
| Textured Gouache (`2d-illustrator`) | nano-banana-2 | gemini-omni-flash | On-twos ~12fps | **Yes** | No (captions only) | sal |
| Whiteboard Doodle (`whiteboard-doodle`) | gpt-image-2 | gemini-omni-flash | On-twos 12fps | **Yes** | **Yes** (marker handwriting) | eve |
| Pixel Art (`pixel-art`) | gpt-image-2 | gemini-omni-flash | On-twos/fours sprite | **Yes** | **Yes** (pixel banner) | leo |
| Claymation (`claymation`) | nano-banana-2 | gemini-omni-flash | On-twos 12fps boil | **Yes** | **Yes** (clay letters) | ara |
| Felted Wool (`fluffy-toy`) | nano-banana-2 | gemini-omni-flash | On-twos 12fps boil | **Yes** | No (captions only) | ara |
| Cozy Kawaii (`3d-mix`) | gpt-image-2 | gemini-omni-flash | Smooth 24fps | No | No (captions only) | ara |
| Low-Poly 3D (`low-poly`) | gpt-image-2 | gemini-omni-flash | Smooth 24fps | No | No (captions only) | ara |
| Papercraft Shadowbox (`3d-papercraft`) | gpt-image-2 | **seedance-2.0** | Smooth 24fps | No | **Yes** (gold-foil ribbon) | ara |
| Mixed-Media Collage (`mixed-media`) | gpt-image-2 | gemini-omni-flash | On-twos 12fps | **Yes** | **Yes** (sticky notes / tape / blocks) | eve |

Notes on the routing:

- **Snap = Yes** means pass `frame_cadence: "on_twos"` to `advibly_render_composition` (Phase 6).
  The final-render temporal effect supplies the cadence and appears in the editor under Effects.
  **Snap = No**
  styles are meant to be smooth (low-poly, kawaii, papercraft) or get their choppiness from hard
  cuts and limited element motion (Cinematic 2D Print), so a global snap would wrongly choppify
  their smooth camera moves.
- **Seedance for papercraft**: its signature is a deep z-axis tunnel through paper layers, which
  needs an exact end frame (a zoomed crop of the start). That is a Seedance start+end interpolation.
  Every other style defaults to Omni Flash.
- **Suggested voice** maps the style's `voiceover.voice_character` to the five xAI voices (eve
  energetic, ara warm, rex confident, sal smooth, leo authoritative). the Low-Poly reference calls for a child
  voice, which is not available; ara (warm) is the closest. Confirm the voice with the user; it is
  easy to swap.
- **In-world headline = Yes** means the style bakes headline text into the world at image
  generation (per the style file's `typography.headline_treatment`). **No** means the style only
  ever carries spoken-word subtitle captions, added at the end with `advibly_add_subtitles`.
