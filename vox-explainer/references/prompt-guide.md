# Prompt Guide: the heart of the Vox collage look (the LOOK layer)

Two prompts decide whether the ad reads as a real Vox collage or a moving poster: the
**image prompt** (makes the collage look) and the **video/motion prompt** (makes it move like
collage). Get these right and the rest is plumbing.

This one file is the whole LOOK layer: the prompt structures, the vocabulary that fills them,
and the ready-made theme presets. (The STORY layer, narrative arcs and pacing and shot
patterns, is its counterpart `beat-layer.md`.)

Everything below sits on top of the **common Vox constraints** (never drop these):

> torn or scissor-cut paper edges · tape · halftone dots · newspaper clippings ·
> paper-stencil shapes · real paper drop shadows · bold flat color · PRINTED, illustrated
> cut-outs · NOT 3D, NOT CGI, NOT photoreal · keep print grain · headline text baked in
> "quotes".

**The one Advibly exception:** the real product photo. On product beats the actual product
(pouch, bottle, box, app screenshot) is composited into the collage **unchanged and
photoreal**, with its own soft contact shadow, a real object placed in a paper world. Never
let the model redraw it as a cut-out; pass the photo in `reference_image_urls` and say it in
words every time. Everything else in the frame obeys the constraints above.

Contents: 1. Image prompt · 2. Video prompt · 3. Layering · 4. Real people and brands ·
5. Theme presets.

---

## 1. Image prompt

The collage look is *born here*. This single image carries the entire aesthetic of its shot,
so invest in it. Structure every image prompt in 5 parts (plus the product part on product
beats):

```
[1 STYLE BLOCK - identical on every poster; the theme preset fills the specifics]
  Mixed-media hand-cut PAPER COLLAGE, editorial motion-graphic zine style. Clearly layered
  paper cut-outs with visible torn and scissor-cut edges, tape corners, and soft real paper
  drop shadows. Halftone print dots, newspaper-clipping scraps, paper-stencil shapes, aged
  paper texture, slight print misregistration. Scattered geometric collage accents (a small
  paper triangle, a circle, a zigzag, halftone strips, bits of washi tape). Figures are
  PRINTED-texture cut-outs of real imagery (photo, woodblock, mural), NOT CGI, NOT a 3D
  render. Keep print grain and paper imperfections. High-contrast, punchy, tactile,
  hand-assembled.

[2 SCENE - describe as SEPARATE cut-out pieces]
  SCENE as layered paper cut-outs: {main subject}, {a prop}, {a text strip}, {a decorative
  scrap}; elements have clear edges, distinct layers, each with its own drop shadow.

[2.5 PRODUCT - product beats only]
  The real {product} photo, unchanged and photoreal, composited into the collage with its own
  soft contact shadow, label crisp and readable; everything around it stays printed paper.

[3 BACKGROUND - one bold flat color]
  on a bold flat {bg color} paper background.

[4 HEADLINE - baked in, short and bold; title shots only]
  A torn-paper banner with a big bold headline "{TITLE}" in {the theme's type style},
  {placement}.

[5 TECH]
  aspect ratio {9:16|16:9|1:1}, 2k resolution.
```

### Techniques

- **Model choice is a texture decision, not a text decision.** The whole look lives in the
  paper grain, and `nano-banana-2` holds that grain the best. It is the default keyframe
  model for this skill; it also renders baked headline text cleanly and takes the product
  photo as a reference. Reach for `gpt-image-2` only when a specific poster's headline keeps
  degrading (it is the strongest at dense baked text) or when a product-reference edit fights
  the collage on nano; `seedream-5-pro` is the third option for very dense typographic
  layouts. Whatever wins the Phase 3 bake-off is the one model used for the whole ad. If a
  set keeps coming back digital-smooth and plasticky, that is the model flattening the paper:
  re-roll on nano-banana-2 before touching the prompt. See `models-and-gotchas.md`.
- **Reuse the style block verbatim on every poster.** Only the scene, background color, and
  headline change. This is what makes 12 shots feel like one film.
- **Describe distinct pieces with visible edges and shadows.** Two reasons: it looks
  assembled (collage, not a smooth painting), and it gives the video model separable layers
  to parallax (see §3). A blended scene can only be panned as one flat plane.
