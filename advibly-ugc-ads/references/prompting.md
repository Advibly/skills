# Omni Flash Prompting Reference

How to write storyboard start-frame prompts (gpt-image-2) and `advibly_generate_video` prompts (Gemini Omni Flash) that produce UGC output instead of commercial output.

---

## The Core Problem

Without specific style modifiers, Gemini Omni Flash defaults to commercial-looking output: perfect lighting, perfectly centered framing, polished skin, studio-quality composition. That is the opposite of what converts on Meta and TikTok. UGC converts because it looks real. Your prompts have to deliberately introduce imperfection.

---

## Required Style Modifiers (Every Shot)

Add all of these to every prompt:

| Modifier | What it does |
|----------|-------------|
| `handheld feel, slight camera shake` | Removes the locked-off tripod look |
| `candid` | One of the strongest realism signals; tells the model this is not a set piece |
| `natural skin texture` | Prevents the "plastic skin" AI tell |
| `imperfect framing` | The model centers everything without this; real UGC is never perfectly composed |
| `[environment-specific lighting]` | Replaces the default studio light inference |

**Lighting options by environment:**
- Kitchen: `soft morning window light` / `warm overhead kitchen light`
- Car: `soft overcast daylight through windshield` / `warm afternoon side light`
- Bathroom: `natural vanity light` / `warm bathroom overhead`
- Gym: `overhead fluorescent, slightly harsh` / `natural light from window`
- Outdoors: `overcast natural light` / `direct afternoon sun, slight squint`
- Bedroom: `soft morning light through curtains` / `bedside lamp, warm`

---

## Storyboard Start Frames

Every clip is animated from a start frame generated with gpt-image-2 (`advibly_generate_image`, 9:16). The frame owns the composition: creator likeness, outfit, product placement, environment, lighting. The video prompt only adds motion and speech. Get the frame right and the clip stays right.

**Reference rules for frame generation** (`reference_image_urls`):
- Creator-on-camera shots: the creator image URL goes on EVERY frame. This is what keeps the same person across the ad.
- B-roll shots (creator not in frame): leave the creator image out; reference only the product photo, or nothing for pure environment frames.
- The product photo URL goes on every frame where the product is visible.
- In the prompt text, point at the references descriptively: "the woman from the reference image," "the product from the reference photo." Reinforce the product visually in words too: color, form factor, "label clearly visible."

**Frame craft:**
- Write the frame as a frozen mid-moment, not a portrait: "about to speak to camera," "reaching for the bottle," "mid-pour." A posed photo animates stiffly; a paused-video moment animates naturally.
- End the prompt with "Looks like a paused video frame, not a posed photo."
- Use the same candid modifiers as video prompts (below); a polished frame produces a polished clip.

**Passing the frame to video:**
- The approved frame URL goes in `start_image_url` on `advibly_generate_video`.
- Do NOT also pass `reference_image_urls` on the video call: that switches the pipeline to reference-to-video and the frame stops being the opening frame.
- Never pass `product_id` to the video tool: it would replace the storyboard frame with the raw product photo as the opening frame.

**What breaks it:**
- Dropping the creator reference on one frame = character drift between shots
- Omitting the product reference on a product frame = the model hallucinates a generic product
- Describing the creator differently across frames (different outfit or hair wording) = the model reinterprets them; reuse the same descriptor phrases verbatim

---

## Multi-Shot Consistency

There is no session memory between calls: every frame and every clip is an independent render. Consistency comes entirely from what you repeat.

- Same creator reference URL on all creator-shot frames
- Same outfit and hair phrasing, copied word for word across frame prompts
- Shots that share a location (Shot 1 and Shot 5 in the testimonial angle) reuse the exact environment sentence
- Same lighting phrase whenever the environment repeats
- Same aspect ratio (9:16) on frames and clips, same duration on every clip, so the cut feels continuous
- On the video prompt, anchor the render to its frame: "The person, outfit, product, and environment stay exactly as in the first frame."

---

## Dialogue

Omni Flash generates native audio: write the dialogue into the prompt as `Speaking directly to lens: "..."` and the creator speaks it in the clip.

Budget approximately 3 words per second of screen time.

| Shot length | Max dialogue |
|------------|-------------|
| 6 seconds | ~18 words |
| 8 seconds | ~24 words |
| 10 seconds | ~30 words |

Over-scripted dialogue rushes and sounds like a script. Under-scripted leaves dead air. Stay within the budget.

Voice caveat: each render synthesizes its own voice, so the creator's voice varies slightly between clips. Keeping dialogue tone and energy consistent in the writing minimizes how noticeable it is.

---

## UGC Dialogue Patterns

Write dialogue like a real person texts, not like a brand writes copy.

**Green:**
- "Okay I need to talk about this."
- "Bro, these finally came in."
- "I've been waiting to post this."
- "This is my new favorite thing."
- "Hear me out."
- "I was skeptical but..."
- "Week two and I'm already noticing a difference."

**Red:**
- "Introducing our new innovative product..."
- "Experience the difference with..."
- "Clinically proven to..."
- "Shop now and save..."
- Anything that sounds like it was written by a brand

**CTA patterns that work:**
- "Link is below."
- "You literally have nothing to lose."
- "Just try it for 30 days."
- "I'll link it."
- "Trust me on this one."

**CTA patterns that kill the UGC feel:**
- "Shop now"
- "Buy today"
- "Use code [X] for [Y]% off" (save this for the caption, not dialogue)
- "Visit our website"

---

## Hard Categories: What to Avoid and How to Work Around It

Some shot types reliably hallucinate in Gemini Omni Flash:

**Eating / drinking:**
Problem: food disappears, color appears on wrong surfaces, consumption motion looks wrong.
Workaround: "holds the product up to camera, smiles, does not eat or drink." End on hold, not consumption.

**Makeup application:**
Problem: color transfers to wrong body parts, pre-existing makeup marks appear, smudging.
Workaround: "holds the product up to camera, does not apply. Finished look: [describe the result]." Show the result without showing the application.

**Complex hand-object interaction:**
Problem: extra fingers, object warping, product changing shape mid-shot.
Workaround: keep interactions simple: pick up, hold, rotate, set down. One action per shot.

**Small text / fine print on packaging:**
Problem: text garbles, especially at arms length.
Workaround: keep the product large in frame (filling 30-40% of the shot), keep the environment simple behind it, and reinforce the label in the prompt ("label clearly visible," plus the product color and form). If a label still garbles, regenerate that one shot with the product closer to camera.

---

## Shot-Level Prompt Templates (Complete)

**Start frame (gpt-image-2, one per shot):**

```
Vertical 9:16 photo, the first frame of a UGC video.
[Creator shots: The woman from the reference image, [outfit detail, same wording every shot],]
[environment: specific room/setting],
[frozen mid-moment pose: about to speak to camera / reaching for the product / mid-action].
[If product in frame: the product from the reference photo, how it is held or placed,
label clearly visible, color/descriptor.]
Candid iPhone photo look, handheld framing slightly off-center, natural skin texture,
[environment-specific lighting], imperfect framing.
Looks like a paused video frame, not a posed photo.
```

**Clip (Gemini Omni Flash, animating the approved frame via `start_image_url`):**

```
The scene comes to life from this frame.
[Action: one action only, continuing naturally from the pose].
Speaking directly to lens: "[dialogue, within word budget]."
[B-roll: no one on camera, voice-over says: "[dialogue]."]
Handheld feel, slight camera shake, candid, natural skin texture, imperfect framing.
The person, outfit, product, and environment stay exactly as in the first frame.
```

Fill every bracket. Vague prompts = generic AI output. Specific prompts = UGC output.
