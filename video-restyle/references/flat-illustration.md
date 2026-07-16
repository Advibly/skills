# Template: flat-illustration

The premium editorial illustration look. The whole frame is repainted as a
clean flat vector-style illustration: the person becomes a warmly drawn flat
character OF THEMSELVES, and the background becomes an idyllic illustrated
scene: soft sky, rounded clouds, rolling hills, stylized foliage. Cheerful,
calm, trustworthy. The look of a brand explainer or a wellness app.

**Pick it when**: the content is friendly, educational, health, finance, or
brand-story shaped and the user wants approachable warmth rather than energy
or art-house texture. The daylight counterpart to watercolor-wash.

**Subject treatment: full repaint.** The person becomes a flat illustration
OF THE SAME person. Replace the identity block's opening with this variant:

```
Transform this clip into a flat illustrated version of the exact same
scene. The person becomes a clean flat-style illustrated character of
themselves: the same recognizable face, the same identity, the same
expressions, the same mouth movements and lip-sync, the same hairstyle and
clothing, matching the source frame for frame. Their spoken performance
continues unchanged. Foreground props they hold or touch stay present,
illustrated in the same style.
```

## Style block (verbatim in every segment prompt)

```
Style: a clean modern flat illustration in motion, like a premium editorial
or brand explainer. Smooth flat color shapes with soft two-tone shading, no
outlines or only minimal thin ones, gentle rounded forms, a subtle paper
grain over everything. The background is an idyllic illustrated scene with
a soft sky, rounded clouds and stylized landscape elements, all in
harmonious flat colors. Small elements drift very slowly so the scene feels
alive. The whole frame reads as one continuous living illustration, warm,
friendly and calm.
```

## Variation axes (rotate per segment)

**Axis 1: the scene.** Rotate so no two adjacent segments repeat.

1. **Rolling hills**: green rounded hills, a blue sky, drifting clouds,
   scattered stylized trees and plants.
2. **Sunset gradient**: a warm gradient sky from coral to mauve, a low sun,
   long soft hill silhouettes.
3. **Cozy interior**: a flat illustrated room, a window with sky beyond,
   a plant, soft wall color.
4. **Botanical close**: oversized stylized leaves and stems framing the
   subject from the edges, a plain soft field behind.

**Axis 2: the palette.** One harmonious set per segment, never repeated on
adjacent segments: sky blue + leaf green + cream, coral + mauve + sand,
teal + mustard + off-white, lavender + mint + warm grey.

**Axis 3: living accents.** One or two tiny scene elements move gently: a
bird gliding across, a cloud drifting, a leaf floating down, sun rays
breathing. Tied to the segment's `topics` when concrete (a small illustrated
coin for money, a leaf for health, an envelope for messages).

## Energy moves

Gentle by design. No impact frames; at most one soft punch-in per video on
the single strongest word. Its beats are scenic and still anchor to words:

- **Scene shift**: on a phrase turn, the scene crossfades to the next scene
  and palette in the rotation, like turning a picture-book page.
- **Soft punch-in**: on the video's strongest word only, the framing eases
  in slightly closer, then eases back.
- **Sky change**: on an emphasis word, the sky's color deepens or the sun
  rays breathe wider.

## Beat vocabulary (anchor these to words)

- **Scene settle**: on the opening word, the scene's elements drift into
  place (clouds slide in, hills settle).
- **Accent glide**: exactly on the word that names a topic, the accent
  element enters (the bird glides across, the leaf floats down).
- **Sky deepen**: on a mid-segment emphasis word, the sky change.
- **Calm hold**: on the final word, everything eases to a gentle hold for
  the cut.

A packed 8-10s segment carries two or three of these soft beats so the
illustration keeps breathing without ever feeling busy.

## Caption treatment

Captions are optional and on by default in a soft form:

```
A short caption reads exactly "SMALL STEPS COMPOUND", spelled exactly,
every word correct, in clean rounded sans-serif letters in the palette's
darkest color, sitting on a soft rounded pill in the lower third.
```

3-6 words, sentence-case or uppercase to match the user's preference.

## Example beat timeline

Segment spoken line: "Saving feels impossible until you make it automatic.
Then it just happens." (9s, emphasis on "automatic")

```
The segment runs this beat timeline over the spoken words:
- It opens on the word "Saving" with rounded clouds sliding gently into a
  sky-blue sky over cream rolling hills, the illustrated person centered,
  a subtle paper grain over the whole frame.
- Exactly on the word "automatic" a small illustrated coin arcs gently
  across the background and the sky deepens one shade, and the caption pill
  reads exactly "MAKE IT AUTOMATIC", spelled exactly, every word correct,
  clean rounded sans-serif in the lower third.
- On the words "just happens" everything eases to a gentle hold for the
  cut. The person's voice and the room's sound continue naturally.
```

## Failure modes

- **The face went generic** (an illustrated stranger): reinforce "the same
  recognizable face, an illustrated character of this exact person" and
  re-roll. Simplify the scene if it persists.
- **Output looks like a photo with a posterize filter**: the flat language
  got trimmed. Restore "smooth flat color shapes", "soft two-tone shading",
  "subtle paper grain".
- **The scene turned busy or cluttered**: hold to one or two accents, keep
  "warm, friendly and calm" at the end of the style block.
- **Lip-sync softened**: add "the mouth shapes stay crisp and readable in
  the flat style" and re-roll.
- **The illustration sits static**: the beats were dropped. Restore two or
  three soft beats (scene settle, accent glide, sky deepen) anchored to
  exact words.
- **Palette drifted or oversaturated**: hold each segment to its three
  named colors, restate "harmonious flat colors".
