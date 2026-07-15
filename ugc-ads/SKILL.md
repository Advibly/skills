---
name: ugc-ads
description: Generate complete multi-shot UGC video ads end to end with the Advibly MCP tools. Takes a brand plus an ad angle and produces a realistic AI creator image (gpt-image-2), a 5-shot direct response script, a user-approved storyboard of start frames, and one video clip per shot animated from its frame (Gemini Omni Flash with native spoken dialogue), then assembles the clips into a finished vertical ad. Use whenever the user wants a UGC ad, a creator-style video ad, a product-in-hand testimonial video, or asks to turn a product into a TikTok/Reels-style ad. Also trigger on phrases like "UGC video," "creator ad," "testimonial ad," or "make an ad for my product" when the Advibly MCP is connected.
---

# Advibly UGC Ad Generator

Generates complete multi-shot UGC video ads through the Advibly MCP. One brand plus one ad angle produces a realistic AI creator, a direct response script, a storyboard of start frames the user approves, and a set of vertical video clips animated from those frames with native spoken dialogue, assembled into a finished ad.

## What this skill does

1. Picks up the brand and product from the user's Advibly account
2. Generates a realistic AI creator reference image with `advibly_generate_image` (gpt-image-2)
3. Writes a 5-shot direct response script with dialogue for each scene
4. Builds a storyboard: one start frame per shot (gpt-image-2, creator + product as references), shown to the user for approval before any video renders
5. Animates each approved frame with `advibly_generate_video` (Gemini Omni Flash image-to-video, 9:16, native audio: the creator speaks the dialogue)
6. Assembles the clips into one finished vertical ad with one `advibly_render_composition` call

Every generation spends the account's Advibly credits. Renders are the expensive part; script edits are free. Get the dialogue right before generating video.

## Requirements

- The Advibly MCP connected (tools named `advibly_*`)
- An onboarded brand at advibly.com (the skill reads it, it cannot create one)
- Enough credits: roughly six images (creator + five storyboard frames) plus five 8-second Omni Flash clips per ad

## Workflow

### Step 1 - Intake

Collect, in order of priority:

1. **Brand** - `advibly_list_brands`. One brand: use it. Several: ask which. None: send the user to advibly.com/onboarding.
2. **Brand context** - `advibly_get_brand` for identity, tone, and the research brief. Use it to sharpen dialogue voice and audience targeting. Call `advibly_get_brand_dossier` only if you need objections, competitor claims, or voice-of-customer quotes for the script.
3. **Product** - the physical thing the creator holds on camera.
   - Store brands (`brand_type: "ecom_store"`): `advibly_get_products`, pick the product with the user, and note its image URL. Do NOT pass `product_id` to the video tool later; this skill needs the photo as a reference image, not a start frame (see Step 4).
   - Other brand types: check `advibly_get_assets` for a usable product photo, or ask the user to provide one (`advibly_upload_asset`). If there is no physical product (SaaS, apps), the ad becomes a to-camera testimonial without product-in-hand shots; screenshots from the asset library can appear as phone-in-hand content instead.
4. **Ad angle** - pick one or ask:
   - `testimonial` - creator speaks to camera about results (default)
   - `car/on-the-go` - creator in car or commuting, convenience angle
   - `unboxing` - creator reveals and reacts to product
   - `lifestyle-demo` - creator using product in natural environment
   - `problem-solution` - before state, product intro, after state
5. **Creator description** - brief description (age, gender, vibe). If not provided, match an archetype from `references/characters.md` to the product category. Default: "27-year-old woman, relatable fitness-oriented, not model-perfect."

Once the brand is resolved, create the run's project with `advibly_create_project` (`brand_id` plus a deliverable-shaped name like "Acme UGC testimonial ad") and pass the returned `project_id` on every generate call in this workflow (creator image, storyboard frames, clips, the final composition) so the whole run shows as one tile in the user's library. If the user is continuing an earlier run, find its project with `advibly_list_projects` instead of creating a duplicate.

Do not ask for everything at once. Brand plus angle is enough to start; fill the rest from the brand data and sensible defaults.

### Step 2 - Generate the creator reference image

Build the prompt from the archetype formula (full templates in `references/characters.md`):

```
Candid iPhone photo of a [age]-year-old [gender], [specific physical detail: hair color/texture with natural flyaways, freckles or not], [minimal/natural/no] makeup, wearing [specific casual outfit, real clothing, not "stylish"], [environment detail: sitting in car / standing in kitchen]. Shot from [slight angle: low angle slightly off-center / slightly above eye level]. [Specific lighting: soft overcast daylight through window / warm morning kitchen light]. Slightly imperfect exposure. Real, unretouched skin texture. Natural hair flyaways. No studio lighting. Editorial-style realism.
```

