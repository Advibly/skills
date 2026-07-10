# Advibly Skills

Claude skills that drive the [Advibly](https://advibly.com) MCP to generate ad creative end to end: on-brand images, UGC-style video ads, talking-actor videos, and carousels.

Each folder under `skills/` is one self-contained skill:

```
advibly-skills/
└── skills/
    └── <skill-name>/
        ├── SKILL.md          # the skill itself
        └── references/       # supporting docs the skill loads as needed
```

## Skills

| Skill | What it does |
|-------|--------------|
| [`advibly-ugc-ads`](./skills/advibly-ugc-ads/) | Generates a complete multi-shot UGC video ad from a brand plus an ad angle: a realistic AI creator image (gpt-image-2), a 5-shot direct response script, a user-approved storyboard of start frames, one vertical clip per shot animated from its frame with native spoken dialogue (Gemini Omni Flash), then assembles the clips into a finished ad. Five preset angles: testimonial, car/on-the-go, unboxing, lifestyle demo, problem-solution. |

## Setup

1. Connect the Advibly MCP to Claude: [advibly.com/mcp-setup](https://advibly.com/mcp-setup)
2. Install a skill. The quickest way, for any supported agent (Claude Code, Claude Desktop, Cursor, and more):
   ```bash
   npx skills add Advibly/advibly-skills
   ```
   This downloads the skill from this repo and wires it into your agent. To pick a specific skill in a multi-skill repo, pass `-s <skill-name>`, e.g. `npx skills add Advibly/advibly-skills -s advibly-ugc-ads`.

   Prefer to install by hand?
   - **Claude Desktop / Cowork**: Settings → Skills → Install, and select the skill's folder under `skills/`
   - **Claude Code**: copy the skill's folder into `~/.claude/skills/`, e.g.
     ```bash
     cp -R skills/advibly-ugc-ads ~/.claude/skills/
     ```
3. Ask Claude for what you want, e.g. `Make a UGC ad for my brand. Angle: testimonial.`

## Requirements

- An Advibly account with an onboarded brand ([advibly.com/onboarding](https://advibly.com/onboarding))
- The Advibly MCP connected to Claude
- Advibly credits (generation tools spend the account's credits)
