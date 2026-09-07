# The Fifteen Templates: Structure, Copy, Sources and Prompt Recipes

Each template is a layout framework with slots for brand-specific copy. The structure is proven; the inputs make it yours. Every concept needs: template name, headline, body (if the template uses one), visual description, image prompt, source grounding. No em dashes in anything you produce from this.

Adapted from Corey Haines' `marketingskills` static ad template library for the Advibly MCP.

## Table of contents

1. The prompt skeleton
2. Brand type decides the visual subject
3. The fifteen templates (structure, copy slot, examples, source, compliance, prompt recipe)
4. Batch distribution

---

## 1. The prompt skeleton

Every prompt, every template, same order. Deviating is what produces an ad that ignores half its copy.

```
Static [platform] ad, [orientation and ratio]. Template: [template name].
[Layout system in one sentence: background treatment, zones, alignment.]
[Headline: "EXACT COPY", placement, weight, case, line count.]
[Body, rows, callouts or quote: each piece in "quotes", with placement.]
[Visual subject: the product from the reference image reproduced faithfully / a photographic scene / a device screen, with lighting.]
Small brand logo [placement], from the attached brand kit.
[Closing constraints: crisp legible typography, every quoted line rendered exactly once, no watermark, no stray text, no invented logos or badges.]
```

Notes that matter:

- The brand kit is attached automatically when `on_brand: true`. Do not restate hex codes or font names. Say "following the attached brand kit" for backgrounds and accents, and describe weight, case and placement for type.
- Anything in double quotes is copy. Anything outside quotes is a description. Keep them separate.
- Prefer "heavy sans" and "regular weight" over font names. Say "sentence case" or "uppercase" explicitly; gpt-image-2 defaults to title case otherwise.
- Say "photorealistic" for any photograph and ask for real texture (skin, fabric, crumbs, grain). Skip "8K", "cinematic", "masterpiece".
- Name where the logo goes. Left unspecified, the model puts it in three places.
- Always close with the negative constraints. Empty space invites invented badges ("Award winning", "#1"), fake star ratings and a second brand name.

## 2. Brand type decides the visual subject

| `brand_type` | Product-anchored templates use | How to attach |
|--------------|-------------------------------|---------------|
| `ecom_store` | The real product | `product_id` from `advibly_get_products`; say "the product from the reference image, reproduced faithfully, same label and colours" |
| `app_ios`, `app_android` | The app icon and real screens | icon comes with the brand kit; pass the `app_screenshot` URLs from `advibly_get_brand` in `reference_image_urls` and say "the app screen from the reference image, reproduced faithfully" |
| `website` (SaaS, services) | A laptop or dashboard, or pure typography | ask for a screenshot, `advibly_upload_asset`, pass as reference; or go type-only (stat, FAQ, founder, competitor callout work with no product shot) |
| `other` | Whatever the brief describes | describe it; consider type-only templates first |

Type-only templates (no product needed): 3 stat callout, 4 review card, 5 testimonial stack, 8 founder message, 10 press mention, 12 numbered list, 13 FAQ card, 14 competitor callout. They work for every brand type.

---

## 3. The fifteen templates

### 1. Headline Statement

Bold one-line claim. Single product hero shot. Minimal background. The headline does all the work.

- **Structure:** one dominant text line (60 percent or more of the visual weight), product image, logo small.
- **Copy slot:** one claim specific enough to stop the scroll.
- **DTC example:** "The last greens powder you'll ever buy."
- **SaaS example:** "Close your books in 3 days, not 3 weeks."
- **App example:** "Log the set before the rest timer ends."
- **Source it from:** the brief's `The One Job`, the slogan, or the most repeated benefit in `Voice of Customer`. A store brand with no brief: the product description's first factual sentence.