Rules:
- Never use "beautiful," "gorgeous," "stunning," or model descriptors
- Always include a specific environment, even in the reference shot
- Always include at least one deliberate imperfection (flyaways, freckles, uneven lighting)
- Always include "Candid iPhone photo" (one of the strongest realism signals)

Call:

```
advibly_generate_image
  prompt: <the creator prompt>
  project_id: <project id>
  model: "gpt-image-2"
  aspect_ratio: "2:3"
  quality: "high"
```

Do NOT pass `brand_id` (or pass `on_brand: false` if you do). The creator image must look like a real person's phone photo; brand-kit references would pull it toward branded collateral. Do pass the run's `project_id`: it only groups the generation under the project and does not affect the look.

If the call returns `status: pending`, fetch the finished URL with `advibly_get_generation` (`wait: true`). You need this URL for every video call. Show the image to the user; regenerate on request (tweaks to hair, age, outfit, environment are one-line prompt edits).

### Step 3 - Write the 5-shot direct response script

For the chosen angle, write the complete shot list (per-angle breakdowns in `references/angles.md`). Each shot gets:

```
Shot N/5 - [Shot name]
Environment: [where the creator is, specific room/setting/context]
Action: [exactly what is happening, one action only per shot]
Creator on camera: [yes / no (B-roll)]
Product in frame: [yes/no, and if yes, how]
Dialogue: [what the creator says, ~3 words per second, UGC voice; B-roll dialogue plays as voice-over]
```

Most shots feature the creator on camera. A shot can be B-roll instead (hands-only close-up, product on a counter, environment detail) when showing the creator adds nothing: common for problem-agitation and transformation beats. Mark it in the shot list; B-roll changes how its storyboard frame is generated in Step 4.

**Direct response structure (default, testimonial angle):**

| Shot | Job | Environment | Product |
|------|-----|-------------|---------|
| 1 - Hook | Stop the scroll with a single claim | Kitchen / bathroom / neutral | Yes, held toward camera |
| 2 - Problem | Make the viewer feel the pain | Bedroom / desk / car | No |
| 3 - Discovery | Introduce the product naturally | Kitchen / bathroom | Yes, opening/interacting |
| 4 - Transformation | Show the result. Specific, not vague | Gym / outdoors / work | No |
| 5 - CTA | One action, remove friction | Back to Shot 1 environment | Yes, held toward camera |

**Dialogue rules:**
- UGC voice: "Okay I need to talk about this," not "Introducing our new product"
- ~3 words per second of screen time. An 8-second shot = ~24 words max
- First line is the hook and must work without sound (captions carry it on autoplay)
- Product name appears in Shot 3 or later, never Shot 1
- CTA is punchy: "link is below" / "you literally have nothing to lose," never "shop now" or "buy today"
- Mirror the brand's tone of voice from `advibly_get_brand`, but keep it person-casual, not brand-polished

Show the user the shot list with the dialogue before generating the storyboard. Dialogue changes are free; renders cost credits. Then proceed.

### Step 4 - Build the storyboard (five start frames)

