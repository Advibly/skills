# Template: podcast-pop

The viral podcast-clip edit. The subject is lifted out of their original
background as a die-cut sticker with a thick white outline and placed over
bold, loud, constantly rotating backgrounds. Torn-paper caption bands carry
the words. Punch-ins hit on emphasis. This is the template for talking-head
content that needs to feel edited, fast, and platform-native (the
arcads/omniflash demo look).

**Pick it when**: the video is a person talking to camera (UGC, podcast clip,
founder rant, hot take) and the user wants maximum retention energy without
changing who the person is.

**Subject treatment: cutout-real.** The person stays fully photoreal. Only
the world around them is replaced. Use the identity preservation block from
SKILL.md verbatim.

## Style block (verbatim in every segment prompt)

```
Style: the person is cut out of their original background as a die-cut
sticker with a thick clean white outline and a soft drop shadow, placed
centered over a bold flat graphic background. The person stays completely
photoreal inside the sticker. A torn-paper caption band sits in the lower
third with bold black condensed uppercase lettering on white, like a strip
ripped from a poster. The background has a slow constant drift so it feels
alive. Flat colors are saturated and high-contrast. The whole frame reads
as a premium social-media podcast edit.
```

## Variation axes (rotate per segment)

**Axis 1: the background look.** Four looks; rotate so no two adjacent
segments repeat. A 4-segment video uses each once; longer videos cycle with
different palettes on the second lap.

1. **Kinetic typography**: the background is filled edge to edge with giant
   bold condensed sans-serif words pulled from this segment's caption,
   repeating in rows, slowly scrolling sideways. Two-color palette (see
   axis 2).
2. **Crumpled paper doodle**: a full-bleed sheet of crumpled colored paper
   with hand-drawn white marker doodles (scribbles, crosses, squiggles,
   arrows) scattered around the subject, drifting slightly.
3. **Retro OS desktop**: a vintage 1990s computer desktop filling the frame:
   stacked application windows with title bars showing freeze-frames of the
   same person, a large pixel arrow cursor drifting, a teal desktop
   background.
4. **Halftone pop**: a flat color field covered in a coarse halftone dot
   pattern with hand-drawn white lightning bolts and burst marks radiating
   from behind the subject, pulsing gently.

**Axis 2: the palette.** One two-color pair per segment, never repeated on
adjacent segments: lime green + hot pink, royal blue + white, bright yellow +
black, mint green + charcoal, orange + cream, lavender + deep purple.

**Axis 3: sticker b-roll.** On segments whose `topics` are concrete, one
flat cartoon sticker graphic with a thick black outline pops in next to the
subject's head and holds (a pink brain for AI, a retro computer for tech, a
rising chart for growth). One sticker maximum per segment; skip it on
segments that already carry a punch-in.

## Energy moves

- **Punch-in**: on the word whose `emphasis` is strongest, write "exactly on
  the word X the framing punches in tighter on the person's face, a bold
  close-up, then eases back". Alternate across the video so it reads as cuts.
- **B-cam angle**: one segment per video can take "the framing sits slightly
  off-center at a subtle side angle, like a second podcast camera".
- Change the palette exactly when the look changes; the hard cut between
  segments IS the transition between segments. Within a packed segment, a
  background swap on a spoken word (using the next palette in the rotation) is
  a legitimate beat.

## Beat vocabulary (anchor these to words)

This is the loudest template; it should never hold one frame. Events to land on
exact spoken words inside a segment:

- **Kinetic type flood**: on the opening word, the background words bounce in
  row by row, or the caption words slam in one at a time on their own beats.
- **Punch-in**: on the emphasis word (see above).
- **Background swap**: on a phrase turn ("but", "so", "here's the thing"), hard
  cut the background to the next palette/motif in the rotation.
- **Sticker pop**: exactly on the word that names a concrete topic, the flat
  cartoon sticker snaps in beside the head and holds.
- **Caption lock**: on the segment's final strong word, the torn-paper band
  locks and the frame settles for the cut.

A packed 8-10s segment uses two or three of these; the emphasis word gets the
punch-in or the biggest slam.

## Caption treatment

The torn-paper band from the style block. Per-segment line:

```
The torn-paper caption band reads exactly "MOST VIDEOS LOSE YOU", spelled
exactly, every word correct, bold black condensed uppercase on white,
anchored in the lower third.
```

3-6 words. The band stays in the same screen position across every segment
(it anchors the eye through the cuts).

## Example beat timeline (appended after identity + style blocks)

Segment spoken line: "Your brain checks out after eight seconds. But you can
fix it fast." (9s, emphasis on "fix")

```
The segment runs this beat timeline over the spoken words:
- It opens on the word "Your" over a crumpled hot-pink paper sheet with
  hand-drawn white marker scribbles, crosses and arrows drifting slowly around
  the person; palette hot pink and white. Exactly on the word "brain" a flat
  cartoon sticker of a pink brain with a thick black outline snaps in beside
  the person's head and holds.
- Exactly on the word "But" the background hard-cuts to a lime-green field of
  giant bold condensed type scrolling sideways; palette lime green and black.
- Exactly on the word "fix" the framing punches in tighter on the person's
  face in a bold close-up, and the torn-paper caption band locks reading
  exactly "FIX IT FAST", spelled exactly, every word correct, bold black
  condensed uppercase on white, anchored in the lower third, then the frame
  settles. The person's voice and the room's sound continue naturally.
```

## Failure modes

- **The subject got stylized** (painted/cartooned): the identity block got
  crowded. Shorten the background description, keep "completely photoreal
  inside the sticker" adjacent to the person lines, re-roll.
- **No white outline / subject blends into background**: strengthen to
  "thick clean white die-cut sticker outline all the way around the person".
- **Caption misspelled or paraphrased**: re-roll the same prompt first;
  then shorten the caption to 3-4 words.
- **Background static and dead**: the drift line got trimmed. Restore "slow
  constant drift so it feels alive" and name what moves (rows of type
  scroll, doodles wobble, cursor drifts).
- **Two stickers or a sticker collage appeared**: say "exactly one sticker
  graphic" in the beat timeline.
- **The frame barely moves**: the beat timeline was written as one look. Rewrite
  it with 2-3 events anchored to exact words (type flood, punch-in, background
  swap, caption lock); pack the segment to 8-10s so there are words to land on.