```
Static [platform] ad, vertical 4:5. Template: headline statement. One dominant headline carries the ad: "[HEADLINE]" set very large in a heavy sans, sentence case, [two or three] short stacked lines, top half of the frame, left-aligned, taking about sixty percent of the visual weight. Below it the product from the reference image, reproduced faithfully (same label, same colours), standing on a plain flat background following the attached brand kit, soft window light, a gentle contact shadow. Small brand logo bottom-right. Generous margins, nothing else in the frame. Photorealistic product, clean legible typography, the headline rendered exactly once, no watermark, no stray text, no invented badges or logos.
```

### 2. Us vs. Them

Side-by-side comparison. Competitor or "the old way" on the left (grayed out), your product on the right (full colour). Four to five rows.

- **Structure:** two columns, check and cross marks per row, your side visually alive.
- **Copy slot:** comparison rows, each a real differentiator, not filler.
- **DTC example:** "Their multivitamin: 13 ingredients. Ours: 60."
- **SaaS example:** "Spreadsheets: 6 hours a week. Us: 6 minutes."
- **App example:** "Notebook: guess last week's weight. Hevy: it's already there."
- **Source it from:** the brief's `Why Us, Not Them` and `Objections and Rebuttals`, reviews that mention switching. Rows must be things the brand's own materials claim; if the brief does not name the competitor, label the left column "the old way" or the category default.
- **Compliance:** do not render a competitor's logo or packaging. Plain text name only, and only if the brief names them.

```
Static [platform] ad, vertical 4:5. Template: us versus them comparison. Headline across the top: "[HEADLINE]" heavy sans, sentence case, one line. Below it a two-column comparison table. Left column header "[THEM]", the whole column desaturated grey and slightly faded; right column header "[BRAND]", full colour and alive, following the attached brand kit. [Four] rows, each with a cross mark on the left and a check mark on the right: "[ROW 1]", "[ROW 2]", "[ROW 3]", "[ROW 4]". At the bottom, the product from the reference image reproduced faithfully, anchored under the right column. Small brand logo bottom-right. Clean grid, generous spacing, every quoted line rendered exactly once and legibly, no watermark, no stray text, no competitor logos, no invented badges.
```

### 3. Stat Callout

One dominant number takes up 60 percent of the visual. Supporting context below.

- **Structure:** giant stat, one line of context, product or logo anchor.
- **Copy slot:** a real, defensible number. Measurement beats superlative.
- **DTC example:** "97% of users feel a difference in 14 days."
- **SaaS example:** "11 hours saved per rep, per week."
- **App example:** "4.8" with "average rating from 335,000+ App Store ratings".
- **Source it from:** the brief's `Proof`, app-store rating and rating count from `advibly_get_brand`, survey data the brief cites. Keep the denominator in the context line ("of 141 surveyed customers"). Never invent or round up.

```
Static [platform] ad, vertical 4:5. Template: stat callout. A [flat / dark] background following the attached brand kit. One giant number dominates the middle sixty percent of the frame: "[STAT]" in an extremely heavy sans, [high contrast colour]. Under it one line of context in a lighter weight: "[CONTEXT LINE]". [If a rating: five small filled star icons in a row directly beneath the number.] At the bottom centre, [the product from the reference image, small / the app icon from the attached brand kit as a small rounded square with the words "[BRAND]" beside it]. Nothing else in the frame, generous margins. Crisp typography, every quoted line rendered exactly once, no watermark, no stray text, no invented logos.
```

### 4. Review Card

A five-star testimonial styled as a screenshotted product review. Reviewer name, star rating, platform.

- **Structure:** looks like a native review UI. Match where the brand's buyers read reviews: App Store, Trustpilot, Amazon, G2, an on-site review widget.
- **Copy slot:** a real review, verbatim. The artifact's credibility is its realism.
- **DTC example:** a Trustpilot card: "I've tried 6 of these. This is the only one I reordered."
- **SaaS example:** a G2-styled card: "Killed 4 tools and replaced them with this."
- **App example:** an App Store card: "Seriously the best workout tracker I've ever used. Simple. Free. Tons of graphs."
- **Source it from:** `Voice of Customer` entries that are customer reviews (not press), the dossier quote bank, `advibly_get_assets` review screenshots, the user's `inputs/reviews`. If the source has no name, label "Verified buyer". Never attach a face to a named reviewer; use an initial in a circle.

