---
name: static-ads
description: >
  Generate a batch of grounded static (image) ad concepts for any brand on the Advibly MCP and render them with gpt-image-2. Fifteen proven layout templates (headline statement, us vs. them, stat callout, review card, testimonial stack, before and after, problem and solution, founder message, feature or ingredient spotlight, press mention, lifestyle hero, numbered list, FAQ card, competitor callout, origin story), cycled across the whole batch so template diversity becomes angle diversity. Every concept pulls its copy from the brand's research brief, dossier, product catalog or the user's own reviews and cites the source; nothing is invented. Plans the batch in text first (template, headline, body, visual, grounding), gets approval, then renders one on-brand image per concept with the copy burned in, the real product attached as a reference for store brands, and delivers an index. Trigger whenever the user wants static ads, image ads, Meta or Instagram or LinkedIn or display ad creative, "static ad concepts", "ad templates", "a batch of ads", "50 concepts", "us vs them ad", "review card ad", "stat ad", "founder note ad", "before and after ad", "creative testing batch", "more ad variations", or asks to turn reviews or a brief into ad creative when the Advibly MCP is connected. Defaults: 15 concepts (one per template), 4:5, gpt-image-2 at high quality, on-brand.
---

# Advibly Static Ads

Turns one brand into a batch of static ad concepts that are structurally diverse and factually grounded, then renders them. The pipeline is: build a source bank from what Advibly already knows about the brand (free), spread the batch across all fifteen templates (free), write every concept as a card the user can approve in one read (free), then render each approved card as one image with the copy burned in. The templates are borrowed from Corey Haines' marketing skills library (`skills/ad-creative/references/static-ad-templates.md`); this skill adapts them to the Advibly MCP.

Speak in the user's language. No em dashes anywhere in output; use periods, commas or line breaks. No emoji in ad copy.

## The two ideas that make this work

1. **Template diversity is angle diversity.** A batch of fifteen clustered on three favourite layouts is three ads with fourteen costumes. Cycle through all fifteen templates; the winner is usually not the one you would have picked by hand. For a batch of N, spread across all templates (one each for 15, two to four each for 30 to 50). If the user has performance data, shift to 60 percent proven templates and 40 percent full coverage, never zero coverage.
2. **Every line of copy has a source.** A headline comes from a review, a proof number, an objection, a competitor line or the founder's real story, and the concept card says which. Invented stats, fake reviewers, paraphrased-into-marketing-speak quotes and made-up press are not "creative liberty", they are compliance and trust failures. If a template has no source material for this brand, skip that template and say so.

## Hard defaults (do not drift)

- **Image model:** `advibly_generate_image` with `model: "gpt-image-2"` and `quality: "high"`. Every template is text-carrying and gpt-image-2 is the strongest at burned-in copy. Switch a single concept to `seedream-5-pro` only when a dense multi-row layout (us vs. them, numbered list, testimonial stack) keeps collapsing after two rerolls. Never mix models inside one concept's rerolls without saying so.
- **On brand:** `on_brand: true`. The logo and brand-kit board are attached automatically, so describe layout and content, not hex codes or font names. Say "following the attached brand kit" where the background or accent matters.
- **Aspect ratio:** `4:5` by default (Meta and Instagram feed). `1:1` for feed plus display, `9:16` for Stories and Reels, `16:9` for LinkedIn and display banners. One ratio per batch unless asked; a second ratio is a second render, not a crop.
- **Copy is burned in at generation.** Advibly has no text-overlay tool by design. Every quoted line in the prompt is the copy, exactly as it should read. Never plan to add text afterward.
- **The real product is a reference, not a description.** For `ecom_store` brands pass `product_id` on every product-anchored template (headline, problem and solution, ingredient spotlight, lifestyle hero, competitor callout, before and after where the product is the "after"). For app brands pass the real store screenshot URLs from `advibly_get_brand` (`scraped_images` of type `app_screenshot`) in `reference_image_urls` whenever a phone screen appears. For website brands with a dashboard, ask for a screenshot and upload it with `advibly_upload_asset`.
- **One project per brand per batch** when the batch has three or more concepts for that brand: `advibly_create_project` first, `project_id` on every generate call, then `advibly_update_project` to set the strongest render as the cover. One or two loose concepts do not need a project.
- **Tools are deferred.** Load schemas with tool search before the first call each session ("advibly list brands", "advibly get brand", "advibly get brand dossier", "advibly get products", "advibly generate image", "advibly get generation", "advibly create project", "advibly upload asset"). Confirm parameter names against what loads.

## Phase A: Intake

