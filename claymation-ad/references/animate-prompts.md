# Animation prompts: the claymation motion

Use this after every storyboard still is approved. Each still becomes one clip; the clips get
stitched into the ad. Read `cast-and-story.md` for the arc and `storyboard-prompts.md` for how
the stills were made.

Every clip is made with `advibly_generate_video`, passing the approved still as
`start_image_url`. Use `gemini-omni-flash` as the default (`aspect_ratio: "9:16"`,
`duration: 10`, `brand_id` required). Use `seedance-2.0` only as the secondary fallback after two
failed Gemini attempts or for a necessary end-frame transformation, and obtain approval first.
When the user explicitly names Gemini or Seedance, use that model for every clip and do not switch.

## Platform basics (what matters for claymation)

- **Image input:** the approved still as `start_image_url`. Do NOT also pass
  `reference_image_urls`; that switches video mode and the still stops being the opening frame.
- **Duration:** 10 seconds for every clip unless the user explicitly requests another duration.
- **Model-specific fields:** omit `mode`, `resolution`, and `end_image_url` for Gemini. With
  Seedance use `mode: "pro"`; use `end_image_url` only if the user selected Seedance or approved
  its fallback for beat 7.
- **Audio:** SFX only. No `Narrator:` line, no spoken words. The voiceover is generated
  separately and mixed on top (see `audio-and-gotchas.md`).
- **Prompt length:** 100 to 260 words, under the 2000-char cap. Structure: Subject + Action +
  Camera + Style + Ambient + Constraints.
- **Forbidden Seedance words** (only when Seedance is selected): `cinematic`, `professional`,
  `stunning`, `8k`, `studio`, `perfect`. Substitute "stop-motion claymation film aesthetic",
  "polished hand-sculpted", "high fidelity", "evenly hand-painted".
- **Smooth motion, never judder.** Do not ask a video model for "stop-motion judder"; it breaks the
  aesthetic. If the user wants judder, it is a clip-level step-frame pass before composition (see
  `audio-and-gotchas.md`).
- **Beat-7 end-frame reveal:** only Seedance takes `end_image_url`. For its explicitly selected
  or approved fallback route, pass the "before" still as `start_image_url` and the lightly-
  improved "after" still as `end_image_url`. With Gemini, perform the subtle change within the
  10-second start-frame clip.

## Universal motion prompt structure (six blocks)

```
[BEAT INTRO]      <- what this clip is: "image-to-video animation of the scene in the start frame"
[SUBJECT LOCK]    <- the cast-sheet fragment, verbatim
[ACTION]          <- one primary motion + small secondary motions, with degree adverbs and timings
[CAMERA]          <- framing + a small camera move (often locked with a subtle handheld macro breath)
[STYLE ANCHOR]    <- "stop-motion claymation film aesthetic" + Aardman tone words
[AMBIENT]         <- room tone + clay foley only. NO narrator or dialogue.
[CONSTRAINTS]     <- preserve-clay-texture + identity + negatives
```

## Reusable SUBJECT LOCK fragments

Lift these verbatim into the SUBJECT LOCK of every relevant beat. Same phrasing every time.

```
PROTAGONIST (example):
"Diane, a woman in her late 50s with shoulder-length terracotta-brown wavy plasticine hair in
distinct ribbon-strands, matte clay skin with visible thumbprint impressions and deep sculpted
laugh lines, warm brown matte clay eyes set into deep sockets, sculpted brow furrow. She wears a
cream chunky knit cardigan with visible wool weave over a rust-red blouse, dark wool trousers,
brown leather slippers."

SUPPORTING (example):
"Margaret, a woman in her 60s with silver curly plasticine hair (carved strand grooves), round
wire glasses, a sage-green cable-knit sweater with visible wool weave, matte clay skin."

PRIMARY SETTING (example):
"A sunlit miniature claymation kitchen: green-painted wooden cabinets, red gingham tablecloth,
hand-thrown ceramic cups, copper kettle on a small stove, potted herbs on the windowsill, warm
tungsten light from a window on camera-left."

PRODUCT PROP (example):
"a small dusty-purple cylindrical clay bottle labeled 'refirm' in hand-painted cream lettering,
matte hand-applied paint with subtle brush texture."
```

