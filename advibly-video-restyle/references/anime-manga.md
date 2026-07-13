# Template: anime-manga

The clip becomes a cel-shaded anime episode: clean line art, flat color
fills with hard two-tone shading, expressive anime eyes on the same face,
manga devices (speed lines, screentone bursts, impact frames) spending the
energy. Loud in a completely different way than podcast-pop: the drama is
drawn, not cut.

**Pick it when**: the audience lives on anime-literate internet (gaming,
crypto, dev tools, streetwear, energy drinks) or the user literally asks for
anime, manga, or cartoon.

**Subject treatment: full repaint.** The person becomes an anime character
version of THE SAME person. Replace the identity block's opening with:

```
Transform this clip into a 2D anime. The person becomes an anime character
version of themselves: the same recognizable facial structure and identity,
the same expressions, the same mouth movements and lip-sync, the same
hairstyle and hair color, the same clothing, matching the source frame for
frame. Their spoken performance continues unchanged. Foreground props they
hold or touch stay present, drawn in the same style.
```

## Style block (verbatim in every segment prompt)

```
Style: modern 2D anime, crisp dark line art, flat cel-shaded color fills
with hard two-tone shadows, subtle rim light on the hair and shoulders,
large expressive anime eyes with catchlights, clean vector-flat background
art. Manga graphic devices live in the background: radiating speed lines,
screentone dot gradients, sharp impact flashes on strong words. Colors are
saturated with strong value contrast. The whole frame reads as one
consistent anime episode, broadcast quality.
```

## Variation axes (rotate per segment)

**Axis 1: the backdrop.** Rotate: a flat color field with radiating manga
speed lines converging behind the head, a soft-gradient anime sky with
drifting clouds, an interior redrawn as flat anime background art, a
screentone halftone field with diagonal energy, a dark dramatic field with a
single rim-light glow.

**Axis 2: the palette.** One anime-grade scheme per segment: sunset orange +
deep teal, cherry pink + navy, acid green + ink black, sky blue + warm
cream, crimson + slate.

**Axis 3: manga devices.** Pick per segment from: floating sound-effect
lettering, a small chibi icon of the topic hovering beside the head, a
screentone burst opening behind the subject, sweat-drop or spark accents on
a joke. One device per segment.

## Energy moves

- **Impact frame**: on the strongest emphasis word: "exactly on the word X the
  background snaps to a white-and-black radial impact flash for a beat, then
  returns". Once per video, on the loudest word.
- **Zoom drama**: one segment may take "the framing pushes in slowly toward
  the eyes, anime confrontation staging".
- The hard cut between segments plus a backdrop change reads as a scene cut;
  an in-segment backdrop swap on a spoken word is a legitimate beat.

## Beat vocabulary (anchor these to words)

Anime spends its energy in drawn events landing on exact spoken words:

- **Speed-line burst**: on the opening word, manga speed lines snap in radiating
  from behind the head.
- **Impact flash**: on the emphasis word (see above), the loudest beat.
- **Backdrop swap**: on a phrase turn, hard cut the backdrop to the next scheme
  in the rotation.
- **Chibi / device pop**: exactly on the word that names a topic, a small chibi
  icon or sound-effect lettering pops in beside the shoulder.
- **Subtitle snap**: on the strong word, the subtitle line snaps in bottom-center
  and holds.

A packed 8-10s segment uses two or three; the emphasis word gets the impact
flash or the push-in.

## Caption treatment

```
Bold subtitle text reads exactly "THIS CHANGES EVERYTHING", spelled exactly,
every word correct, thick white uppercase lettering with a heavy black
outline, anchored bottom-center like anime subtitles.
```

## Example beat timeline

Segment spoken line: "We built this in a weekend and shipped it today." (8s,
emphasis on "shipped")

```
The segment runs this beat timeline over the spoken words:
- It opens on the word "We" over a flat acid-green field with dark manga speed
  lines radiating from behind the person's head, drifting slowly; palette acid
  green and ink black. Exactly on the word "weekend" a small chibi rocket icon
  pops in beside their shoulder.
- Exactly on the word "shipped" the background snaps to a white-and-black radial
  impact flash for a beat, then returns, and bold subtitle text snaps in reading
  exactly "SHIP IT TODAY", spelled exactly, every word correct, thick white
  uppercase with a heavy black outline, bottom-center, holding through the end.
  The person's voice continues naturally.
```

## Failure modes

- **Face drifts to a generic anime character**: tighten "the same
  recognizable facial structure, this exact person as an anime character"
  and reduce backdrop complexity. Skin tone and hairstyle anchors help:
  name them explicitly.
- **3D or semi-real render instead of 2D**: open the style block's first
  line with "flat 2D anime, hand-drawn look" and re-roll.
- **Lip-sync flaps generically**: add "the mouth animates precisely with the
  spoken words".
- **Speed lines everywhere, every segment**: devices are per-segment picks,
  one each. Rewrite the beat timelines.
- **Text came out pseudo-Japanese**: captions must carry the exact quoted
  English text plus the spelling line; re-roll.
- **Nothing animates on the words**: the beat timeline was written as one look.
  Restore 2-3 drawn beats (speed-line burst, impact flash, subtitle snap)
  anchored to exact words; pack the segment to 8-10s.