```
Static [platform] ad, vertical 4:5. Template: review card. A flat background following the attached brand kit. Centred, a white rounded card styled like a screenshot of [an App Store review / a Trustpilot review / an on-site product review widget]: five filled [gold] stars at the top, then the review text in a regular weight: "[REVIEW VERBATIM]". Below the text a small circular avatar showing the initial "[X]", the reviewer label "[NAME or Verified buyer]" and a small "[PLATFORM or PRODUCT]" tag. [Behind the card, softly out of focus, the product from the reference image.] Small brand logo bottom-right. Realistic review UI proportions, crisp legible text, every quoted line rendered exactly once, no watermark, no stray text, no invented names or logos.
```

### 5. Testimonial Stack

Three customer quotes arranged vertically, avatar plus name plus one-line quote each.

- **Structure:** three short rows; each quote must scan in two seconds.
- **Copy slot:** three quotes covering different objections or benefits, not the same praise three times.
- **DTC example:** three customers on results, taste and convenience.
- **SaaS example:** three roles (IC, manager, exec) each praising their own outcome.
- **App example:** one on the graphs, one on the social feed, one on progress.
- **Source it from:** reviews, picked for coverage not enthusiasm. Trim a long review to its one best sentence but keep the words the customer used. Initial avatars, not generated faces.

```
Static [platform] ad, vertical 4:5. Template: testimonial stack. Background follows the attached brand kit, clean and high contrast. Headline at the top: "[HEADLINE]" heavy sans, one line. Below it three horizontal rows stacked vertically, each a rounded card with a circular avatar on the left showing the reviewer's first initial, the name in bold and the quote in a regular weight: Row 1 name "[NAME 1]", quote "[QUOTE 1]". Row 2 name "[NAME 2]", quote "[QUOTE 2]". Row 3 name "[NAME 3]", quote "[QUOTE 3]". At the bottom [the brand logo / the app icon from the attached brand kit, small, with the words "[BRAND]" beside it]. Clean spacing, every quoted line rendered exactly once and legibly, no watermark, no stray text, no invented logos.
```

### 6. Before / After

Split image with an arrow between. Transformation framing: product results, workflow, or visual proof.

- **Structure:** two panels, arrow or divider, minimal copy labelling each state.
- **Copy slot:** label the states in the customer's words ("Sunday-night spreadsheet dread" to "Reports send themselves").
- **DTC example:** messy powder-and-shaker counter to one gummy pack.
- **SaaS example:** cluttered six-tab workflow to one clean dashboard.
- **App example:** a blank text prompt to a finished full-length song.
- **Source it from:** the brief's `Before to After` section, transformation language in reviews ("I used to X, now I Y").
- **Compliance:** before-and-after claims are regulated in health, finance and beauty. Default to workflow, ritual or environment transformations; body, skin or financial outcomes only when the user confirms platform policy.

```
Static [platform] ad, vertical 4:5. Template: before and after. [Background] following the attached brand kit. Two panels stacked top and bottom with a bold arrow pointing down between them. Top panel labelled "Before" in small uppercase: [the before state, photorealistic, flat cold light]. Bottom panel labelled "After" in small uppercase: [the after state, warm light; the product from the reference image reproduced faithfully if it is the after]. Headline under the panels: "[HEADLINE]" heavy sans, one line. Small brand logo bottom-right. Crisp legible text, every quoted line rendered exactly once, no watermark, no stray text, no invented logos.
```

### 7. Problem / Solution

Pain point on top (text or image), product as the answer below.

