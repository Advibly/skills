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

## Setup

1. Connect the Advibly MCP to Claude: [advibly.com/mcp-setup](https://advibly.com/mcp-setup)
2. Install a skill:
   - **Claude Desktop / Cowork**: Settings → Skills → Install, and select the skill's folder
   - **Claude Code**: copy the skill's folder into `~/.claude/skills/`, e.g.
     ```bash
     cp -R advibly-ugc-ads ~/.claude/skills/
     ```
3. Ask Claude for what you want, e.g. `Make a UGC ad for my brand. Angle: testimonial.`

## Requirements

- An Advibly account with an onboarded brand ([advibly.com/onboarding](https://advibly.com/onboarding))
- The Advibly MCP connected to Claude
- Advibly credits (generation tools spend the account's credits)
