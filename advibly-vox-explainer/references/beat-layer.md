# Beat / Shot Layer: narrative arc + per-shot spec (the STORY layer)

The counterpart to `prompt-guide.md` (which covers the LOOK layer). This covers the beat
layer: the narrative skeleton across the whole ad plus the per-shot spec (shot size, camera
move, element motion). Distilled from short-form, story-structure, and editing research
(sources at bottom). One poster per shot; two shots per beat.

Three tiers, three control methods (this is the design):

- **Narrative arc**: preset menu. AI recommends from the claim, the user confirms.
- **Per-beat scene, headline, narration**: AI drafts, the user approves or edits. This is the
  one mandatory gate.
- **Shot size + camera move**: rule-derived from arc position plus anti-monotony. AI fills
  from a hard-constrained vocabulary; the user can override but rarely needs to. Never
  free-form (a free model will emit "orbit" or "dolly zoom" and break the flat art).

---

## 1. Narrative arc library (pick one; recommend by claim)

| Arc (token) | When to use | Beat shape |
|---|---|---|
| `hook_payoff` | default for any single idea; safest | Hook > Context > Build > Payoff > Button |
| `pas` | pain-aware product ads, urgency | Problem > Agitate > Solve (product reveal) > Proof > CTA |
| `bab` | when the "after" sells better than the pain | Before > After > Bridge (product reveal) > CTA |
| `aida` | cold-audience paid ads | Attention > Interest > Desire > Action |
| `storybrand` | brand or service, customer as hero | Hero wants > Problem > Guide (brand) > Plan > CTA > stakes |
| `how_it_works` | product, process, or system explainer | Hook > What it is > 2-3 shown steps > Benefit > CTA |
| `timeline` | history, "evolution of", journeys | Start > event > event > turning point > present > takeaway |
| `man_in_hole` | case study, comeback, transformation (highest-rated arc) | OK > fall > deepen > climb out > better than before |
| `story_spine` | mission, brand, founder tales | Once... > Every day... > Until one day... > Because... > Until finally... > Ever since... |
| `origin` | founder, "why we exist" | World > spark > leap > struggle > breakthrough > today |
| `myth_buster` | correct a misconception | FACT first > the myth > expose the fallacy > what to believe > CTA |
| `listicle` | tips, tools, rankings, "N ways to..." | Promise > item > item > ... > number one > recap/CTA |
| `three_act` | any narrative 60s piece | Setup > Confrontation (rising) > Resolution |
| `story_circle` | character-driven brand story | You > Need > Go > Search > Find > Take > Return > Change |

**Claim-to-arc heuristic:** product or service ad > `pas` / `bab` / `aida` / `storybrand`;
concept or system > `how_it_works` / `hook_payoff`; historical or "evolution of" >
`timeline`; transformation or case study > `man_in_hole`; brand or mission > `story_spine` /
`origin`; correcting a belief > `myth_buster`; ranking or tips > `listicle`.

**The product-reveal beat (ad-specific).** In a product ad, one beat is the turn where the
real product photo arrives, hero-framed (paper burst, converging torn arrows, glow pulse).
It sits where the arc pivots: the Solve in `pas`, the Bridge in `bab`, the Desire in `aida`,
the Guide in `storybrand`, the "what it is" in `how_it_works`. Beats before it carry
`"product": false`; the reveal and everything after usually carry `true`. A no-product brand
explainer just skips this mapping.

## 2. Hook, pacing, beat count (firm presets)

- **Hook in 3s or less.** Roughly 65 percent of viewers decide by 3 seconds. Beat 1's baked
  headline must carry the payoff promise (bold claim, provocative question, surprising stat,
  "you're doing X wrong"). Never spend beat 1 on setup.
- **Hook patterns (pick one for beat 1):** `mistake_callout` · `pain_point` ·
  `surprising_stat` · `direct_question` · `urgent_warning` · `secret_reveal` ·
  `experiment_story` · `pattern_interrupt` · `outcome_tease`. Underlying triggers: curiosity
  gap, pattern interrupt, bold claim.