- **Structure:** two zones, tension above, relief below.
- **Copy slot:** the pain in the customer's exact words, then the product's one-line answer.
- **DTC example:** "Tired of choking down swampy greens?" then a gummy pack.
- **SaaS example:** "Your CRM knows nothing about product usage." then an integration screenshot.
- **App example:** "Tossing and turning, checking the clock again?" then the stick pack.
- **Source it from:** the most common pain phrasing in `Voice of Customer` and `Before to After`; verbatim beats paraphrase.

```
Static [platform] ad, vertical 4:5. Template: problem and solution. Top zone, about forty percent of the frame, tension: [photorealistic scene of the problem, flat grey light], with the headline "[PAIN LINE]" in a heavy sans, sentence case, two lines, overlaid. Bottom zone, relief: [bright / warm] background following the attached brand kit, the product from the reference image reproduced faithfully, and one line beneath: "[ANSWER LINE]". Small brand logo bottom-right. Clean split between the zones, crisp legible typography, every quoted line rendered exactly once, no watermark, no stray text, no competitor logos.
```

### 8. Founder Message

Handwritten-style or plain-text note from the founder. Conversational, personal.

- **Structure:** note-style layout, founder name, no product glamour shot.
- **Copy slot:** "I built this because..." one honest paragraph, no marketing polish.
- **DTC example:** "Hey, I made this because every 'healthy' snack was secretly candy."
- **SaaS example:** "I ran RevOps for 6 years. This is the tool I kept wishing existed."
- **Source it from:** the actual founding story, from the user or the dossier. This template collapses if fabricated. Write it as a draft and mark it "needs founder sign-off" in the card; never present invented first-person words as the founder's.
- **Real people:** no generated founder photo. Use their real photo via `advibly_upload_asset`, or no photo at all (the note carries it).

```
Static [platform] ad, vertical 4:5. Template: founder message. A plain note-style layout: an off-white paper card fills most of the frame on a flat background following the attached brand kit. On the card, typed in a simple regular-weight sans like a personal note, this text exactly: "[NOTE, 40 to 70 words]". Signed below in a handwritten-style script: "[FIRST NAME], founder". No product shot, no photo. Small brand logo bottom-right. Crisp legible text, every line rendered exactly once, no watermark, no stray text, no invented logos.
```

### 9. Feature Spotlight (Ingredient Spotlight)

Product hero in the centre, four to five callout boxes around the edges highlighting key components.

- **Structure:** centre image, radiating callouts, each callout three to six words.
- **Copy slot:** the components buyers actually ask about, not the full feature list.
- **DTC example:** the stick pack with a callout per ingredient.
- **SaaS example:** a dashboard screenshot with callouts on the four features reviews mention most.
- **App example:** the phone with callouts on rest timer, graphs, Apple Watch, friends' routines.
- **Source it from:** product descriptions and ingredient lists, app feature lists, the features reviews mention most. Use the ingredient's name; attach a function ("supports sleep") only if the brand's own copy says it.

```
Static [platform] ad, vertical 4:5. Template: ingredient spotlight. [Background] following the attached brand kit. The product from the reference image reproduced faithfully, centred and large. [Five] small rounded callout boxes arranged around the product near the edges, each connected to it by a thin clean line, each with a bold label: "[CALLOUT 1]", "[CALLOUT 2]", "[CALLOUT 3]", "[CALLOUT 4]", "[CALLOUT 5]". Headline at the top: "[HEADLINE]" heavy sans, one line, [with one supporting line beneath: "[SUPPORT LINE]"]. Small brand logo bottom-right. Photorealistic product with soft rim light, crisp legible callouts, every quoted line rendered exactly once, no watermark, no stray text, no invented badges.
```

### 10. Press Mention

"As seen in" with publication names and a pull quote.