- **Bake the headline into the image.** Image models render crisp text; video models smear
  it. Keep it short and bold, 2-3 words, exact words in "quotes".
- **One bold flat background color per beat.** Busy backgrounds muddy the cut-out silhouettes
  and kill the Vox punch. Let the palette travel across beats to carry mood (aged sepia >
  bold pop > champion gold; or cool problem tones > warm product tones).
- **Say "NOT 3D, NOT CGI, printed texture, keep grain"** or the model drifts toward smooth
  render-CGI and loses the paper feel.
- **2k resolution** so cut-outs stay crisp when animated.
- Keep a **consistent paper-texture language** across posters (edge roughness, halftone
  density). Color can change per beat; texture must not.
- Prompts cap at 2000 characters on the Advibly tools: trim adjectives, never the layering
  description or the product compositing line.

### Worked example (product-reveal beat, deep red)

> Mixed-media hand-cut PAPER COLLAGE, editorial motion-graphic zine style. Clearly layered
> paper cut-outs with visible torn and scissor-cut edges, tape corners, and soft real paper
> drop shadows. Halftone print dots, newspaper-clipping scraps, paper-stencil shapes, aged
> paper texture, slight print misregistration. Scattered geometric collage accents (a small
> blue paper triangle, a red circle, a zigzag, halftone strips, bits of washi tape). Figures
> are PRINTED-texture cut-outs, NOT CGI, NOT a 3D render, keep print grain and paper
> imperfections. High-contrast, punchy, tactile, hand-assembled. SCENE (as layered paper
> cut-outs): torn-paper arrows converging inward from all four edges, a radial paper
> starburst, scattered geometric scraps, clear edges and drop shadows. The real Acme Creatine
> black pouch photo, unchanged and photoreal, centered in the starburst with its own soft
> contact shadow, label crisp and readable; everything around it stays printed paper. On a
> bold flat deep-red paper background. A torn-paper banner with a big bold cut-out headline
> "THE REAL ONE" in bold condensed grotesque, upper third. Aspect ratio 9:16, 2k resolution.

Note the density: this single prompt runs about 900 characters and stays well under the
2000-char cap. Do not thin it out. The concrete accents (the blue triangle, the washi tape,
the misregistration) are what make the model print a collage instead of a flat illustration.

### The image dimensions (pick one term per axis; star = strongest theme levers)

| Dimension | Controls | Vocab bank (mix and match) |
|---|---|---|
| **Medium / technique** | the strongest style lever | paper collage · torn-paper collage · photomontage · screenprint · risograph · letterpress · linocut / woodcut · lithograph · halftone print · gouache · photocopy / xerox · rubber stamp |
| **Art movement / era** ⭐ | the single best theme knob; shifts palette, type, and layout at once | Swiss / International Typographic · Bauhaus · mid-century modern · Russian Constructivism · Dada photomontage · Pop Art · psychedelic 60s · punk zine · Memphis · Art Deco · WPA / vintage travel poster · Soviet · atomic age · retro-futurism |
| **Composition / layout** ⭐ | poster hierarchy and negative space | modular grid · asymmetric · strong negative space · diagonal dynamic · foreground-midground-background depth · hero headline plus subhead hierarchy · full bleed · radial · stacked bands · off-center focal |
| **Color palette** ⭐ | the cleanest theme distinguisher; stops muddy gradients | limited 2-3 color · duotone · monochrome plus one accent · riso fluorescent pink plus federal blue · Bauhaus primaries · 70s mustard / rust / avocado · 60s pop (hot pink, orange) · teal and orange · CMYK process · cream / kraft base · neon on black · named hex |
| **Typography treatment** ⭐ | the headline LOOK (we only passed the words) | exact words in "quotes" plus a REAL type style: bold condensed grotesque (Helvetica, Akzidenz) · geometric sans (Futura, Bauhaus) · slab serif · 70s display serif · wood type · hand-lettered · ransom-note cut-out letters · stencil · all caps · knockout / reversed · huge headline plus small caption |
| **Print finish / texture** | "made by hand, not AI-slick" | halftone dots · Ben-Day dots · riso ink misregistration / overprint · letterpress deboss · newsprint · kraft / cardstock · aged, foxed paper · fold creases · deckled vs scissor-cut edges |
| **Lighting / capture** | keeps it "scanned", not "rendered" (anti-3D) | flat even studio light · shadowless scanned-document light · straight-on, head-on · flat-lay top-down · subtle paper drop shadows |
| **Mood** | tone anchor | playful · bold · urgent · editorial, serious · nostalgic · optimistic · ominous · satirical · activist, protest |

