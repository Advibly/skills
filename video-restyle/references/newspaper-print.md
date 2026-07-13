# Template: newspaper-print

The subject, kept photoreal but printed in coarse black-and-white halftone,
lives inside a broadsheet newspaper world: headline type, column rules,
folded paper texture, with one red editorial marker slashing accents through
the monochrome. Serious, punchy, opinion-page energy.

**Pick it when**: the content is a take, an argument, news-jacking, finance,
or anything that benefits from "read all about it" authority with a modern
edit's pace.

**Subject treatment: cutout-real, printed.** The person keeps their exact
face and performance but is rendered as a high-quality halftone newspaper
print of themselves. Use the standard identity block from SKILL.md, then the
style block handles the printing.

## Style block (verbatim in every segment prompt)

```
Style: a living broadsheet newspaper. The person appears as a crisp
black-and-white halftone newspaper print of themselves, coarse dot pattern
visible up close, cut out and mounted over a newsprint page background:
off-white paper stock, dense headline typography, narrow justified text
columns, thin column rules, a visible fold crease. One spot color exists in
this world: a red editorial marker used for circles, underlines and arrows.
Everything else is ink black on newsprint. The page elements drift subtly
so the frame feels alive. The whole frame reads as a designed front page in
motion.
```

## Variation axes (rotate per segment)

**Axis 1: the page layout.** Rotate: a giant headline block filling the
space behind the subject, a multi-column text page with the subject mounted
over it like a front-page photo, a collage of torn newspaper clippings at
angles, a classified-ads grid texture, a near-empty paper field with one
huge printed word.

**Axis 2: the red accent.** Exactly one red element per segment, rotated:
a hand-drawn red circle around the subject or a background word, a thick
red underline sweeping beneath the caption, a red arrow pointing at the
subject's gesture, a red "corrections" cross through a background word, a
red stamp mark (no readable stamp text).

**Axis 3: headline words.** Background headline type pulls 1-3 words from
the segment's `topics` or caption, printed in huge condensed serif caps.
Background words are texture; keep them few so the model spells them right.

## Energy moves

- **Punch-in**: works well here; "exactly on the word X the framing punches in
  tighter on the person's face, front-page portrait crop".
- **Red moment**: the strongest word gets the red accent drawn ON it ("exactly
  on the word X a red circle snaps around the person").
- Palette never rotates in this template (that is the point); the layout axis
  carries the variety, and a layout swap on a spoken word is a legitimate beat.

## Beat vocabulary (anchor these to words)

The page assembles and annotates itself on exact spoken words:

- **Headline set**: on the opening word, the big background headline type prints
  itself in across the page.
- **Layout swap**: on a phrase turn, the page turns to the next layout in the
  rotation (a clippings collage, a column page, a single huge word).
- **Red mark**: exactly on the emphasis word, the one red element draws in (a
  circle, an underline sweep, an arrow) as described above.
- **Punch-in**: on the strong word, the front-page portrait crop.
- **Headline lock**: on the final word, the caption strip locks in the lower
  third.

A packed 8-10s segment uses two or three; the emphasis word gets the red mark
or the punch-in.

## Caption treatment

```
A headline caption reads exactly "THE MARKET IS WRONG", spelled exactly,
every word correct, printed in heavy black condensed serif capitals inside
a white paper strip in the lower third, like a front-page headline.
```

## Example beat timeline

Segment spoken line: "Everyone missed the fine print, and it cost them
millions." (9s, emphasis on "millions")

```
The segment runs this beat timeline over the spoken words:
- It opens on the word "Everyone" over a collage of torn newspaper clippings
  mounted at slight angles, drifting subtly, the background headline type
  printing itself across the page.
- Exactly on the word "fine print" a thick red underline sweeps beneath the
  caption strip, and the caption reads exactly "READ THE FINE PRINT", spelled
  exactly, every word correct, heavy black condensed serif capitals in a white
  paper strip, lower third.
- Exactly on the word "millions" the framing punches in tighter on the person's
  face in a front-page portrait crop and a red circle snaps around them, then
  holds. The person's voice continues naturally.
```

## Failure modes

- **The subject rendered in color**: reinforce "the person is printed in
  black-and-white halftone, no color on the person" (positive framing:
  "monochrome halftone person").
- **Background paragraphs are gibberish**: fine at texture size; if
  readable-large and wrong, shrink them ("small dense text columns") or swap
  to the classified-grid layout.
- **More than one red element**: say "exactly one red element in the frame".
- **Halftone dots crawl or shimmer**: add "the halftone dot pattern stays
  steady and printed".
- **Modern glossy look**: restore "off-white newsprint paper stock" and
  "visible fold crease".
- **The page sits still**: the beats were dropped. Restore 2-3 beats (headline
  set, layout swap, red mark, punch-in) anchored to exact words; pack the
  segment to 8-10s.