1. **Brand.** `advibly_list_brands`. One brand: use it. Several: ask which. Note `brand_type`; it decides where the visual subject comes from (see the table in `references/templates.md`).
2. **Brand context.** `advibly_get_brand`. The `brand_brief` is the primary source bank: Voice of Customer, Proof, Objections and Rebuttals, Why Us Not Them, Angles, Voice and Tone, Guardrails. Read the Guardrails before writing a word; they are the brand's own banned claims and banned words.
3. **Deeper sources, when needed.** `advibly_get_brand_dossier` for the full voice-of-customer quote bank, named competitors and pricing. `advibly_get_products` for store brands (names, descriptions, ingredient lists, prices, hero photos). `advibly_get_assets` for uploaded reviews, screenshots and winning ads.
4. **The user's own corpus.** Ask once, briefly: "Do you have winning ads, reviews, or ad comments I should pull from?" Real reviews with names beat everything Advibly scraped. If they attach files, read them; if they paste text, use it verbatim.
5. **Batch shape.** How many concepts (default 15), which platform and ratio (default Meta feed 4:5), and what to sell (one product, one angle, or the whole brand). Do not ask for all of this at once; brand plus "how many" is enough to start.

**If the brand has no brief** (`brand_brief` missing): try the dossier; if that is empty too, the only grounded material is the product catalog, the store description and whatever the user provides. Say so plainly, then use the templates that survive on product facts alone (headline statement, feature or ingredient spotlight, lifestyle hero, problem and solution from the product description) and skip the ones that need reviews, stats or press. Do not fill the gap with plausible-sounding numbers.

## Phase B: The source bank (free)

Before writing concepts, pull the raw material into a short list you can point at, grouped by template need:

| Bucket | Where it lives in Advibly | Feeds templates |
|--------|---------------------------|-----------------|
| Customer quotes, verbatim, with name and platform when known | brief `Voice of Customer`, dossier quote bank, `advibly_get_assets`, user uploads | 4 review card, 5 testimonial stack, 13 FAQ card (objection phrasing) |
| Proof numbers with their basis (surveys, ratings, counts) | brief `Proof`, app-store `averageUserRating` and `userRatingCount`, product pages | 3 stat callout, 12 numbered list |
| Pain language ("I used to X, now I Y") | brief `Before to After`, `Voice of Customer` | 6 before and after, 7 problem and solution |
| Objections in the customer's words | brief `Objections and Rebuttals`, ad comments the user shares | 13 FAQ card |
| Named competitors and the one axis of difference | brief `Why Us, Not Them`, dossier competitors | 2 us vs. them, 14 competitor callout |
| Ingredients, features, components buyers ask about | product descriptions, app feature lists, brief `Snapshot` | 9 spotlight, 12 numbered list |
| Press and analyst mentions with the outlet named | brief `Proof` and `Voice of Customer` entries sourced to a publication | 10 press mention |
| The founder's real story | user-provided, the about page in the dossier, the brief `Snapshot` | 8 founder message, 15 origin story |
| The one-line claim | brief `The One Job`, slogan, the most repeated benefit | 1 headline statement, 11 lifestyle hero |

