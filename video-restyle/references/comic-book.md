# Template: comic-book

Western pop-art comic book. The whole frame is redrawn as a printed comic
panel: bold black ink outlines, flat saturated primaries, coarse Ben-Day
halftone dot shading, action starburst backgrounds, burst clouds and star
shapes. Where anime-manga is Japanese cel and screentone, comic-book is
American newsstand pop art: thicker outlines, louder primaries, POW energy.

**Pick it when**: the content is punchy, hype, opinionated, or announcement
shaped (hot takes, product drops, sports talk, reaction clips) and the user
wants a loud drawn look rather than the photoreal sticker of podcast-pop.

**Subject treatment: full repaint.** The person becomes a comic-book drawing
OF THE SAME person. Replace the identity block's opening with this variant:

```
Transform this clip into a pop-art comic book version of the exact same
scene. The person becomes a hand-inked comic drawing of themselves: the same
recognizable face, the same identity, the same expressions, the same mouth
movements and lip-sync, the same hairstyle and clothing, matching the source
frame for frame. Their spoken performance continues unchanged. Foreground
props they hold or touch stay present, drawn in the same style.
```

## Style block (verbatim in every segment prompt)

```
Style: a vintage American pop-art comic book panel in motion. Bold clean
black ink outlines around every form, flat saturated primary colors, coarse
Ben-Day halftone dots shading the skin and clothing, crisp cel fills with
no photographic texture. The background is a comic action backdrop that
keeps a slow constant motion so the panel feels alive. A rectangular yellow
comic caption box with bold black comic lettering sits in the lower third.
The whole frame reads as one printed comic page brought to life, ink slightly
glossy, paper white kept bright.
```

## Variation axes (rotate per segment)

**Axis 1: the action backdrop.** Rotate so no two adjacent segments repeat.

1. **Starburst rays**: giant radiating sunburst rays exploding from behind
   the subject, slowly rotating, in the segment's two palette colors.
2. **Ben-Day field**: a flat color field covered edge to edge in coarse
   halftone dots that drift diagonally, one hand-drawn burst cloud floating.
3. **Panel grid**: the background is a wall of smaller comic panels showing
   freeze-frame drawings of the same person, gutters bright white, the grid
   sliding slowly sideways.
4. **Burst clouds and stars**: jagged white-outlined burst clouds and small
   stars scattered around the subject, bobbing gently.

**Axis 2: the palette.** One loud pair per segment, never repeated on
adjacent segments: sunburst yellow + fire red, cyan + magenta, royal blue +
orange, red + halftone black on white, acid green + purple.

**Axis 3: onomatopoeia bursts.** On the segment's emphasis word, one short
comic sound word inside a jagged burst shape snaps in near the subject
(POW, BAM, BOOM, ZAP, WOW). One per segment maximum, 3-4 letters, spelled
exactly. Skip it on segments that carry an impact frame instead.

## Energy moves

- **Impact frame**: on the strongest emphasis word, "exactly on the word X
  the panel flashes to a high-contrast two-color impact frame with radiating
  speed rays for a beat, then returns". One per video at most twice.
- **Punch-in**: on an emphasis word, the framing punches in tighter on the
  drawn face, ink outlines thickening slightly, then eases back.
- **Panel cut**: on a phrase turn, the backdrop hard-cuts to the next motif
  and palette in the rotation, like turning a page.

## Beat vocabulary (anchor these to words)

- **Ray spin-up**: on the opening word, the starburst rays sweep in and
  settle into slow rotation.
- **Onomatopoeia snap**: exactly on the emphasis word, the burst word slams
  in beside the subject and holds for a beat.
- **Dot drift shift**: on a mid-segment word, the halftone dots change
  direction or density.
- **Caption stamp**: on the segment's final strong word, the yellow caption
  box stamps down and the panel settles for the cut.

A packed 8-10s segment uses two or three of these; the emphasis word gets the
onomatopoeia snap, impact frame, or punch-in.

## Caption treatment

The yellow comic caption box from the style block. Per-segment line:

```
The yellow comic caption box reads exactly "THIS CHANGES EVERYTHING",
spelled exactly, every word correct, bold black comic lettering, anchored
in the lower third.
```

3-6 words. The box stays in the same screen position across segments.

## Example beat timeline

Segment spoken line: "Everyone said it was impossible. Then we shipped it in
a week." (9s, emphasis on "shipped")

```
The segment runs this beat timeline over the spoken words:
- It opens on the word "Everyone" with giant sunburst-yellow and fire-red
  rays radiating from behind the drawn person, rotating slowly, Ben-Day dots
  shading their face and shirt.
- Exactly on the word "impossible" the framing punches in tighter on the
  drawn face, ink outlines thickening, then eases back.
- Exactly on the word "shipped" a jagged white burst shape slams in beside
  the person reading exactly "BAM", spelled exactly, and the yellow comic
  caption box stamps down reading exactly "SHIPPED IN A WEEK", spelled
  exactly, every word correct, bold black comic lettering in the lower
  third, then the panel settles. The person's voice and the room's sound
  continue naturally.
```

## Failure modes

- **The face went generic** (a drawn stranger): reinforce "the same
  recognizable face, a comic drawing of this exact person" and re-roll.
  Busy backdrops steal capacity from the face; simplify axis 1 if it
  persists.
- **Output looks like a photo with a filter**: the print lines got trimmed.
  Restore "bold black ink outlines", "flat cel fills", "Ben-Day halftone
  dots", "no photographic texture".
- **Onomatopoeia misspelled or multiplied**: keep it 3-4 letters, say
  "exactly one burst word", re-roll before rewriting.
- **Caption paraphrased**: re-roll same prompt; then shorten to 3-4 words.
- **The panel sits static**: the beats were dropped. Restore two or three
  events anchored to exact words (ray spin-up, punch-in, onomatopoeia snap,
  caption stamp).
- **Lip-sync smeared under the ink**: add "the mouth shapes stay crisp and
  readable through the ink shading" and re-roll.