## The anti-smoothing constraint block (every clip)

The video model's tendency to smooth sculpted clay into a glossy 3D render mid-clip is the #1
failure mode. Every prompt ends with a variant of:

```
Preserve all clay texture from the start frame: matte plasticine surfaces, visible thumbprint
impressions and tool marks, real knit-fabric wool weave, matte eyes with a single soft
highlight. The character stays visually identical to the start frame (same hair strands, same
skin, same outfit, same eye sockets). No live-action, no photorealistic skin smoothing, no
Pixar wet-eye sheen, no 3D ray-traced materials, no morphing, no extra fingers, no warped
label, no on-screen text, no subtitles, no captions.
```

## Per-beat formulas

### Beat 1 - setup (target 10s)

Subject: {PROTAGONIST_FRAGMENT} in {PRIMARY_SETTING_FRAGMENT}. Action: she slowly pours tea
from the ceramic kettle (0 to 3s), sets it down with a slight hand turn (3s to mid), tilts her
head toward the window, a slow single blink (mid to end); subtle steam micro-motion. Camera:
locked medium-wide, very subtle handheld macro breathing, no zoom or pan. Ambient: faint kettle
pour, soft kitchen room tone, distant birdsong. Constraints block.

### Beat 2 - inciting moment (target 10s)