- **Beat count and length:**

  | Duration | Beats | Shots (posters) | Shot length | VO words |
  |---|---|---|---|---|
  | **~30s** | 3-4 | 6-8 | 4-5s | ~70-80 |
  | **~60s** | 5-6 | 10-12 | 5-6s | ~130-150 |

  60s is the practical ceiling: the stitcher takes 12 clips per pass.
- **Proportions:** hook 1-3s > body 70-80 percent > payoff 10-20 percent > end/CTA 0-2s.
- **Change something visually every 3-5s; never hold one poster past 7s.** This is why beats
  split into two short shots.
- **Endings:** `hard_cut` (on the payoff; default, drives rewatches) · `quick_cta` (2s or
  less, one action plus benefit, 3-5 words) · `loop_close` (last line mirrors the first for
  a seamless replay).

## 3. Shot-pattern library (per beat)

### Shot size (composition zoom of the poster)

`EST_WIDE` (whole scene or system; orient, scale) · `WIDE` (subject in environment) ·
`MEDIUM` (one subject centered; the workhorse) · `CLOSE` (one detail fills the frame;
emphasis, emotion) · `DETAIL` (a single texture, word, or number; a punch beat).

Coverage plays out **across beats**: move-in `EST_WIDE > MEDIUM > CLOSE` builds intensity and
peaks on CLOSE; move-out `CLOSE > ... > WIDE` reveals context, good for endings. The best
two-shot beat is **establishing wide > detail cut-in**: a wide to orient (headline on),
hard cut to a CLOSE of the key element (headline off).

### Camera move: HARD-CONSTRAINED flat-safe vocabulary

Rule: **uniform translate plus uniform scale is safe** (text scales and moves as one piece);
anything that warps perspective, blurs, or rotates text off-axis will smear the flat art.

| Safe (token) | Realize on flat art | Job | i2v phrasing |
|---|---|---|---|
| `static` | no transform, tiny element float | let a stat or quote land | "locked-off static, only subtle paper flutter, text fixed" |
| `push_in` | uniform scale-up (Ken Burns) | tension, focus | "very slow push in, text stays sharp and centered" |
| `pull_out` | uniform scale-down | reveal, big picture | "slow pull out revealing the full scene, flat, steady" |
| `pan` | translate across an over-wide poster | read a list or timeline | "slow horizontal pan, flat 2D, no perspective shift" |
| `tilt` | vertical translate | reveal scale, countdown | "slow vertical tilt, flat parallax, steady" |
| `parallax` | fg/mid/bg layers at different speeds | the "living paper" signature | "subtle multi-layer parallax, paper layers drift at different speeds, 2.5D, flat" |
| `element` | one cut-out slides or hinges in, rest still | introduce or emphasize one item | "one paper element slides in from the edge, others static, stop-motion feel" |

**Bold / experimental (available, not banned):** `orbit`, `dolly_zoom`, `roll`, `whip`. They
tend to warp flat art and smear text, but they are a style choice: use them with
`constraints: loose`, re-roll until a take lands, and save them for a beat where the effect
earns it. In `strict` mode the stability guards will fight them; that is the point of the
mode. If you just want a dolly-in's feel cleanly, a 2D `push_in` scales text as one piece and
never warps.

### Element motion (what moves inside; a SEPARATE axis from camera). THE ENERGY ENGINE.

This is where dynamism actually comes from, and rich motion is safe. A single collage
keyframe animated with "the goat bobs, both traders gesture, coins and shells scatter, AND a
paper bird flaps across the whole frame" stays perfectly flat with text intact, and looks
great. So do not limit motion to one gentle verb. `element_motion` is the AI's per-beat
creative call: write what actually fits THIS scene, and make it rich (multiple things moving
at once). Shot size gates it: WIDE means several elements move; CLOSE means that one thing
animates strongly.