- **Structure:** outlet row, one strong quote, product anchor.
- **Copy slot:** a real quote from real coverage.
- **DTC example:** "They look and taste like gummy bears." Glamour.
- **SaaS example:** an analyst or industry-newsletter quote with the outlet named.
- **Source it from:** `Voice of Customer` and `Proof` entries sourced to a publication, the dossier's press section, coverage the user supplies.
- **Compliance:** only outlets that actually covered the brand. Set outlet names as plain wordmarks in text rather than reproducing their logos (logo-usage terms vary).

```
Static [platform] ad, vertical 4:5. Template: press mention. [Background] following the attached brand kit. At the top a small uppercase label "AS SEEN IN" and beneath it a row of publication names set as plain clean wordmarks in dark grey: "[OUTLET 1]", "[OUTLET 2]". In the middle a large pull quote in a heavy [serif / sans]: "[QUOTE]" with the attribution "[OUTLET]" in a lighter weight beneath it. At the bottom, the product from the reference image reproduced faithfully, small, as the anchor, with the brand logo beside it. Generous margins, crisp legible typography, every quoted line rendered exactly once, no watermark, no stray text, no invented publication logos.
```

### 11. Lifestyle Hero

Product in use in a real environment. Minimal copy. Aspirational, not salesy.

- **Structure:** one photograph does the work; a short line and logo at most.
- **Copy slot:** five to eight words, identity-flavoured ("Mornings, handled.").
- **DTC example:** product on a kitchen counter mid-routine.
- **SaaS example:** the tool on-screen in a real work moment (standup, close call, ship day).
- **App example:** the phone propped on a bench between sets.
- **Source it from:** winning ads' visual patterns, identity language in reviews, the brief's `Angles to Explore`.

```
Static [platform] ad, vertical 4:5. Template: lifestyle hero. One photorealistic photograph does the work: [scene, time of day, light], [person described by scale and framing, looking at the product or task, not at the camera, hands doing something concrete], the product from the reference image reproduced faithfully in use. Real texture and small imperfections, candid, unposed, shallow depth of field. One short line in the lower third on a small solid colour block following the attached brand kit: "[LINE]". Small brand logo bottom-right. Every quoted line rendered exactly once, no watermark, no stray text, no invented logos.
```

### 12. Numbered List

"5 reasons [audience] are switching to [brand]." Icons next to each point.

- **Structure:** numbered rows, icon plus short line each, product or logo anchor at the bottom.
- **Copy slot:** each reason a distinct angle: pain, outcome, proof, differentiator, price.
- **DTC example:** "5 reasons runners switched to [brand] this year".
- **SaaS example:** "4 reasons finance teams are leaving [legacy tool]".
- **App example:** "4 reasons creators are building whole albums with Suno".
- **Source it from:** aggregate the most common switching reasons across reviews; the brief's `Why Us, Not Them` and `Proof`. Four rows is safer than five for text fidelity.

```
Static [platform] ad, vertical 4:5. Template: numbered list. [Background] following the attached brand kit. Headline at the top: "[HEADLINE]" heavy sans, sentence case, two lines. Below it [four] numbered rows, each with a large number, a simple line icon and one short line: "1  [REASON 1]", "2  [REASON 2]", "3  [REASON 3]", "4  [REASON 4]". At the bottom, [the product from the reference image, small / the brand logo from the attached brand kit, centred, small]. Clean spacing, crisp legible typography, every quoted line rendered exactly once, no watermark, no stray text, no invented logos.
```

### 13. FAQ Card

A common objection as the question, answered directly.

- **Structure:** question prominent, answer concise, product anchor.
- **Copy slot:** the objection as customers phrase it; the recognition is the hook.
- **DTC example:** "But does it work for sensitive skin? Yes, and here's why."
- **SaaS example:** "Will this survive our security review? SOC 2 Type II, SSO, EU hosting."
- **App example:** "Do I have to pay to get value? No. Logging is free and unlimited."
- **Source it from:** the brief's `Objections and Rebuttals`, `inputs/comments`, the objections people post under the brand's ads. Check the rebuttal against the product's actual tiers before rendering it.

