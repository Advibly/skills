# Video Motion Prompts

Use this only after the user approves the storyboard. Make each approved still one image-to-video
clip. Pass it as `start_image_url`; do not add `reference_image_urls` to the same request.

## Model selection and durations

| Situation | Model | Duration | Notes |
|---|---|---:|---|
| User explicitly requests Gemini Omni Flash | `gemini-omni-flash` | 8s | Use it for every clip. No end frame. |
| User explicitly requests Seedance 2.0 | `seedance-2.0` | 8s | Use it for every clip. End frame is available. |
| User does not name a model | `gemini-omni-flash` | 8s | Default four-beat cut. |
| Default Gemini clip fails twice or needs an end-frame transition | `seedance-2.0` | 8s | Explain the fallback and get approval before switching that clip. |

Use `duration: 8` for every clip. Both Gemini Omni Flash (any 4-10s) and Seedance 2.0 (any 4-15s)
support it. Hold each shot to 8 seconds: 10-second beats go too still and start to morph. If the
loaded Advibly MCP schema cannot support 8 seconds for the selected model, report that before
rendering. Never silently swap a user-requested model.

Gemini Omni Flash may generate native audio. Direct it to create only scene-appropriate SFX and
room tone. Never request dialogue, narration, lyrics, captions, subtitles, or baked-on text; the
narrator voiceover is generated separately and mixed on top in Phase 6.

```text
model: <selected model>
aspect_ratio: "9:16"
duration: 8
start_image_url: <approved still URL>
prompt: <beat prompt below>
```

Set `mode: "pro"` only for Seedance. Do not set `end_image_url` for Gemini; it is available only
for Seedance and only after the user selected it or approved the fallback.

## Prompt structure

```text
[SUBJECT LOCK FROM APPROVED STILL]
[TIMED PRIMARY ACTION]
[ONE CAMERA DIRECTION]
[STYLE PRESERVATION]
[SFX]
[CONSTRAINTS]
```

Use one primary action and tiny secondary movement. For these 8-second clips, use a tight setup →
action → one small payoff arc; keep it moving so the shot never goes static.
When using Seedance, omit these degrading words: `cinematic`, `professional`, `stunning`, `8k`,
`studio`, and `perfect`. They are not Gemini restrictions.

## Beat 1: problem hook

```text
Image-to-video from the approved start frame. Subject: <PROBLEM CHARACTER, copied verbatim>.
Action: during the first third, it blinks slowly, the small mouth trembles, and the object settles
a little more into its <posture>. During the final two-thirds, it lifts its gaze to camera with one
tiny hopeful reaction. Camera: locked extreme macro with only subtle handheld breathing. Preserve
the original expressive feature-film 3D animation, warm tactile materials, soft volumetric light,
and shallow depth of field from the start frame. Audio: quiet <room-specific ambience> and one
soft characterful sigh, no spoken words. Constraints: preserve exact object shape, eye placement,
surface texture, and framing; no morphing, extra eyes, text, subtitles, captions, live action, or
photoreal conversion.
```

## Beat 2: product reveal

```text
Image-to-video from the approved start frame. Subject: <PROTAGONIST, copied verbatim> holding
<PRODUCT, copied verbatim>. Action: during the first third, they look down at the product with
delighted curiosity. During the middle, their eyes rise to the camera and a gentle smile forms.
During the final third, they make one small confident product-presenting gesture while keeping the
label readable. Camera: locked medium portrait with slight natural breathing only. Preserve the
exact face, hair, outfit, hands, product shape, label, light, and setting of the start frame.
Audio: soft room tone and subtle fabric movement, no dialogue. Constraints: no extra fingers,
label drift, face morphing, text, captions, subtitles, live action, or photoreal conversion.
```

## Beat 3: mascot mechanism

```text
Image-to-video from the approved start frame. Subject: <MASCOTS, copied verbatim> in the approved
<BENEFIT METAPHOR>. Action: in the first half, the mascots begin <approved action> in coordinated
gentle rhythm. In the next third, the soft <energy path> brightens and travels through the scene.
At the end, they pause and share a tiny satisfied glance as the landscape settles into a calm even
glow. Camera: one slow subtle push toward the action. Preserve mascot count, material,
proportions, scene geometry, and warm 3D-animation texture from the start frame. Audio: quiet
magical sparkle and gentle working sounds, no narration. Constraints: no clinical anatomy, horror,
extra limbs, morphing, on-screen text, subtitles, captions, live action, or photoreal conversion.
```

## Beat 4: CTA

```text
Image-to-video from the approved start frame. Subject: <PROTAGONIST, copied verbatim> holding
<PRODUCT, copied verbatim> with label facing camera. Action: in the first half, they give a warm
confident smile and a small happy head tilt. In the second half, they lift the product slightly
closer without covering the label, then settle into a clean end pose. Camera: locked medium
portrait, gentle natural breathing, no zoom. Preserve protagonist identity, product label,
lighting, and clean lower third exactly as the start frame. Audio: soft room tone, no dialogue or
music. Constraints: no extra fingers, product-label drift, face morphing, on-screen text,
subtitles, captions, live action, or photoreal conversion.
```

## Clip QA and retry policy

- Watch from the opening frame through the final frame.
- Confirm the subject still matches the approved still: eye placement, hands, hair/outfit,
  mascot count, and product label.
- Confirm only the requested action happens; remove random new actions on the next retry.
- Confirm no narration, captions, or invented on-screen text appears.
- Retry a failed clip at most twice with one specific tightened constraint.
- If the user explicitly selected a model, do not switch it. If Gemini was the default, ask before
  using Seedance as the fallback. For a persistent failure, return to the storyboard still instead
  of spending more video credits.