- **Encouraged (safe and punchy):** multiple elements moving at once · coins, petals, scraps
  scattering and bursting · elements popping, sliding, flapping, hinging in · drift · sway ·
  flutter · pulse · settle · confetti and torn scraps drifting down across the frame.
- **A hero traveling element** (a bird, plane, coin, arrow flying across the frame) is a
  great occasional punch on a key beat, NOT every shot. A flyer in every frame reads as a
  formula. Let most beats just move their own elements richly.
- **The dramatic moves** (assemble-from-empty, confetti bursts, an impact shake, a whip) are
  the same axis pushed harder, phrased for the video model. Full recipes in
  `motion-collage.md`. Save them for the beats that earn a punch: the hook, the product
  reveal, the payoff, the ending. A film that fires every trick on every shot reads as a
  formula; the strongest reference films used the big moves once or twice.
- **The only real limits:** keep it rigid paper (cut-outs slide, flap, scatter; no organic
  melting, no `morph`, no `warp`, no body horror), keep the text stable, keep it flat 2D.
- Camera and element motion are independent: one camera move plus as much element motion as
  the scene wants.

### Anti-monotony (the biggest quality lever)

Every beat is a similar-looking poster, so **no two adjacent beats use the same camera move;
alternate families (scale, translate, static); reserve `static` for the payoff or quote
beat** so the motion drop signals "this is the point."

**Move-rhythm presets (drop in per arc):**

- `hook_payoff` (8 shots): push_in > pan > parallax > static > push_in > tilt > pull_out > static
- `pas` / `bab` (6 shots): push_in > static > pan > parallax > push_in > static
- `listicle`: the same move on every item (pan or tilt) but flip direction or add parallax on
  the number-one item; static on the recap
- `timeline`: pan the same direction beat to beat (moving through time) > push_in on the
  turning point > pull_out on the takeaway

**Signature opener and transitions (optional punch).** The hook beat can build itself with an
**assemble-from-empty** open (a bare paper field where the cut-outs fly in and snap into the
first poster), which announces the collage medium in the first second. A **whip** works well
as an occasional between-shot transition. Both are motion-prompt techniques, not free camera
moves; see `motion-collage.md`. Use each sparingly.

## 4. Vocab tokens (copy-paste)

```
ARCS:  hook_payoff pas bab aida storybrand how_it_works timeline man_in_hole
       story_spine origin myth_buster listicle three_act story_circle
HOOKS: mistake_callout pain_point surprising_stat direct_question urgent_warning
       secret_reveal experiment_story pattern_interrupt outcome_tease
SIZES: EST_WIDE WIDE MEDIUM CLOSE DETAIL
MOVES: static push_in pull_out pan tilt parallax element
       (bold, loose-mode only: orbit dolly_zoom roll whip)
ENDINGS: hard_cut quick_cta loop_close
BEATS: 30s > 6-8 shots @4-5s (~75w) · 60s > 10-12 shots @5-6s (~140w) · hook <=3s ·
       change every 3-5s · never past 7s per shot
```

## Sources (key)

Story structure: StudioBinder (three-act, story circle, story structure), Pixar story spine
(tckpublishing, sessionlab), Vonnegut shapes plus the Cornell six-arc study
(technologyreview). Ad frameworks: AIDA/PAS/BAB (soarai, swiftcopy), StoryBrand
(innatemarketinggenius), per-second ad timing (benly.ai). Explainer and doc craft: Vox case
studies (jasperpictures, storybench), mypromovideos, Ken Burns (masterclass). Hook, pacing,
retention: go-viral.app, prepublish.ai, teleprompter.com, socialync.io, vidpros.com (clip
length). Shots, coverage, editing: StudioBinder shot sizes and camera movements,
learnaboutfilm coverage and sequence, insidetheedit (pacing).
