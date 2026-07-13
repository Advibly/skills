# Animation prompts: the stickman motion (the LOOK layer for motion)

Use after every storyboard still is approved. Each still becomes one clip; the clips get stitched.
This holds constant no matter what story you invented. Every clip is `advibly_generate_video` with
the approved still as `start_image_url`, `gemini-omni-flash` by default (`aspect_ratio: "9:16"`,
`duration` ~5 to 10s to fit the beat, `brand_id` required). Use `seedance-2.0` only as the secondary
fallback after two failed Gemini attempts or for a controlled before/after transformation, with
approval. If the user names a model, use it for every clip.

## Platform basics (what matters for stickman)

- **Image input:** the approved still as `start_image_url`. Do NOT also pass `reference_image_urls`;
  that switches video mode and the still stops being the opening frame.
- **Duration:** size to the beat (~5 to 10s); Gemini caps at 10s. Confirm supported values against
  the loaded tool schema.
- **Model-specific fields:** omit `mode`, `resolution`, `end_image_url` for Gemini. With Seedance use
  `mode: "pro"`; use `end_image_url` only for an approved before/after transformation beat.
- **Audio:** SFX only. No `Narrator:` line, no spoken words. The voiceover is external (see
  `audio-and-gotchas.md`).
- **Prompt length:** ~90 to 240 words, under the 2000-char cap.
- **Do not ask the model for "12fps", "choppy", "on twos", or "stop-motion".** Generate smooth; the
  snappy limited-animation feel is added with the clip-level step-frame pass before composition.
- **Static camera is the default** (flat 2D comic frames): locked, no zoom or pan. An intentional
  impact shake, a whip, or a quick push is fine when a specific beat calls for it.
- **Before/after transformation route:** only Seedance takes `end_image_url` (start = "before" still,
  end = "after" still). With Gemini, perform the whole change within the clip.

## Universal motion prompt structure (six blocks, every beat)

```
[CLIP INTRO]      <- "image-to-video animation of the flat 2D scene in the start frame"
[SUBJECT LOCK]    <- the character / recurring-element wording, verbatim from your cast sheet
[ACTION]          <- one primary motion + small secondary motions, snappy pose-to-pose feel
[CAMERA]          <- locked static 2D frame by default (intentional shake/whip only when a beat needs it)
[STYLE ANCHOR]    <- "flat 2D vector cartoon animation, clean flat linework"
[AMBIENT]         <- comic foley only. NO narrator or dialogue.
[CONSTRAINTS]     <- the anti-3D / anti-flicker block below
```

## SUBJECT LOCK

Lift your locked character and recurring-element descriptions verbatim from the cast sheet into the
SUBJECT LOCK of every relevant beat, word for word. Whatever you wrote for the protagonist, a
mascot, an anthropomorphized device, or the product prop appears identically in every clip that
contains it. Consistency of the wording is what holds the silhouettes across clips.

## The anti-3D / anti-flicker constraint block (paste into every clip)

The model's tendency to add shading and volume and to morph or boil the linework is the #1 failure
mode. Every prompt ends with a variant of:

```
Preserve the completely flat 2D vector look from the start frame: uniform flat black outlines of
even weight, a pure white background, solid flat fills, no shading anywhere. The characters and any
recurring elements stay exactly the proportions and silhouette of the start frame. No 3D shading, no
added shadows, no gradients (except the intentional {BRAND_COLOR} glow), no volume or depth, no
rendered lighting, no line flicker, wobble, or boiling of the outlines, no morphing, no realistic
anatomy growing in, no texture, no motion blur except described speed lines, no on-screen text, no
subtitles, no captions.
```

## Generic per-shot motion scaffold (fill for each of YOUR beats)

