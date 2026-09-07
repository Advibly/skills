# Grounding, Real People, Competitors and Compliance

The rules that keep a batch honest. Read once per session; apply to every card.

## 1. Where copy is allowed to come from

In order of preference:

1. **The user's own corpus.** Winning ads, reviews with names and platforms, ad comments. Ask once at intake. Read attached files; use pasted text verbatim.
2. **The Advibly brand brief** (`advibly_get_brand` → `brand_brief`). Sections and what they are for:
   - `Voice of Customer`: quotes with a source tag. Entries tagged with a publication (Glamour, The Verge) are press, not reviews; entries tagged `site`, `App Store`, `Trustpilot` are reviews. A quote with a real name beats a quote without one.
   - `Proof`: numbers with their basis. Keep the basis in the ad ("of 141 surveyed customers").
   - `Objections and Rebuttals`: FAQ cards. Verify the rebuttal against the product's real tiers and policies before rendering.
   - `Why Us, Not Them`: named competitors and the axis of difference. This is the only place a competitor name may come from.
   - `Before to After`: the two states for before-and-after and problem-and-solution.
   - `Angles to Explore`: headline and lifestyle-line inspiration.
   - `Voice and Tone` and `Guardrails`: banned words and banned claims. Read before writing.
3. **The dossier** (`advibly_get_brand_dossier`): the longer quote bank, competitors, pricing, press. Fetch it when the brief's `Voice of Customer` has fewer than three review-type entries or the brief is missing.
4. **Product data** (`advibly_get_products`): names, descriptions, ingredient lists, prices, variants. Factual and safe for headline, spotlight, numbered list.
5. **Store metadata** (`advibly_get_brand` → `context_brand`): app-store `averageUserRating` and `userRatingCount`, the app description's own quoted user reviews (these usually carry names), the store description sentence.

Nothing else. Not general knowledge about the category, not "typical" numbers, not a plausible reviewer.

## 2. Verbatim rules

- Reviews are quoted as written. Trim to one sentence for a testimonial stack, but do not rewrite the customer's words into marketing voice.
- Numbers keep their denominator and their source. "94% fall asleep in under 1 hour" becomes "94% of 141 surveyed customers fell asleep in under an hour". If the brief's Guardrails require a specific phrasing (Som Sleep's does), use it.
- Press quotes keep the outlet named in the ad.
- When a source has no name: "Verified buyer", "App Store review", or the outlet. Never invent a first name and a last initial.
- Founder words: only what the founder supplied. Otherwise write in third person from facts, and mark first-person drafts "needs founder sign-off" in the concept card.

## 3. Real people

- **Never generate a real person's face.** Founder, named reviewer, athlete, press author. Options: their real photo via `advibly_upload_asset` passed in `reference_image_urls` with "reproduced faithfully"; a faceless scene (hands at a workbench, seen from behind, over the shoulder); an initial in a circle for review avatars.
- Generated people are fine when they are nobody: the person in a lifestyle hero, a hand holding the product. Describe them by scale, framing, gaze and what their hands are doing.
- Do not put a named reviewer's quote next to a generated face. The pairing implies the face is theirs.

## 4. Competitors

- Name a competitor only when the brief's `Why Us, Not Them` names them and the difference is factual (format, pricing model, ingredient count, what the product is or is not).
- Plain text name only. Never render their logo, packaging, colours or UI.
- If the brief does not name anyone, use the category default: "greens powders", "OTC sleep aids", "your spreadsheet", "the notebook".
- "Us vs. them" rows must each be something the brand's own materials claim about itself; the cross mark on the other side is implied by the brand's differentiation, not a factual claim about a named competitor unless the brief makes it.

## 5. Platform compliance

- **Before and after** in health, beauty, weight and finance is restricted on Meta and Google. Prefer ritual, workflow and environment transformations. Body or outcome transformations only when the user confirms policy.
- **Health claims:** supplements cannot claim to treat, cure or prevent. Use the brand's own phrasing ("supports", "drug-free", "non-habit forming") and the Guardrails section.
- **Press:** only outlets that covered the brand. Set names as text wordmarks, not reproduced logos.
- **Ratings:** the number and the count must match the store or platform at generation time. Pull them from `context_brand` rather than memory.
- **Comparative claims:** see section 4. Some platforms restrict naming competitors; flag it in the card.

## 6. The no-brief fallback

When `brand_brief` is missing and the dossier is empty:

1. Say so in one line. "This brand has no research brief yet, so the batch is limited to what the product catalog and store page can back."
2. Templates that survive on product facts alone: 1 headline statement, 7 problem and solution (from the product description's own pain framing), 9 feature or ingredient spotlight, 11 lifestyle hero, 12 numbered list (features only, no "reasons customers switched"). App brands add 3 stat callout from the store rating.
3. Skip and list: 4 review card, 5 testimonial stack, 8 founder message, 10 press mention, 13 FAQ card, 14 competitor callout, 15 origin story, 2 us vs. them, 6 before and after (unless the product page has a transformation claim).
4. Offer the fix: the user can paste reviews or attach a winning-ads folder, and the skipped templates come back.

## 7. Concept card checklist

Before a card goes in the sheet:

- [ ] Template named, brand named.
- [ ] Headline under twelve words, body under thirty, one idea.
- [ ] Every claim traceable to a source listed under "Grounded in".
- [ ] No banned words from the brief's `Voice and Tone` or `Guardrails`.
- [ ] Numbers carry their denominator.
- [ ] No real person's face is generated.
- [ ] Competitor, if named, comes from the brief and is text only.
- [ ] Product-anchored template has a `product_id` or a reference URL.
- [ ] Image prompt follows the skeleton and closes with the constraints.