**One-line image structure** (works across gpt-image-2, nano-banana-2, seedream):
`[MEDIUM] + [ART MOVEMENT/ERA] of [SUBJECT + SCENE as cut-out pieces], [COMPOSITION],
[straight-on scanned-flat framing, flat even light, paper drop shadows], [named limited COLOR
PALETTE], [PRINT FINISH], headline "EXACT WORDS" in [NAMED TYPE STYLE] [placement],
[PRODUCT line if a product beat], [MOOD], [aspect]`. Phrase positively; these models take no
negative field, so "NOT 3D, NOT CGI" belongs inside the style block as description.

---

## 2. Video prompt

The motion prompt turns "pan a poster" into "a living collage". Use the 5-axis structure.
All five axes matter, especially FEEL and COLOR, which set pacing and grade:

```
[GOAL] Animate this still into a mixed-media collage MOTION GRAPHIC. Keep it flat 2D paper,
       NOT 3D, no photoreal (the composited product photo is the one photoreal element and
       stays exactly as it is).

[AXIS 1 · CAMERA]   one smooth continuous move for this shot, from the beat map's
                    camera_move: {slow push in | lateral parallax pan | locked-off static}.
[AXIS 2 · MOVEMENT] the beat map's element_motion, written rich: layered paper cut-outs
                    drift with visible drop-shadow parallax; named elements bob, scatter,
                    hinge, flap; halftone dots pulse; torn edges and tape flutter; a
                    breathing quality.
[AXIS 3 · AESTHETIC]preserve the torn-paper, tape, halftone, newspaper, paper-stencil
                    textures exactly; keep the bold flat background.
[AXIS 4 · FEEL]     the beat's feel: tender / urgent / triumphant / editorial, "a page from
                    a scrapbook".
[AXIS 5 · COLOR]    this beat's palette, high contrast; if part of an arc, name where it
                    sits (aged sepia > bold pop > champion gold).

[CONSTRAINTS] keep the layout and ALL on-screen text perfectly stable and legible; the real
              product photo stays sharp, photoreal, and unwarped; ONE smooth continuous
              move; absolutely no sudden zoom snaps, no jump cuts, no teleporting or
              re-framing inside the shot; no new objects, no morphing, no drift.

[AUDIO] paper foley only: {tear / rustle / soft whoosh / gentle settle}, quiet ambient room
        tone. No voiceover, no spoken words, no narration, no music with lyrics.
```

### Techniques

- **The MOVEMENT axis is the lever** that makes the model move *layers* (parallax) instead of
  sliding the whole frame. Name the layered paper motion explicitly.
- **FEEL and COLOR are not decoration.** A "tender, sepia" beat animates slower and warmer
  than a "triumphant, gold" beat. Do not drop them.
- **The CONSTRAINTS axis prevents the common breaks:** text wobble, 3D drift, product warp,
  and the big one, internal jump cuts.
- **Never write "snap", "punch-in", "slam", or "quick zoom".** Video models over-react and
  generate a jump inside the shot that reads as a one-frame flash. Ask for ONE smooth
  continuous move.
- **Do not restate the picture.** The still already has subject, scene, text, style;
  restating them (especially the text) makes the model re-synthesize and warp them. Describe
  motion, keep the rest as "preserve".
- **One camera move plus one action cluster per shot.** For richer editing, cut between
  multiple short shots (wide plus detail) rather than asking one clip for a timeline.
- **The AUDIO axis is mandatory on Advibly:** clips ship SFX-only because the voiceover is
  composed externally. Any spoken words in a clip collide with it.

### The stability axes (what actually fixes loop, wobble, and morph failures)

