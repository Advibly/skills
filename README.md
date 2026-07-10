# Advibly Skills

Claude skills that drive the [Advibly](https://advibly.com) MCP to generate ad creative end to end: on-brand images, UGC-style video ads, talking-actor videos, and carousels.

Each folder in this repo is one self-contained skill:

```
advibly-skills/
└── <skill-name>/
    ├── SKILL.md          # the skill itself
    └── references/       # supporting docs the skill loads as needed
```

## Skills

| Skill | What it does |
|-------|--------------|
| [`advibly-ugc-ads`](./advibly-ugc-ads/) | Generates a complete multi-shot UGC video ad from a brand plus an ad angle: a realistic AI creator image (gpt-image-2), a 5-shot direct response script, a user-approved storyboard of start frames, one vertical clip per shot animated from its frame with native spoken dialogue (Gemini Omni Flash), then assembles the clips into a finished ad. Five preset angles: testimonial, car/on-the-go, unboxing, lifestyle demo, problem-solution. |
| [`advibly-collage-motion`](./advibly-collage-motion/) | Decode-then-animate pipeline for halftone paper-collage and stop-motion-graphic ads. Reverse-engineers a reference image into a field-editable JSON spec, generates on-brand stills with gpt-image-2 (store products locked via catalog photo references), then animates them into a default 4-scene set of 8s assemble-from-empty clips with Gemini Omni Flash (empty color field, cut-out pieces slide in and snap into place, native audio). Labels are burned in at generation, faithful to the decoded color field by default. |

## Install

First, connect the Advibly MCP to Claude: [advibly.com/mcp-setup](https://advibly.com/mcp-setup).

Then install a skill. `npx skills` works for any supported agent (Claude Code, Claude Desktop, Cursor, and more) and pulls straight from this GitHub repo:

**advibly-ugc-ads**
```bash
npx skills add Advibly/advibly-skills -s advibly-ugc-ads
```

**advibly-collage-motion**
```bash
npx skills add Advibly/advibly-skills -s advibly-collage-motion
```

Install both at once:
```bash
npx skills add Advibly/advibly-skills
```

### Manual install

Prefer to install by hand?

- **Claude Desktop / Cowork**: Settings → Skills → Install, and select the skill's folder.
- **Claude Code**: clone the repo and copy the skill folder into `~/.claude/skills/`:
  ```bash
  git clone https://github.com/Advibly/advibly-skills.git
  cp -R advibly-skills/advibly-ugc-ads ~/.claude/skills/
  cp -R advibly-skills/advibly-collage-motion ~/.claude/skills/
  ```

Then ask Claude for what you want, e.g. `Make a UGC ad for my brand. Angle: testimonial.` or `Reverse-engineer this collage reference and animate it.`

## Requirements

- An Advibly account with an onboarded brand ([advibly.com/onboarding](https://advibly.com/onboarding))
- The Advibly MCP connected to Claude
- Advibly credits (generation tools spend the account's credits)
