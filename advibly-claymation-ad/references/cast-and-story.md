# Cast and Story: the narrative layer

The counterpart to the two LOOK files (`storyboard-prompts.md` for stills,
`animate-prompts.md` for motion). This covers the STORY layer: what a claymation ad *is* as a
story, the cast-and-continuity sheet you lock before generating, the 8-beat arc, the category
variations, and the narration timing. Read this before writing any beat map.

## What "claymation" means here (and what it doesn't)

The look anchors on **Aardman Animations** (Wallace & Gromit, Chicken Run) and **Laika**
(Coraline, Kubo): hand-sculpted clay and plasticine, visible tool marks, slightly imperfect
armatures, miniature physical sets. Not generic CGI cartoon, not Pixar.

Anchor every story and prompt on these traits:

- **Hand-sculpted clay / plasticine surfaces**: visible fingerprint impressions, sculpting-tool
  marks, slight asymmetry, subtle pinch lines around facial features.
- **Matte clay material**: no Pixar wet-eye sheen, no glossy refraction; clay reads opaque,
  slightly waxy, with soft micro-bumps.
- **Exaggerated, character-driven faces**: oversized noses, deep wrinkles when called for,
  asymmetric eye placement, painted-on or sculpted eyebrows. Characters can be quirky or
  grotesque rather than conventionally pretty.
- **Real knit and felt fabric**: chunky wool sweaters, knit cardigans, felt curtains,
  separately constructed and stitched, not painted on.
- **Wooden and ceramic props**: real-wood tables, hand-thrown ceramic mugs, tin kettles,
  fabric tablecloths (the Aardman miniature-set vibe).
- **Warm tungsten interior light** for domestic scenes; cool fluorescent for office or
  dystopian scenes.
- **Shallow macro depth of field** with creamy bokeh, the soft macro-photography feel that
  sells the miniature-set illusion.
- **Subtle imperfection everywhere**: slightly uneven paint on labels, irregular fabric weave,
  clay surfaces never perfectly smooth.

**Do NOT use these words** in any prompt (they pull the render away from clay):
`Pixar`, `3D rendered`, `digital`, `CGI`, `anime`, `cel-shaded`, `2D`, `painted illustration`,
`realistic photo`, `live action`, `photorealistic`, `smooth render`, `subsurface scattering`,
`ray-traced`. When the selected video model is Seedance, also strip its forbidden words:
`cinematic`, `professional`, `stunning`, `8k`, `studio`, `perfect`.

## The claymation story shape

Claymation ads in this genre are **narrative**: a quirky, warm third-person narrator tells a
small story about a character who has a problem the product quietly solves. The protagonist
drives the whole arc. There is no anthropomorphized-problem character (that is a different,
Pixar-style device). The audience roots for one clay person across the beats, which is what
makes the product feel like a genuine relief rather than a pitch.

The whole ad is a miniature short film that happens to be an ad. Plan all beats up front so the
character, product prop, and miniature set stay identical from beat 1 to the CTA.

## The 8-beat arc (the full story)

| Beat | Length | Purpose | What's on screen |
|---|---|---|---|
| **1. Setup** | 10s | Introduce the protagonist in their everyday world. | Wide or medium shot of the protagonist in their domestic miniature set. The narrator says their name and one defining trait. |
| **2. Inciting moment** | 10s | The protagonist notices the problem. | Close-up of the face as they spot the issue (lines in a mirror, a number on a scale, a sound). Surprised or concerned expression. |
| **3. Social validation** | 10s | Someone else acknowledges the problem, often unintentionally. | Two-character scene: protagonist with a friend, spouse, or coworker in the secondary setting. A small exchange or remark. |
| **4. Quiet despair** | 10s | A solo reflection beat. | Protagonist alone at a window, mirror, or sink, looking at their reflection. No dialogue; the narrator carries it. |
| **5. Clay infographic** | 10s | Explain the mechanism with a clay-sculpted chart. | A hand-sculpted clay infographic on a wall or tablet (for example a "Calcium in skin" chart with clay letters and a plasticine line graph). Static or one small animated indicator. **Optional**: drop it when the product needs no explaining. |
| **6. Discovery** | 10s | The protagonist finds the product. | Close to medium shot of the product as a slightly imperfect clay prop on a wooden table, shelf, or windowsill. The protagonist reaches for it. |
| **7. Transformation** | 10s | Time passes, the product is used, the change is visible. | The protagonist applies or takes the product, then a "weeks later" reveal with a subtle visual improvement (smoother skin, brighter eyes, better posture). |
| **8. Resolution + CTA** | 10s | Confident protagonist with the product, captioned CTA. | Protagonist holds the product, smiling at the camera or another character. Lower third kept clean for the burned-in CTA caption. |

**Total:** 80 seconds of clip duration. If the user wants tighter, use the **5-beat short**
(Setup, Inciting, Discovery, Transformation, CTA), which lands at 50 seconds. Beat 5 is optional
even in the full arc. Use 10 seconds for every clip unless the user explicitly requests a
different duration.

## Category variations

