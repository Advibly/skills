# Story and beats (the STORY layer)

The style-agnostic half of the skill: how to invent an original concept, structure it into a beat
map, and set the shot cadence. Read this before writing any beat map (Phase 3). The look comes
from the chosen style file; the story comes from you.

## You invent the story. Always.

The style files are look references only. Nothing about the example content inside a style file is a template.
For every brief, design a **fresh concept** that fits this brand and topic:

- **The idea / metaphor**: the one memorable way to dramatize the topic (a firewall as a castle,
  compound interest as a snowball, a cluttered inbox as an avalanche). This is the creative act;
  spend real thought here. It must be original to the brief, not borrowed from a style file's example.
- **The cast**: if the concept wants a character or mascot, design your own using the style's
  construction logic (its `character_design` field tells you how the style builds a character, not
  which character to use). Many explainers need no character at all.
- **The script**: the narration, written as one continuous persuasive or teaching read.
- **The on-screen words**: any baked headline copy, invented for this ad.

Two explainers in the same style for two different brands should look alike and share nothing
else. If you find yourself reaching for a style file's example subject, stop and invent.

## Narrative arcs (pick one that fits the topic)

- **how_it_works** (default for product/feature explainers): hook the outcome, then walk the
  mechanism step by step, then land on the benefit. The most common explainer shape.
- **pas** (problem, agitate, solve): name a pain, twist the knife, present the product as relief.
  For hard-sell ads.
- **bab** (before, after, bridge): the world without, the world with, the product as the bridge.
- **aida** (attention, interest, desire, action): classic direct response.
- **timeline**: a history or evolution told in chronological beats. Good for origin stories.
- **myth_buster**: state a common belief, dismantle it, replace it with the truth. Great for
  "not all X is equal" claims.
- **man_in_hole**: a subject falls into trouble and climbs out. For transformation stories.

Match the arc to the claim, not to the style. Any arc can run in any style.

## Hook patterns (beat 1 must land in under 3 seconds)

Never spend beat 1 on setup. Open on the payoff tension. Pick a pattern:

- **surprising_stat**: a number that stops the scroll.
- **mistake_callout**: "you're doing X wrong."
- **bold_claim**: a strong assertion the rest of the ad earns.
- **question_hook**: a question the brief makes the viewer want answered; invent your own for this topic.
- **in_medias_res**: drop straight into the dramatic middle, then explain.

## Beat and shot structure

- **A beat is a story unit** (one idea, one narration line). **A shot is a single clip.** A beat
  is usually 1 or 2 shots.
- **Recommend 4 to 6 seconds for every generated shot.** Omni Flash's minimum is 4 seconds, so
  no shot in the beat map may be shorter. Read the style file's `transitions_and_cuts.cut_style`
  to shape what happens inside that window:
  - Fast-cut styles (Pixel Art, Textured Gouache, Mixed-Media) pack multiple action beats,
    snap transitions, or reframes inside a 4 to 6-second generated clip.
  - Held styles (Papercraft, Low-Poly, Cinematic 2D Print) sustain one clear move for 4 to 6
    seconds.
  - Whiteboard is special: it **pans across one board** to new drawings instead of hard-cutting.
    Model this as spatial reveals on a continuous surface, not discrete cuts.
- **The stitcher takes at most 12 clips.** At 4 to 6 seconds each that is 48 to 72 seconds. Plan
  shot count to fit both the target length and the 12-clip ceiling. Over 12 shots, stitch in two
  passes.
- **Two-shot beats** (a wide establishing shot carrying the headline, then a detail cut-in) let
  the narration span both while the visual cuts mid-sentence. Use them in the held styles.

## Camera and element motion (write per shot)

Draw the moves from the chosen style's vocabulary, never a generic set:

- **Camera move** comes from the style's `camera_language` (Low-Poly and Papercraft glide and
  push in; Pixel and Cinematic 2D Print cut on static frames; the stop-motion styles lock off with
  a slow push). **No two adjacent beats share a camera move.** Reserve `static` for the payoff.
- **Element motion** comes from the style's `motion.signature_moves`, written fresh per shot to
  fit that scene (rich, several things moving). This is where the energy lives.
- **Signature punches** (a pixel slash with screen shake, a papercraft tunnel reveal, a whiteboard
  draw-on, a clay impact splash) belong on the beats that earn them (hook, product reveal, payoff),
  not every shot.

## Narration

- The beats' narration lines together are the script. Write them as one continuous read.
- Time each line to the style's `voiceover.pace_words_per_sec` inside that beat's duration (most
  styles ~2.5 wps, ~15 words in a 6s beat; Pixel and Mixed-Media run faster at ~3.5 wps; Low-Poly
  slower at ~1.5 to 2 wps). A line that overruns its beat gets tightened, never time-stretched.
- Keep it persuasive or teaching, in the brand's voice, in the user's language.

## The beat map JSON (deliver this for approval)

Editable field by field. Proceed only on an explicit yes.

```json
{
  "project": "acme-fridge-explainer",
  "topic": "how our fridge keeps food fresh 3x longer",
  "brand_id": "<id>",
  "product": "Acme SmartFridge",
  "product_photo_url": "<url or null>",
  "style": "<chosen style file slug, e.g. low-poly>",
  "aspect": "16:9",
  "language": "en",
  "arc": "how_it_works",
  "voice": "<eve|ara|rex|sal|leo, from the style's suggested voice>",
  "music": "<one-line brief from the style's audio_recipe.music_prompt>",
  "snap": true,
  "beats": [
    {
      "id": 1,
      "role": "hook",
      "hook": "question_hook",
      "narration": "Why does your produce wilt in days?",
      "shots": [
        {
          "id": "1a",
          "dur": 4,
          "shot_size": "WIDE",
          "camera_move": "slow_push_in",
          "scene": "<your invented scene, described in the style's medium>",
          "element_motion": "<from the style's signature_moves, written for this scene>",
          "headline": "<baked in-world text, only if the style bakes headlines; else null>",
          "product": false
        }
      ]
    }
  ]
}
```

`snap` is `true` when the chosen style's cadence is on-twos / stop-motion (drives the step-frame
pass), `false` for the smooth styles. `voice` and `music` seed Phase 6. `headline` is non-null
only on shots that bake in-world text, and only for styles whose `typography` bakes headlines.

## Anti-monotony checklist (before you show the map)

- Beat 1 hooks in under 3 seconds, no setup.
- The idea is original to this brief, not a style file's example subject.
- Every generated shot is 4 to 6 seconds by default; fast styles use internal action and cuts
  rather than sub-4-second model calls.
- No two adjacent beats share a camera move; `static` is saved for the payoff.
- Element motion is written per shot, rich, from the style's vocabulary.
- The narration reads as one continuous script and times to the style's words-per-second.
- Baked headlines are marked only where the style supports them; everything else relies on the VO
  and optional captions.
