---
name: advibly-pixar-style-ad
description: >
  Turn a brand and product into a finished vertical feature-film-style 3D animated ad on the
  Advibly MCP. Lock an original expressive cast and 4-beat story, create sequential gpt-image-2
  storyboard stills, animate 8-second shots with Gemini Omni Flash by default or Seedance 2.0
  as fallback, then compose narration, music, and SFX. Trigger for "Pixar-style ad", "Pixar
  cartoon ad", "3D animated product ad", "animated ad with a talking problem", a warm big-eyed
  feature-animation look, or a matching reference. If the user explicitly names Gemini Omni
  Flash or Seedance 2.0, use that model.
---

# Advibly Pixar-Style Ad

Create a short product story with an original, warm feature-film 3D animation look. The user may
call it "Pixar-style"; preserve the requested qualities, but describe the work in generation
prompts as **expressive feature-film 3D animation** rather than trying to duplicate a studio's
exact visual signature. Build original characters and mascots. Never introduce a copyrighted
character, existing franchise setting, or studio logo.

Speak in the user's language. No em dashes anywhere in output; use periods or line breaks. Keep
on-screen labels and captions free of emoji unless requested.

## The core idea (read this first)

The look, the motion, and the voice are **three separate steps**:

1. **The look is born in the IMAGE step.** Each beat is a finished feature-3D *still* made by the
   image model. All the DNA (large expressive eyes with catchlights, tactile materials, warm
   volumetric light, creamy depth of field, character identity, product packaging) lives in that
   still. A weak still cannot be repaired downstream. Re-roll cheap here rather than paying to
   animate it.
2. **The motion is added after.** The video model animates the still while preserving its look.
   8-second clips are the default: a 10-second beat goes too still and starts to morph, so hold
   each shot to 8 seconds and let one clear action carry it.
3. **The voice is always external.** Clips ship SFX-only (room tone, soft characterful sounds) and
   the warm narrator is generated separately with `advibly_generate_voiceover`, scored with
   `advibly_generate_music`, and assembled on top with `advibly_render_composition`. Baking a narrator into the video model
   forces lip-sync compromises and gives a different voice every beat. One voiceover render is one
   consistent voice across all four beats.

## Read these references before generating

- Read `references/cast-and-story.md` before creating the cast sheet or beat map. It contains the
  4-beat arc, claim-safe mechanism choices, 8-second timing, and the approval JSON.
- Read `references/storyboard-prompts.md` before every still. It contains the style lock,
  beat-specific prompt formulas, reference chain, and still QA.
- Read `references/animate-prompts.md` before every video. It contains Gemini and Seedance prompt
  formulas, the model-selection rules, the 8-second SFX-only motion cadence, and clip QA.
- Read `references/audio-and-gotchas.md` before the audio pass or when debugging a weak render. It
  contains the voiceover voice map, the music brief, the final composition contract, captions,
  and the model / failure-mode gotchas.

## Non-negotiable workflow

1. **Lock the plan before spending credits.** Create a cast-and-continuity sheet and a 4-beat
   script. Show the editable beat map and wait for approval before generating images.
2. **Generate stills before video.** The still owns the original 3D-animation look, identity,
   product packaging, and composition. A video prompt cannot repair a weak still.
3. **Keep character continuity sequential.** Generate the initial hero/protagonist still first.
   Reuse its approved URL and exact cast wording for every later beat with that character. Do not
   generate character-carrying frames in parallel.
4. **Do not animate an unapproved storyboard.** Present the stills in beat order with narration
   and regenerate individual misses before the video pass.
5. **Honor a model request.** When the user explicitly names `gemini-omni-flash` or
   `seedance-2.0`, use it for every clip and do not switch models automatically. Otherwise use
   Gemini Omni Flash by default and treat Seedance as the secondary fallback described below.
6. **8 seconds per shot.** Use `duration: 8` for every clip (Gemini and Seedance both support it).
   Do not use 10 seconds: the beats go static and start to morph. Only change duration if the user
   explicitly overrides it.
7. **Clips are SFX-only; the narrator is external.** Do not ask the video model for narration,
   dialogue, subtitles, captions, or music. Generate one voiceover and one music bed after the
   picture edit and mix them on top.

## Tool discovery and asset rules