Rules for the bank: quote reviews verbatim (typos included if they are the customer's), keep numbers with their denominator ("94 percent of 141 surveyed customers", not "94 percent"), and record the source next to every entry. `references/grounding.md` has the full sourcing and compliance rules, including what to do about real people's faces and competitor names.

## Phase C: The concept sheet (free, and the real approval gate)

Write every concept as a card, in text, before spending a credit:

```
## Concept 3: Stat Callout
Brand: Som Sleep
Headline: "94%"
Body: "of 141 surveyed customers fell asleep in under an hour"
Visual: midnight background, giant number centre, one context line beneath, stick pack anchor bottom
Image prompt: <the full gpt-image-2 prompt, see references/templates.md>
Grounded in: brand brief, Proof: "94% of 141 surveyed customers report falling asleep in under one hour"
```

Assign templates first, then brands and sources, then write copy. Spread the batch across all fifteen; if a template has no grounded material, list it under "Skipped" with the reason rather than forcing it. Vary the visual treatment so two concepts never look like siblings (different backgrounds, product scales, photo versus flat). For a multi-brand batch, rotate brands so no brand takes the same template twice.

Copy rules per template are in `references/templates.md`. Global rules: headline under twelve words, body under thirty, one idea per ad, brand name spelled exactly, no banned words from the brief's Guardrails, no claim the brief cannot back.

Show the sheet. Get approval. Copy edits are free; a regenerated image is not. On a 50-concept batch, add an `INDEX.md` style table (concept number, template, brand, headline, grounding) at the top so the reviewer can scan it in two minutes.

## Phase D: Render

One call per approved concept:

```
advibly_generate_image
  prompt: <the concept's image prompt>
  brand_id: <brand id>
  project_id: <project id, if the batch has a project for this brand>
  model: "gpt-image-2"
  quality: "high"
  on_brand: true
  aspect_ratio: "4:5"
  product_id: <store product id, on product-anchored templates>
  reference_image_urls: [<real screenshot or review image URLs, when the template uses them>]
```

Every prompt follows the skeleton in `references/templates.md`: the ad type and ratio, the template name, the layout system in one sentence, each piece of copy in "quotes" with placement, weight and case, the visual subject (product "from the reference image, reproduced faithfully"), logo placement, then the closing constraints ("every quoted line rendered exactly once, no watermark, no stray text, no invented logos or badges"). gpt-image-2 invents secondary text and fake badges in empty space unless told not to.

The calls are independent, so fire them in parallel when the client allows. A call that returns `status: pending` still shows the image in chat; collect the URL with `advibly_get_generation` (`wait: true`) only when you need it for the index or a reroll reference.

**Rerolls.** A garbled headline is a reroll with fewer words or a simpler layout, not a different model. A comparison table or list that merges rows: cut to four rows, then try `seedream-5-pro`. A product that drifted from the reference: restate "reproduced faithfully, same label, same colours" and keep `product_id`. A render that has been `processing` for more than five minutes at high quality: fire it again at `quality: "medium"` rather than waiting.

## Phase E: Review and deliver

Present the batch as a table: concept number, template, brand, headline, grounding, image. Read each render against its card before showing it: is every quoted line present and spelled right, is the product the real product, did the model add a badge, a fake award or a second logo. Flag misses honestly and reroll the ones that matter.

Then hand over:

- The index (template, headline, grounding, URL) so the user can drop concepts into their ad platform and trace every claim.
- What was skipped and why (usually "no press coverage in the brief" or "no named reviews").
- Which templates to double down on next batch, if the user has said what has worked before.

If the user wants a concept posted, `advibly_social_list_accounts` shows connected platforms and `advibly_social_create_post` publishes or schedules. Offer it only after approval.

## Notes and rules

- **Plan in text, render once.** Cards are free to iterate; images are not.
- **Verbatim beats paraphrase.** A review in the customer's own words converts because it reads as real. Do not tidy it into marketing voice.
- **Real names and real stats only.** No invented reviewer names, no rounded-up counts, no "as seen in" for outlets that never covered the brand. When the source has no name, label it "Verified buyer" rather than inventing one.
- **Never generate a real person's face.** Founder, customer or press author: use their real photo via `advibly_upload_asset` as a reference, or shoot the scene faceless (hands, from behind, initials avatar). A generated likeness of a real person is a liability, not an ad.
- **Comparative claims are the brand's to defend.** Name a competitor only when the brief names them and the axis of difference is factual. Never render a competitor's logo or packaging; set the name in plain text.
- **Health, finance and beauty before-and-afters are regulated.** Prefer workflow or ritual transformations (messy powder to gummy, six tabs to one dashboard) over body or outcome transformations unless the user confirms the platform policy.
- **Respect the brief's Guardrails section literally.** It is the brand's own list of what it cannot say.
- **Self-contained prompts always.** Each render has zero memory of the previous one; restate the layout every time.
- **No minors in generated scenes.** A lifestyle scene with a child at the table was rejected by the output safety layer; the same scene with an adult passed. Use adults, or hands-only framing, even for kids' products.
- **On failure:** `content_rejected` can fire at the input stage (wording) or the output stage (the rendered image); either way rework the prompt rather than retry verbatim, changing the scene first and the copy second. A render that sits in `processing` past five minutes at high quality: re-fire at `quality: "medium"`. `insufficient_credits`: call `advibly_buy_credits` and share the checkout link.

## Honest limits

- **Text fidelity is high, not perfect.** In a 30-concept test across eight brands, every quoted line rendered correctly at `quality: "high"`, including a 60-word founder note and three-paragraph origin copy. The residual risk is text you did not quote: the model invents plausible in-UI text (a song title on a laptop screen) and can echo badges that exist in a reference screenshot. Read every render anyway, and for copy that must be letter-perfect treat the render as the layout and set final text in a design tool.
- **Dense layouts lose rows.** Five comparison rows is the ceiling for us vs. them and numbered list; four is safer. Three quotes is the ceiling for a testimonial stack.
- **UI is reproduced, not replicated.** A phone screen in a FAQ card or before-and-after redraws the app: large type and layout survive, small numbers drift. Pass the real screenshot as a reference and say "reproduced faithfully".
- **The brief is only as good as the research run.** Brands with no brief and no dossier can support four or five templates honestly. Say so instead of inventing the rest.

## References

- `references/templates.md`: the fifteen templates with structure, copy slot, DTC and SaaS and app examples, where to source each in Advibly, compliance notes and a paste-ready gpt-image-2 prompt recipe per template.
- `references/grounding.md`: sourcing rules, the brand-type to visual-subject table, real-people and competitor rules, platform compliance, and the no-brief fallback.