| Axis | Controls | Phrasing |
|---|---|---|
| **Motion amplitude** ⭐⭐ | THE anti-morph, anti-text-warp lever | "very subtle" · "minimal" · "moderate" · "about 5 percent movement" · avoid "intense", "large", "explosive" near text |
| **Dimensional lock** ⭐ | keep it flat | "flat 2D, camera parallel to the poster, no 3D rotation, no perspective change; paper layers parallax only, elements slide or pivot as rigid flat paper, do not bend or morph" |
| **Stability anchors** ⭐⭐ | protect text, layout, product | "the printed headline and layout stay sharp, legible and perfectly stable; do not redraw, distort or move the lettering; the product photo stays photoreal and unwarped" |
| **Lighting over time** | the safest motion for flat art | "soft light sweep across the surface" · "gentle shadow drift" · "subtle glow pulse" |
| **Shot structure** | cuts and loops | "single continuous shot, no cuts, no scene changes" · give one-way moves a settle endpoint ("...then settles into place") |

### Advanced motion vocabulary (the dramatic looks, all via the motion prompt)

The dramatic "motion collage" looks (pieces flying in and assembling, confetti, camera
kicks, whip sweeps) are not a separate engine on Advibly. They are the element-motion axis
pushed harder, phrased for the video model. Reach for them on the beats that earn a punch
(the hook, the product reveal, the payoff), not every shot. Full recipes in
`motion-collage.md`; the phrasing bank:

| Look | What it does | Motion-prompt phrasing (positive, for omni-flash) |
|---|---|---|
| **assemble-from-empty** | the poster builds itself | "the frame starts as a nearly bare {bg} paper field, then the cut-out pieces fly in from the edges one after another and snap into their places, building the finished collage, and lock still on the complete poster" |
| **fly-in-and-snap** | one piece lands hard | "the {element} cut-out flies in from off-frame, overshoots slightly, and snaps into place with a paper thwack" |
| **confetti / paper scatter** | tactile celebration energy | "small paper confetti chips and torn scraps drift and tumble down across the frame throughout, catching the light" |
| **starburst pulse** | emphasis behind a hero | "a cut-out paper starburst behind the {hero} pulses and slowly rotates, rays breathing on the beat" |
| **camera shake / impact kick** | weight on a landing | "as the {element} lands, the whole frame gives one small sharp shake that settles immediately, like a stop-motion bump" |
| **whip settle** | fast energy that resolves | "a quick whip-pan sweep that settles onto the composition and holds steady" |

Rules that keep these from breaking: keep the pieces **rigid paper** (they slide, flap,
hinge, scatter; they never melt, morph, or bend organically), keep the **headline and
product photo stable**, keep it **flat 2D**. A camera shake is one short settle, never a
continuous wobble. For a hard, exact assemble-from-empty landing (bare field to finished
poster), Seedance with `start_image_url` = a near-empty paper field and `end_image_url` =
the finished poster is more reliable than asking omni to invent the build. See
`motion-collage.md`.

### Model-specific rules (Advibly's three video models)

1. **gemini-omni-flash (default).** 4-10s, 9:16 and 16:9 only, no end frame. The best text
   stability and layered parallax on flat art, which is exactly what collage motion needs.
   Positive phrasing ONLY: "no" and "don't" can produce the opposite; convert every
   constraint into a positive ("the camera stays locked" instead of "no camera movement",
   "the lettering stays exactly as printed" instead of "don't redraw the text"). Blocks
   recognizable real people and third-party logos.
2. **seedance-2.0 (fallback).** Any 4-15s, all aspect ratios, supports an optional
   `end_image_url` alongside the start frame: switch the ad to Seedance when a reveal must
   land on an exact finished poster, when the format is 1:1 or another non-vertical aspect,
   or when a shot needs more than 10s. Supports negative phrasing. Also blocks real people
   and third-party logos.
3. **kling-v3 (real people and brands).** 3-15s, native audio (keep `generate_audio` on for
   the foley), supports negative phrasing and an end frame. The only model here that allows
   recognizable celebrities and marks.

Headline protection is per shot: anchor the text hard on `"title": true` shots; detail shots
without a headline are free to move wilder.

---

## 3. Layering

Can you feed a background plus separate component images to the video model and get dramatic
per-component motion? In practice, no: reference-to-video is generative, not a layer
compositor. It reinterprets the references and invents its own motion, so you get less
control, not more.