Tools are deferred. Load the exact Advibly tool schemas with tool search before the first call
each session (search "advibly generate image", "advibly generate video", "advibly generate
voiceover", "advibly generate music", "advibly stitch videos", "advibly get generation", "advibly
get products", "advibly upload asset", "advibly add subtitles"). Confirm parameter names and
supported durations against what loads rather than assuming. The core calls are:

```text
advibly_generate_image     -> gpt-image-2 storyboard stills
advibly_generate_video     -> gemini-omni-flash or seedance-2.0 image-to-video (8s)
advibly_generate_voiceover -> one continuous narrator MP3 (xAI TTS)
advibly_generate_music     -> one instrumental score bed (MiniMax 2.6)
advibly_render_composition      -> merge the SFX-only clips in beat order
advibly_get_generation     -> finished URL of any generation when you need it downstream
```

A generate call returns a public `url` (and renders in chat). If it returns `status: pending`,
call `advibly_get_generation` (`wait: true`) only when you need the finished URL downstream: for a
still feeding motion, or a prior still anchoring the next beat, you always do.

Use `reference_image_urls` for still continuity and `start_image_url` for video. Do **not** pass
both `start_image_url` and `reference_image_urls` to a video call: it changes the mode and the
approved still may no longer be the first frame.

For a store brand, use the available product/catalog tool to find the hero product image. For any
other brand, use an approved asset or upload a user-supplied product photo with the available
asset tool. Pass that URL as a still reference only on product-reveal and CTA frames. Do not
attach it to the problem or mechanism beat or it may leak the package into the hook. Do **not**
pass `product_id` to the generation tools: it forces edit mode against the raw photo.

Use `brand_id` (required on every call) to file work with the chosen brand. Default
`on_brand: false` for images: the feature-animation palette and the product-photo reference should
control the art direction. Turn it on only when the user explicitly wants the brand kit to shape
the setting or palette. Verify the product label against the supplied reference; do not rely on
invented small text.

Check credit balance before the animation pass. If the available MCP returns a per-render price,
show the total for the approved storyboard plus up to two retries per beat and wait for explicit
confirmation. If it does not expose pricing, tell the user that the video, voiceover, and music
passes spend Advibly credits and obtain confirmation before starting them.

## Phase 1: intake

Collect only what is still missing. Start with the brand and hero product; infer reasonable
creative defaults from the brand context.

1. Resolve the brand. If several exist, ask which one; if none exist, direct the user to Advibly
   onboarding.
2. Read the brand context. Use its audience, approved claims, tone, and proof to choose the
   problem character, the script, and the narrator voice. Fetch a fuller dossier only when a
   claim or objection needs it.
3. Resolve one hero product and its reference photo. For services or software, use a screen,
   interface, or symbolic product object rather than forcing a physical package.
4. Ask for a target platform only when it matters. Default to 9:16 vertical and four 8-second
   clips, for an approximately 32-second cut. Use `duration: 8` for every Gemini or Seedance
   generation in this skill. Do not vary duration unless the user explicitly overrides this rule.

Do not ask for a full casting brief unless the user has a strong preference. Build a sensible
original protagonist, setting, and mascot system from the brand, then expose them in Phase 2.

## Phase 2: cast sheet and beat map (mandatory approval)

Read `references/cast-and-story.md`. Deliver an editable JSON map with:

- `brand_id`, product, product-photo URL, category, audience, aspect, and language
- exact protagonist wording: age range, hair, eyes, skin details, outfit, and personality cue
- the anthropomorphized problem, its face placement, and its emotion
- the mascot form, color, eye treatment, and mechanism action
- the repeatable setting and a byte-identical style-lock phrase
- `video_model` and `model_source` (`default` or `user-explicit`)
- `voice` (the narrator: `ara` warm-storyteller default, `sal` smooth, `leo` authoritative,
  `rex` confident, `eve` energetic) and `music` (a one-line instrumental brief)
- four 8-second beats, each with visual goal, narration line written to time, SFX, and whether it
  shows the product

Make the claim visual metaphorical and aligned with approved product proof. Do not depict medical
diagnosis, treatment, or guaranteed physiological outcomes. Write each narration line to time
(~2.2 to 2.6 words per second, roughly 18 to 20 words per 8-second beat) so no clip rides silent.
Ask the user to approve or edit the map. Do not render until they approve.

## Phase 3: storyboard stills (mandatory visual approval)

Read `references/storyboard-prompts.md`. Generate one 9:16 still per beat with
`advibly_generate_image`, `model: "gpt-image-2"`, `quality: "high"`, `on_brand: false`, and
`num_images: 1`. Poll with `advibly_get_generation` (`wait: true`) when a usable URL is needed
downstream.

```text
beat 1: no character or product reference; approve as the visual anchor
beat 2: product photo URL + an existing approved hero URL, if the user supplied one
beat 3: no product photo; use only a mascot reference if the map has one
beat 4: approved beat-2 protagonist URL + product photo URL
```

If the product must be legible, use the supplied photo as a reference, describe its exact
packaging, and reserve a clean lower third for the later CTA. Still generation is sequential for
beats 1, 2, and 4. Treat the approved beat-2 still as the protagonist identity anchor when no
hero reference was supplied. Beat 3 can run independently after the style lock is established.

Show the complete storyboard in beat order with the narration line, target duration, and QA
result. Regenerate only failed stills. Do not begin video until the user approves the board.

## Phase 4: animate approved stills

Read `references/animate-prompts.md`. Select the model once before the render pass:

- **User names Gemini Omni Flash:** use `gemini-omni-flash` for every clip.
- **User names Seedance 2.0:** use `seedance-2.0` for every clip.
- **No model named:** use `gemini-omni-flash` for every approved standard beat. Consider
  Seedance only after two Gemini retries on a clip or for a needed end-frame transition. Explain
  why and get user approval before mixing in the fallback.

For each approved still, call `advibly_generate_video` with:

```text
model: <selected model>
mode: "pro"                         # Seedance only; omit for Gemini
aspect_ratio: "9:16"
duration: 8
start_image_url: <approved beat still URL>
prompt: <beat-specific SFX-only motion prompt>
```

Do not add `reference_image_urls` to these calls. Gemini does not support an end frame; reserve
`end_image_url` for an explicitly selected or approved Seedance transition. Run the approved clips
in parallel when the MCP/client permits it, then poll each with `advibly_get_generation` for its
final URL. Regenerate only failed clips, up to two retries per beat. Tighten the relevant
constraint instead of adding vague quality adjectives.

## Phase 5: voiceover + music + final composition

Full recipe and gotchas in `references/audio-and-gotchas.md`.

1. **Voiceover.** Join the four beats' narration lines into one continuous warm read and generate
   it once:
   ```text
   advibly_generate_voiceover
     brand_id: <brand id>
     text: <the full narration, beats joined in order; [pause] between beats when a line lands
            short of its 8s window; <slow>...</slow> on the CTA line>
     voice: <the beat map's voice: ara / sal / leo / rex / eve>
     language: <only when auto-detection would get it wrong>
   ```
   The script was written to time in Phase 2. If it exceeds the scene total, tighten and regenerate
   (cheap), never time-stretch the voice.
2. **Music.** Generate one gentle instrumental bed from the beat map's `music` description
   (`instrumental: true`). Warm, hopeful, storybook; composition auto-trims it with a tail fade.
3. **Compose once.** Call `advibly_render_composition` with approved clips as ordered `scenes`
   (each `volume: 0.3`), `voiceovers: [{source: <VO generation id>}]`, the bed as `music`,
   `aspect_ratio: "9:16"`, and `keep_scene_audio: true`. The default music level already sits
   correctly under narration. It returns `status: pending`, `generation_id`, and `edit_url`.
4. Mention the `edit_url` in final delivery so the user can fine-tune the ad in the Advibly video editor.

Never ask the video model to bake narration or captions into a clip. Verify the final master has
no dead silent tail, correct 9:16 framing, a readable product label, and a clean CTA lower third.

## Phase 7: captions (optional, after the VO is mixed in)

Upload the final mix with `advibly_upload_asset` (`source_url`), then `advibly_add_subtitles`
with a preset (TikTok white-with-black-stroke is the neutral default). Add the brand and product
names to `vocabulary` so the transcriber spells them right. Never caption the SFX-only cut; there
is nothing to transcribe.

## Phase 8: optional publish

If the user wants to post it: `advibly_social_list_accounts`, then `advibly_social_create_post`
with the final video. Only offer after the user has seen the finished ad.

## Failure handling

- **Pending generation:** call `advibly_get_generation` (`wait: true`) for the matching id until a
  reusable URL is returned.
- **Character drift:** regenerate the offending still from the approved identity anchor. Reuse
  exact hair, outfit, eye, and setting wording. Do not try to repair it in video.
- **Bad hands, eyes, or label:** regenerate the still with a short specific constraint such as
  "exactly five fingers on each visible hand" or "label remains clear and unchanged."
- **Video morphing:** simplify to one primary action, lock the camera, keep it to 8 seconds, and
  repeat the start-frame preservation constraint. Do not add more motion to hide the defect.
- **VO runs longer than the picture:** tighten the narration and regenerate (preferred), or slow
  the picture to fit before composition. See `references/audio-and-gotchas.md`. Never time-stretch the
  voice.
- **Insufficient credits:** use the connected credit-purchase tool and share its checkout link;
  do not substitute another paid provider.
