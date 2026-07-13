# Motion Collage: the dramatic looks, done entirely on the MCP

The reference workflow this skill grew from ran a local Python engine (Pillow + ffmpeg) to
cut a poster into pieces and hand-animate each one: the pieces-fly-in-and-assemble look,
drifting confetti, camera shake on impact, whip transitions. On Advibly there are **no
scripts and no local frame engine.** Every one of those looks is reachable through the MCP,
two ways:

1. **As motion instructions in the video prompt** (the default, works for almost everything).
   The `collage-motion` skill already proves this: it animates flat collage stills
   into "empty color field, cut-out pieces slide in and snap into place" clips with
   `gemini-omni-flash`. Same engine, same idea here.
2. **As a start-frame plus end-frame reveal on Seedance** when you need the build to land on
   an exact finished poster (omni has no end frame).

Cut-out elements, when you truly need one, come from the **image model used as a
background-remover** (custom isolate prompt + the photo as a reference). No `rembg`, no
`youchuan`, no transparent-PNG pipeline.

Use these on the beats that earn a punch (the hook, the product reveal, the payoff). A film
where every shot fires confetti and shakes reads as a formula; the reference films used the
big moves once or twice and let the other beats breathe.

---

## 1. Assemble-from-empty (the poster builds itself)

The signature "motion collage" move: the frame begins as a nearly bare paper field and the
cut-out pieces fly in and snap together into the finished poster.

### The soft version (omni-flash, one call)

Start from the **finished poster** and let the motion prompt describe the build. Omni
interprets it as pieces settling in from an almost-empty state. Good enough for most beats,
one generation, no end frame needed.

```
advibly_generate_video
  model: "gemini-omni-flash"
  start_image_url: <the finished poster URL>
  prompt: |
    Animate this still into a paper-collage MOTION GRAPHIC, printed cut-outs, not photoreal.
    The frame starts as a nearly bare {bg} paper field, then the cut-out pieces fly in from
    the edges one after another and snap into their places, building this exact finished
    collage, and lock still on the complete poster for the final beat. Rigid flat paper, each
    piece slides and snaps as one solid cut-out. The headline lands last and stays sharp and
    perfectly legible. Keep the torn-paper, tape and halftone textures. One continuous build,
    no morphing, no melting, flat 2D. Audio: paper foley only (soft slides, snaps, a settle
    thunk), quiet room tone, no voiceover, no spoken words, no music with lyrics.
  aspect_ratio: <9:16 or 16:9>
  duration: <6 to 8>
  brand_id: <id>
```

### The hard version (Seedance start-frame to end-frame)

When the build must land on the exact poster (a product reveal, the payoff), give Seedance a
**near-empty field as the start** and the **finished poster as the end**. Seedance
interpolates the assembly and lands precisely.

- Make the empty-field start image with one image call: the theme's style block, the same
  `{bg}` paper background, and "an almost bare paper field, only faint paper texture and a
  couple of stray scraps at the edges, no headline yet". Same aspect, same texture language
  as the poster.
- Then:

```
advibly_generate_video
  model: "seedance-2.0"
  mode: "fast"
  start_image_url: <the empty-field image>
  end_image_url:   <the finished poster>
  prompt: <the cut-out pieces fly in from the edges and assemble into the final collage; rigid flat paper; the headline resolves last; paper foley only, no spoken words>
  aspect_ratio: <any>
  duration: <5 to 8>
  brand_id: <id>
```

One model per delivered ad still holds: if you use the Seedance route for the reveal, run
the whole ad on Seedance.

---

## 2. Hero element flying across the frame

A single cut-out (a paper bird, a coin, an arrow, a product) travels across the whole frame.
A great **occasional** punch, wrong as a per-shot habit. Phrase it on the element-motion
axis of a normal image-to-video call:

> "a cut-out paper {bird} flaps across the whole frame from lower left to upper right,
> casting a soft paper shadow, while the rest of the scene drifts gently underneath it"

If you want that hero piece to be a real isolated cut-out (crisp edges, its own shadow, not
redrawn), make it with the background-remover recipe in §5 first, then this is just its
motion.

---

## 3. Confetti, scatter, starburst (tactile energy, no overlay asset)

The reference engine drew confetti and starbursts procedurally in code. On Advibly they are
motion-prompt lines on the beat that needs them:

- **Confetti drift:** "small paper confetti chips and torn scraps drift and tumble down
  across the frame throughout, catching the light, at different depths."
- **Scatter burst:** "on the beat, a scatter of paper {coins / petals / scraps} bursts
  outward from the center and settles, each piece a rigid flat cut-out."
- **Starburst pulse:** "a cut-out paper starburst behind the {hero} slowly rotates and pulses
  on the beat, rays breathing."

Keep them rigid paper and keep the text stable. Confetti reads best on a payoff or CTA beat,
or the ending.

---

## 4. Camera shake, impact kicks, whip

- **Impact kick:** "as the {element} lands, the whole frame gives one small sharp shake that
  settles immediately, like a stop-motion bump." One shake per landing, never a continuous
  wobble (that warps text).
- **Whip settle within a shot:** "a quick whip-pan sweep that settles onto the composition
  and holds steady." Good on a hook.
- **Whip as a transition between shots.** `advibly_render_composition` does hard cuts only, so a
  whip *between* two shots has two routes: (a) end shot A on a whip-out and open shot B on a
  whip-in via their motion prompts, so the cut hides inside the blur, or (b) skip the MCP
  stitch and concat locally with an ffmpeg `xfade` whip (see `models-and-gotchas.md`), then
  compose the audio. Route (a) is all-MCP and usually enough. Reserve whips for a couple of
  transitions, not every cut.

---

## 5. Cut out a real element with the image model (no local tools)

Advibly has no background-removal tool, so use the image model as one. Call
`advibly_generate_image` with the source photo as a reference and a prompt that isolates it:

```
advibly_generate_image
  model: "nano-banana-2"        # or gpt-image-2; both isolate cleanly
  on_brand: false
  reference_image_urls: [<the source photo URL>]
  prompt: |
    Isolate the {object} exactly as it is in the reference, remove everything else, and place
    it centered on a plain flat {color} paper background with a soft real contact shadow. Do
    not redraw, restyle, or recolor the object; keep its exact shape, label and texture.
  brand_id: <id>
```

You get the element sitting on a clean flat card you can then composite into a poster (pass
it back in as a reference) or hand to the hero-fly-across motion in §2. On the standard path
you rarely need this: composite the product straight into the poster prompt instead (see
`prompt-guide.md` §1). It earns its cost only when a piece must move independently or land
with pixel-clean edges.

---

## When to stay on the standard path

If "living poster" motion (parallax, bob, drift, a rich element-motion line) already sells
the beat, stay there: it is one image-to-video call and fully automated. Assemble-from-empty,
isolated hero pieces, and Seedance end-frame reveals are worth the extra calls only on the
one or two beats where the drama pays for itself.