Subject: {PROTAGONIST_FRAGMENT} in tight close-up at the mirror. Action: her clay fingertip
lifts and gently touches the area with the carved lines (0 to 2s), her sculpted brow furrows a
touch, mouth opens slightly in quiet alarm, one slow blink, holds very still (2s to end).
Camera: locked tight close-up, subtle handheld micro-drift, shallow macro DoF. Ambient: soft
bathroom room tone, faint plumbing hum. Constraints block (add: "the sculpted lip lines stay in
the same position").

### Beat 3 - two-character scene (target 10s)

Subject: {PROTAGONIST_FRAGMENT} on the right and {SUPPORTING_FRAGMENT} on the left at a wooden
cafe table. Action: the supporting character tilts her head and her clay mouth opens to speak,
painted lips forming rough shapes (rough lip sync is fine, no exact sync), eyebrows raising on
emphasis (0 to 3s); the protagonist's eyes glance down, self-conscious (3s to mid); she lifts
her teacup partway, hesitates, sets it back (mid to end); slight steam from both cups. Camera:
locked medium two-shot, subtle handheld breath. Ambient: soft cafe tone, faint cup-on-saucer
clink, distant muffled conversation. Constraints block (both characters unchanged). No spoken
words in the clip; the exchange is carried by the external narration.

### Beat 4 - quiet despair (target 10s)

Subject: {PROTAGONIST_FRAGMENT} alone at a full-length mirror in a dim living room. Action: she
slowly lifts a clay hand to her cheek (0 to 2s), eyes lower a fraction, a very slow blink (2s to
mid), lets her hand fall back slowly, gaze still on her reflection (mid to end); the mirror
reflection mirrors her motion exactly. Camera: locked medium-wide, subtle handheld macro breath.
Ambient: very soft room tone, the faintest distant clock tick. Constraints block (add: "the
mirror reflection matches her pose accurately throughout").

### Beat 5 - clay infographic (target 10s)

Subject: the sculpted clay line graph on its hand-carved frame on a cream clay wall. Action: the
chart sits still as soft light shifts across it (0 to 2s), a small sculpted clay arrow indicator
traces along the graph left to right in measured even motion (2s to mid), the arrow rests at the
drop and the label tag settles with a small final motion (mid to end). Camera: locked head-on,
subtle handheld macro breath, a slight 5% slow zoom toward the drop. Ambient: a quiet low hum,
the silence of a research beat. Constraints block (chart and frame unchanged; add: "no
digital-text overlay, no smooth animated graphics, no extra labels, no on-screen text beyond
what is already sculpted into the still").

### Beat 6 - discovery (target 10s)

Subject: {PRODUCT_PROP_FRAGMENT} on a wooden table, {PROTAGONIST_FRAGMENT}'s clay hand entering
from camera-right. Action: her fingers move slowly toward the bottle, hovering (0 to 2s), her
hand gently picks it up, fingers curling around it, the bottle lifts (2s to mid), she slowly
rotates it so the hand-painted label faces camera more cleanly, the light catching the matte
paint (mid to end). Camera: locked medium, subtle handheld breath, a slight 3% slow dolly-in.
Ambient: soft kitchen tone, faint distant kettle whistle. Constraints block (add: "the product
bottle stays visually unchanged, same dusty-purple matte paint, same hand-painted label text
legible, no smooth digital plastic look, no warped or shifting label text").

### Beat 7 - transformation (target 10s)

Subject: {PROTAGONIST_FRAGMENT} in her bathroom holding the {PRODUCT_PROP_FRAGMENT}, mirror
behind. Action: she uncaps the bottle and takes a small amount onto her fingertip in slow
deliberate motion (0 to 3s), gently applies it with measured strokes, watching her reflection
(3s to mid), lowers her hand and her expression slowly warms into a small pleased smile (mid to
end); the carved lines above her lip are visibly slightly smoother than the clip start while
everything else about her identity stays exactly the same. Camera: locked medium close-up,
subtle handheld breath, a slight 3% slow dolly-in on her face at the end. Ambient: soft bathroom
tone, a gentle cap unscrewing. Constraints block (add: "only the specific upper-lip area may
appear subtly smoother by the end, no other part of the face changes; the bottle prop stays
identical to Beat 6"). **End-frame route:** if using `end_image_url`, the "after" still is the
last frame and this action interpolates toward it.

### Beat 8 - resolution + CTA (target 10s)

Subject: {PROTAGONIST_FRAGMENT} in {PRIMARY_SETTING_FRAGMENT} holding the {PRODUCT_PROP_FRAGMENT}
at chest height, facing camera. Action: she gives a small warm gentle smile, sculpted laugh
lines working with it, her eyes meeting the camera and brightening softly (0 to 2s), raises the
bottle a touch closer, hand turning so the label reads cleanly (2s to mid), her smile widens
into a small satisfied grin and she holds the pose, still and warm, ready for a caption on the
lower third (mid to end). Camera: locked medium, subtle handheld breath, no zoom or pan.
Ambient: warm kitchen tone, faint birdsong. Constraints block (protagonist identical to Beat 7,
bottle identical to Beat 6; add: "the lower third stays visually clean and uncluttered for a
post-production caption overlay").

## Cross-clip continuity rules

1. **Each clip's `start_image_url` is that beat's approved still.** Do not chain by using an
   animated end frame as the next beat's anchor; drift compounds. Anchor motion on the stills.
2. **Lift the SUBJECT LOCK fragments verbatim** into every beat. Do not paraphrase the
   protagonist description.
3. **Keep the STYLE block consistent:** "stop-motion claymation film aesthetic" everywhere,
   never `cinematic`, never `Pixar`, never `3D rendered`.
4. **Use `duration: 10` for every clip.** Write each narration line to fit its 10-second window
   so no clip rides silent.
5. Once all stills are approved, the clips can render in parallel if the client fires several
   calls at once; each returns `status: pending` with a `generation_id`, collect finished URLs
   with `advibly_get_generation` (`wait: true`).

## Per-clip QA (claymation-specific)

Watch the full clip and verify:

- [ ] **Clay texture preserved end-to-end** (the smoothing tendency is the #1 risk): thumbprint
      and tool marks stay visible in close-ups.
- [ ] **Knit fabric stays woven wool**, not painted-on stripes.
- [ ] **Matte eyes**, no Pixar wet-eye sheen developing mid-clip.
- [ ] **Identity holds** from the input still to the last frame: same face proportions, hair
      strands, outfit.
- [ ] **Product label paint stays hand-applied** looking, no digital crispness leaking in.
- [ ] **Beat-7 improvement is localized** (only the upper-lip area, not the whole face).
- [ ] **No burned-in text or subtitles** appeared.
- [ ] **Mirror reflections move correctly** on beats 2, 4, 7.
- [ ] **Beat-3 two-shot lip sync** is plausible (rough match is fine).

If clay texture flattens or identity drifts, regenerate with a tightened MATERIAL DETAIL block
and an explicit "preserve all clay texture from the start frame" constraint, or re-roll the
still on `nano-banana-2` and re-animate. 2-retry cap per beat.