The real lever inside the standard path: **the more clearly layered the poster, the more
layered motion the video model produces.** Distinct cut-outs with edges and shadows let the
model drift them at different depths (parallax), which is the "living collage". A flat,
blended image can only pan as one plane. So push the layering in the image prompt (§1) if you
want livelier motion.

**Cutting out one real element (via the image model, no local tools).** When you genuinely
need a clean cut-out (a hero piece to composite, or a product isolated onto a paper card),
Advibly has no background-removal tool, so use the image model as one: call
`advibly_generate_image` with the source photo in `reference_image_urls`, `on_brand: false`,
and a prompt like "isolate the {object} exactly as it is, remove everything else, place it
centered on a plain flat {color} paper background with a soft contact shadow, do not redraw
or restyle the object". `nano-banana-2` and `gpt-image-2` both do this cleanly. You rarely
need it on the standard path (composite the product straight into the poster instead); it is
mainly for the `motion-collage.md` techniques.

---

## 4. Real people and third-party brands

Real-person or celebrity posters are fine to *generate* (use a reference photo to anchor the
face). The catch is *animation*: Seedance and Omni Flash refuse recognizable celebrities and
brand logos at the content-filter level, and removing the name from the prompt does not help;
it is the image content that trips it. Route the whole ad to `kling-v3` in that case. The
user's own brand and product are not "third-party": the product photo composites fine.

---

## 5. Theme presets: combine the dimensions

A **theme preset** is one pick from each image dimension (§1) plus a motion amplitude (§2),
on top of the common Vox constraints. This is how the skill offers different looks without
hand-writing each prompt. The library:

| Preset | Era / movement | Palette | Type style | Print finish | Motion | Fits |
|---|---|---|---|---|---|---|
| `american-retro` | 1950s US ad, pulp | bold retro primaries | wood type, bold slab | halftone, aged | punchy | DTC products, sports, money, food |
| `swiss-modern` | Swiss / International Typographic | 2-color plus red accent | Helvetica / Akzidenz grotesque | clean flat, subtle grain | calm | SaaS, tech, finance, explainers |
| `punk-zine` | 90s punk DIY | black and white plus one spot color | ransom-note cut letters | photocopy, misregistration | punchy / max | streetwear, music, counterculture, gaming |
| `soviet-constructivist` | Russian Constructivism | red, black, cream | bold condensed diagonal | letterpress, newsprint | punchy | bold claims, industry, "revolution" angles |
| `wpa-propaganda` | 1930s WPA poster | muted 3-color | stencil, gothic | screenprint grain | calm | health, public-good, heritage brands |
| `70s-groovy` | 1970s | mustard, rust, avocado | bulbous display serif | riso grain | punchy | food, culture, nostalgia, lifestyle |
| `chinese-ink` | Chinese woodblock and ink | ink plus vermilion | brush lettering plus red seal | rice paper, seal | calm / punchy | Chinese culture, tea, heritage topics |
| `atomic-age` | 1950s futurism | teal, orange, cream | atomic script | halftone | punchy | science, space, gadgets, "the future" |
| `paper-craft-cream` | editorial paper craft (premium DTC) | warm cream, grey, kraft plus ONE brand accent | heavy grotesque, serif for gravitas, torn-paper label chips | handmade fiber-grain paper, soft cut-paper drop shadows, no newsprint | calm | supplements, wellness, premium DTC, "quiet luxury" |

`paper-craft-cream` is the house special for premium DTC: the whole ad lives on one textured
warm-cream handmade-paper canvas instead of a bold color per beat, ideas arrive by paper
physically tearing, unrolling, and peeling, and restraint (near-monochrome plus a single
brand accent per beat) is the premium signal. Signature reveal moves to name in scenes and
motion: torn newspaper reveal, radial tear-burst, scroll unroll, sticker peel, book open,
crack open, cut-outs slide and settle, arrows converge, glow pulse.

**How to use:** read the claim and the brand, suggest 3-4 fitting presets (or compose a
custom theme by mixing the §1 banks), render beat 1's wide poster once per candidate (the
bake-off), and let the user pick by eye. The picked preset's fields fill the style block,
palette, and type slots of every image prompt, and its motion amplitude seeds every video
prompt. The common Vox constraints and the stability anchors are always on. Match the topic
and the brand, not the language the user typed in.