Generate one start frame per shot with gpt-image-2. Each frame is the exact opening moment of its clip: the video model animates from it, so whatever is in the frame (the creator's face, outfit, the product, the room) is what the clip keeps. This is what makes the ad look seamless.

For each shot:

```
advibly_generate_image
  prompt: <start-frame prompt, see below>
  project_id: <project id>
  model: "gpt-image-2"
  aspect_ratio: "9:16"
  quality: "high"
  reference_image_urls: [<creator image URL>, <product photo URL if product is in frame>]
```

Reference rules:
- Creator-on-camera shots: the creator image URL goes in `reference_image_urls`. Missing it = creator drift between shots.
- B-roll shots (creator not in frame): do NOT include the creator image; reference only the product photo (or nothing for pure environment shots).
- Add the product photo URL whenever the product is in frame, on both creator and B-roll frames.
- In the prompt text, point at the references descriptively: "the woman from the reference image," "the product from the reference photo, red bottle, label clearly visible."

**Start-frame prompt structure:**

```
Vertical 9:16 photo, the first frame of a UGC video. [For creator shots: The woman from the reference image, [outfit detail, same wording every shot],] [environment: specific room/setting], [frozen mid-moment pose: about to speak to camera / reaching for the product / mid-action]. [If product in frame: the product from the reference photo, how it is held or placed, label clearly visible, color/descriptor.] Candid iPhone photo look, handheld framing slightly off-center, natural skin texture, [environment-appropriate lighting], imperfect framing. Looks like a paused video frame, not a posed photo.
```

Keep the environment and outfit wording identical across shots that share a location (Shot 1 and Shot 5 in the testimonial angle) so the scenes match.

Execution notes:
- Fire the five image calls in parallel if the client supports it; collect finished URLs with `advibly_get_generation` (`wait: true`).
- **Present the storyboard to the user**: all five frames in shot order, each with its shot name and dialogue line. This is the approval gate. Regenerate individual frames on request (a frame tweak is one cheap image; a bad clip is an expensive video). Do not start video generation until the user approves the storyboard.

### Step 5 - Animate the five clips

For each approved frame, call `advibly_generate_video` with Gemini Omni Flash in image-to-video mode. The model generates native audio, so the creator speaks the dialogue in the clip itself.

```
advibly_generate_video
  prompt: <shot prompt, see below>
  brand_id: <brand id>
  project_id: <project id>
  model: "gemini-omni-flash"
  aspect_ratio: "9:16"
  duration: 8
  start_image_url: <the shot's approved storyboard frame URL>
```

Rules:
- `start_image_url` is the approved frame. Do NOT also pass `reference_image_urls`: that switches the pipeline to reference-to-video and the frame stops being the opening frame. The frame already carries the creator and product likeness.
- Never pass `product_id` to this tool: it would replace your storyboard frame with the raw product photo as the opening frame.
- The prompt now describes motion and speech, not composition (the frame owns composition): what the creator does from this frozen moment, and the dialogue.

**Shot prompt structure:**

```
The scene comes to life from this frame. [Action, one action only, continuing naturally from the pose]. Speaking directly to lens: "[dialogue]." [For B-roll: no one on camera, voice-over says: "[dialogue]."] Handheld feel, slight camera shake, candid, natural skin texture, imperfect framing. The person, outfit, product, and environment stay exactly as in the first frame.
```

Full modifier tables, lighting options, and hallucination workarounds are in `references/prompting.md`. The non-negotiable modifiers on every shot: `handheld feel, slight camera shake`, `candid`, `natural skin texture`, `imperfect framing`. Without them Omni Flash defaults to polished commercial output, which is the opposite of what converts.

Execution notes:
- Fire the five calls in parallel if the client supports it; video renders take a few minutes each.
- Each call waits ~40s and usually returns `status: pending` with a `generation_id`. Collect finished URLs with `advibly_get_generation` (`wait: true`), one per generation.
- `content_rejected` means the content policy blocked the prompt: rework the wording, do not retry verbatim.
- `insufficient_credits`: call `advibly_buy_credits` and share the checkout link; credits apply automatically after payment.
- If one shot comes back wrong (garbled label, weird hands, off dialogue), regenerate only that shot.

### Step 6 - Assemble the ad

Call `advibly_render_composition` once with the five generation ids or HTTPS URLs as ordered
`scenes`, `keep_scene_audio: true`, `aspect_ratio: "9:16"`, and the run's `project_id`. Do not add voiceovers or music:
the creator's native spoken audio belongs to each scene. The tool hard-cuts the clips, returns
`status: pending`, a `generation_id`, and an `edit_url`. The chat widget polls the render; call
`advibly_get_generation` with `wait: true` only if a finished URL is required for captions or
publishing. Deliver the render and mention the `edit_url` so the user can fine-tune the ad in the
Advibly video editor. Then set the finished ad as the project cover with `advibly_update_project`
(`project_id` plus `cover_generation_id: <the composition's generation id>`). Total runtime is
about 40 seconds for five 8-second shots.

### Step 7 - Optional: publish

If the user wants to post the ad, `advibly_social_list_accounts` shows the brand's connected platforms and `advibly_social_create_post` publishes or schedules it (attach via `generation_ids` for single clips, or `media_urls` for a stitched file uploaded with `advibly_upload_asset`). Only offer this after the user has seen the final ad.

## References

- `references/prompting.md` - style modifiers, storyboard start-frame rules, dialogue pacing, what breaks realism and consistency, hallucination workarounds
- `references/angles.md` - shot-by-shot breakdowns for all 5 preset angles
- `references/characters.md` - creator image prompt templates for 8 archetypes, tuned for gpt-image-2

## Honest limits

- Voice drifts between clips: each Omni Flash render synthesizes its own voice, so the creator sounds slightly different shot to shot. Casting the same creator reference keeps the look consistent; the voice is close but not identical. If the user needs one continuous voice, point them at `advibly_generate_talking_video` (single-scene talking actor) instead.
- Product label text can garble when the product is small in frame or the environment is busy. Keep the product large and close (30-40% of frame) in product-forward shots.
- Character consistency flows through the storyboard: creator reference on every creator-shot frame, and that frame as `start_image_url` on the clip. If the creator drifts in a clip, the fix is usually regenerating its storyboard frame, not the video prompt.
- Eating/drinking shots hallucinate. Use hold-and-show or hold-and-smile endings for any consumable product.
- Complex hand-object interaction warps. One simple action per shot: pick up, hold, rotate, set down.