```
Image-to-video animation of the flat 2D scene in the start frame. {snappy limited-animation feel, OR
gentle/calm limited animation if the beat is quiet}.

Action: {one primary motion for this beat of your story, plus one or two small secondary motions,
described with degree adverbs and rough timing}. {any comic VFX behavior: a glow pulsing, lightning
flickering, a burst popping in, a puff drifting, speed lines}.

Camera: {locked static 2D frame, no move — OR an intentional impact shake / whip if this beat needs
it}.

Style: flat 2D vector cartoon animation, clean flat linework.

Ambient (SFX only): {the comic foley for this beat — zap, pop, whoosh, thunder, gulp, chime, a
comic explosion, room tone}. No voiceover, no spoken words, no music with lyrics.

{ANTI-3D / ANTI-FLICKER CONSTRAINT BLOCK}
```

Principles: the still owns composition, so the prompt describes **motion, not layout**. One clear
primary action per beat. Keep motions small and snappy; stick figures move pose-to-pose, not with
fluid arcs.

## Worked example (ILLUSTRATION ONLY, do not reuse)

From the energy-drink reference, the "transformation" beat (the turn), showing how the scaffold gets
filled for a big moment. Invent your own beats; this only demonstrates the structure.

```
Image-to-video animation of the flat 2D scene in the start frame. Snappy limited animation.

Action: the stick figure tips the can up and drinks (0 to 3s); a sudden white flash fills the frame
(~4s); its whole outline floods {BRAND_COLOR} with internal energy squiggles and large {BRAND_COLOR}
lightning bolts erupt outward as it snaps into a power pose (4 to 7s); the gray mood cloud rockets
off the top of the frame trailing debris (~6s); it holds the glowing pose (7 to end).

Camera: locked static 2D frame with a hard screen shake on the flash and lightning (~4 to 6s), then
settle.

Style: flat 2D vector cartoon animation, clean flat linework.

Ambient (SFX only): a can pop and gulp, a thunder blast on the flash, an electric surge. No
voiceover, no spoken words, no music with lyrics.

{ANTI-3D / ANTI-FLICKER CONSTRAINT BLOCK, plus: the color flood and lightning are flat 2D, not a 3D
effect; the cloud keeps its flat gray silhouette as it flies off}
```

A calmer concept fills the same scaffold with gentle motion, a soft glow instead of lightning, and
no shake. The structure is fixed; the content is yours.

## Cross-clip continuity rules

1. **Each clip's `start_image_url` is that beat's approved still.** Do not chain animated end frames
   as the next beat's anchor; drift compounds. Anchor motion on the stills.
2. **Lift the SUBJECT LOCK wording verbatim** into every beat.
3. **Keep the STYLE anchor consistent:** "flat 2D vector cartoon animation, clean flat linework"
   everywhere; never `cinematic`, `3D`, or `rendered`.
4. **Size each clip's duration to its narration line** so no clip rides silent.
5. Once stills are approved, clips can render in parallel; each returns `status: pending` with a
   `generation_id`, collect finished URLs with `advibly_get_generation` (`wait: true`).

## Per-clip QA (stickman-specific)

Watch the full clip and verify:

- [ ] **Flat linework preserved end-to-end**: no cel-shading, gradients, or drop shadows creep in.
- [ ] **No line flicker or boiling**: outlines stay clean and steady.
- [ ] **Pure white background** holds; it does not drift gray or textured.
- [ ] **Characters and recurring silhouettes hold** from the input still to the last frame; no
      morphing, no realistic anatomy growing in.
- [ ] **{BRAND_COLOR} stays only on the product and its energy**; gray only on a problem element.
- [ ] **A transformation reads as a flat color flood + comic VFX**, not a 3D burst.
- [ ] **No burned-in text or subtitles** appeared (except intended baked text).
- [ ] **Camera behaves**: static unless a beat intentionally shakes or whips.

If shading or 3D creeps in, the lines boil, or a silhouette morphs, regenerate with a tightened
CONSTRAINTS block, or re-roll the still on `nano-banana-2` and re-animate. 2-retry cap per beat. The
on-twos snap pass in the final phase also masks minor line wobble; re-roll a clip whose lines visibly
boil.