```
Static [platform] ad, vertical 4:5. Template: FAQ card. [Background] following the attached brand kit. A large question fills the upper half, set in a heavy sans with a small "Q" marker: "[QUESTION]". Below it a concise answer with a small "A" marker in a regular weight: "[ANSWER]". At the bottom, [the product from the reference image reproduced faithfully / a smartphone shown straight on, cropped at the bottom edge, its screen showing the app screen from the reference image reproduced faithfully]. Small brand logo bottom-right. Crisp legible typography, every quoted line rendered exactly once, no watermark, no stray text, no invented logos.
```

### 14. Competitor Callout

Name a specific competitor (or the category default) and explain the difference. Bold but factual.

- **Structure:** their name versus yours, one clear axis of difference.
- **Copy slot:** a difference you can defend with facts; comparative claims invite scrutiny.
- **DTC example:** "Like AG1, minus the powder-and-water ritual."
- **SaaS example:** "[Competitor] charges per seat. We don't."
- **Source it from:** `Why Us, Not Them` in the brief, competitor mentions in reviews. Customers name the alternative for you.
- **Compliance:** comparative advertising must be truthful and substantiatable; some platforms restrict naming competitors. Plain text name, never their logo or packaging. If the brief does not name a competitor, use the category default ("greens powders", "your spreadsheet").

```
Static [platform] ad, vertical 4:5. Template: competitor callout. [Background] following the attached brand kit. Headline, very large, heavy sans, two lines: "[HEADLINE NAMING THE COMPETITOR]". The competitor's name set in plain text, no logo. Beneath it one supporting line in a regular weight: "[SUPPORT LINE]". The product from the reference image reproduced faithfully in the lower half, photorealistic, soft daylight. Small brand logo bottom-right. Crisp legible typography, every quoted line rendered exactly once, no watermark, no stray text, no competitor logos or packaging.
```

### 15. Origin Story

Founder or workshop photo with the why-we-built-this narrative. Longer copy than other formats.

- **Structure:** portrait or scene photo, two or three short paragraphs, product secondary.
- **Copy slot:** the specific moment or frustration that started it; specificity is the credibility.
- **DTC example:** "We spent 2 years and 47 batches getting this right. Here's why."
- **SaaS example:** "We were the customer. The tool we needed didn't exist, so we built it."
- **Source it from:** the real story from the user or the dossier. Pairs with warm and retargeting audiences better than cold.
- **Real people:** third person is safest when the founder has not supplied words ("Mark Rober spent years as a NASA engineer before..."). No generated likeness; use the real photo as a reference or a faceless scene (hands, from behind).

```
Static [platform] ad, vertical 4:5. Template: origin story. Top sixty percent: [a photorealistic scene: a person seen from behind / hands at work, the founder's real photo from the reference image], warm light, no invented face. Bottom forty percent on a flat panel following the attached brand kit, a small bold title "[TITLE]" then three short paragraphs in a regular-weight sans, left-aligned: "[PARAGRAPH 1]" then "[PARAGRAPH 2]" then "[PARAGRAPH 3]". Small brand logo bottom-right. Crisp legible text, every quoted line rendered exactly once, no watermark, no stray text, no invented logos.
```

---

## 4. Batch distribution

| Batch size | Per template | Notes |
|-----------:|-------------:|-------|
| 15 | 1 | The default. One pass through the library. |
| 30 | 2 | Second variation changes the brand, product or the source quote, not just the colour. |
| 50 | 3 to 4 | Add the INDEX table at the top of the concept sheet. |

If performance data shows certain templates consistently winning for this brand, shift to 60 percent proven templates and 40 percent full-cycle coverage. Never drop coverage to zero: fatigue is why you are generating regularly, and the template that is tired next month is the one you are scaling today.