- **Health / supplement** (refirm, ashwagandha, GLP-1): the full 8-beat works well; the clay
  infographic (beat 5) sells the mechanism. Warm domestic palette.
- **Beauty / skincare**: emphasize beat 2 (the mirror) and beat 4 (self-reflection). The
  infographic beat is usually optional; the story is emotional, not mechanistic.
- **Office / B2B** (focus, energy, productivity): the protagonist is in a **cool
  fluorescent-lit office** for beats 1 to 4 (a tired worker at a grey desk), warm light only
  arriving after discovery and transformation. The palette shift is the story.
- **Food / kitchen**: beats 1, 6, and 7 dominate; the social beat (3) becomes a family dinner.
  Warm kitchen light throughout, lots of ceramic and wood.

## The cast-and-continuity sheet (lock this BEFORE generating anything)

Claymation ads usually feature two or three named characters and one or two miniature sets.
Lock all of them up front and reuse the **exact wording** in every prompt; drift compounds
otherwise. Middle-aged and older characters suit the sculpted look best.

```
PROTAGONIST
- Name (used by the narrator): e.g. Diane
- Age range: 30s / 40s / 50s / 60s (the look favors middle-aged and older)
- Distinctive feature: e.g. shoulder-length terracotta-brown wavy plasticine hair in
  ribbon-strands, deep sculpted laugh lines, hooded eyelids
- Build: average / petite / sturdy
- Eye color: e.g. warm brown, sculpted lower lids visible
- Outfit: e.g. cream chunky knit cardigan over a rust-red blouse, dark wool trousers,
  brown leather slippers
- Posture cue: e.g. slight forward lean, soft rounded shoulders

SUPPORTING CHARACTER (beat 3)
- Relationship: best friend / spouse / coworker
- Distinctive feature: e.g. silver curly plasticine hair, round wire glasses,
  sage-green cable-knit sweater
- Age range: similar or older than the protagonist

NARRATOR (voiceover, not visible)
- Voice persona + Advibly voice: warm storytelling (ara) / smooth (sal) /
  wry documentary (leo) / confident (rex) / energetic (eve)
- Tone: gentle observational / wry / matter-of-fact

PRIMARY SETTING (reused for beats 1, 6, 8 to anchor continuity)
- e.g. a small sunlit kitchen with green-painted cabinets, red gingham tablecloth,
  wooden table, copper kettle on the stove, potted herbs on the windowsill

SECONDARY SETTING (beat 3)
- e.g. a neighborhood cafe with potted plants, wooden tables, hanging brass pendant lights

PRODUCT (as a clay prop)
- Render as a clay-stylized prop: matte hand-painted label, slightly imperfect cylinder or
  jar shape, paint that looks hand-applied
- Copy the exact label text from the brand's product photo reference
- Position: on the wooden table / bathroom shelf / kitchen counter

STYLE LOCK (paste verbatim into every image prompt; see storyboard-prompts.md for the canonical text)
```

## Narration voice and timing

- **One narrator voice for the whole ad.** Warm storytelling cadence, a slight pause between
  sentences, mid-pace (never rushed), a slight smile in the voice. Reference the protagonist by
  name when it helps ("Diane noticed..."). The Advibly voice that matches this Aardman
  storyteller tone best is **`ara`** (warm, friendly); `sal` (smooth) and `leo` (authoritative,
  good for a wry documentary narrator) are the alternatives. See `audio-and-gotchas.md`.
- **Write to time.** Roughly 2.5 to 3 words per second. Size each beat's narration to fit its
  clip duration so no clip rides silent under a short line. Rough guide:

  | Narrator words | Spoken duration | Beat clip duration |
  |---|---|---|
  | 18 to 22 | ~6 to 7s | 10s |
  | 23 to 28 | ~8 to 10s | 10s |
  | 29+ | tighten or split across two beats | 10s |

- **The narration lines together are the ad's script.** Write them as one continuous, gentle,
  persuasive story read, not eight disconnected captions. It should make sense read straight
  through, because that is exactly how `advibly_generate_voiceover` renders it in one pass.
- **Sparse character dialogue.** One short, natural line per character, casual, never marketing
  copy. Often the supporting character makes the observation ("You look so rested lately, what
  changed?"). By default fold it into the narrator's read; only split it to a second voice if
  the user asks (see `audio-and-gotchas.md`).

## Cost shape (present before firing, wait for confirmation)

A full 8-beat claymation ad, at the 2-retry cap:

- 8 storyboard stills (gpt-image-2), generated sequentially for identity continuity. Add an
  "after" still only when Seedance is explicitly selected or approved for beat 7's end-frame reveal.
- 8 clips (`gemini-omni-flash`, 10s each by default). Seedance 2.0 is a secondary fallback, also
  at 10 seconds, unless the user explicitly selected it.
- 1 voiceover render (~0.03 credits per 1000 characters) and 1 music track (0.3 credits).
- The stitch is free.

The 5-beat short is roughly 5 stills plus 5 clips. Always present the total credit cost and
wait for explicit confirmation before firing any generation.
