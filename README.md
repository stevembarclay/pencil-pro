# pencil-pro

> Structured Pencil.dev workflows for Claude Code. Gives Claude a design playbook and science-backed defaults so it stops guessing and starts building intentionally.

<video src="https://github.com/stevembarclay/pencil-pro/releases/download/v1.1.0/demo.mp4" controls width="100%"></video>

---

## Why this exists

When you ask Claude to design something in Pencil without any guidance, it fumbles. It invents colors, picks arbitrary font sizes, sets disabled buttons to 50% opacity because that "feels right," and edits directly into whatever file is open without checking what's already there. The output looks like a Figma template someone downloaded in 2019.

The problem isn't Claude — it's that good design has specific, defensible values that Claude doesn't know unless you tell it:

- Hover states need at least an **8% lightness delta** or they're imperceptible on most monitors
- Disabled elements at **40% opacity** (not 50%) — 50% creates visual competition with active elements; MD3 and Workday converged on 40% after user testing
- Body text on dark backgrounds shouldn't be **pure white** — the halation effect makes it harder to read, not easier. Use `#E2E8F0` or `#F1F5F9`
- Display type at 56px+ needs **negative letter spacing** (−0.03em) because optical counters open up at large sizes
- Touch targets are **44×44px minimum** on mobile, period

These aren't opinions. They're the kind of thing a senior product designer knows from doing it wrong a few times. Without this knowledge, AI averages what it's seen on the internet and produces slop.

pencil-pro embeds those values as lookup tables Claude checks before making any decision. It also gives Claude a structured workflow for working with `.pen` files: explore the canvas before touching it, inject your brand tokens at session start, find empty space before placing a new screen, use bulk replacement tools for token propagation instead of clicking through nodes one by one.

The result: designs that are structurally sound and perceptually intentional — not averaged from what the internet thinks a dashboard should look like.

---

## Quick Start

**Before you begin:** make sure [Pencil.dev](https://pencil.dev) is installed and open, and you have [Claude Code](https://claude.ai/code) installed.

**Step 1 — Install the skill** (pick one):

```bash
# Global — available in every project
git clone https://github.com/stevembarclay/pencil-pro ~/.claude/skills/pencil-pro

# Per-project — only available in this repo
git clone https://github.com/stevembarclay/pencil-pro .claude/skills/pencil-pro
```

**Step 2 — Run setup** (once, takes ~30 seconds):

```
In Claude Code, say: run the pencil-pro setup wizard
```

Pick a preset (Tailwind, shadcn/ui, Material, Minimal) and you're done. Setup writes your design system config into the skill so Claude uses it automatically.

**Step 3 — Use it:**

```
Using pencil-pro, open my-design.pen and tell me what screens are in it.
```

That's it.

---

## What It Does

Five workflows Claude follows automatically when you reference `pencil-pro`:

| Workflow | When to use |
|---|---|
| **Canvas Archaeology** | "What's in this .pen file?" — explore before editing |
| **Design Token Propagation** | A color or font changed — push it across the file |
| **Canvas Spatial Management** | Adding a screen — find where to place it without overlap |
| **Style Guide Pull** | Building a page type you haven't designed before |
| **Bulk Property Inspection** | Audit for consistency before a token replacement |

Four scaffold archetypes — ready-to-run `batch_design` scripts for **Dashboard**, **List/Queue**, **Detail/Review**, and **Marketing Page** layouts.

**Perceptual Design Defaults** — science-backed lookup tables for typography, color contrast, spacing, motion, and icons. Built into the skill so Claude applies them automatically.

**Complete tool reference** for all 12 Pencil MCP tools — every parameter documented.

---

## Preset Design Systems

Five opinionated presets with distinct aesthetics, plus two baseline options if you're already working with an existing system:

| Preset | Primary | Fonts | Spacing | Dark? |
|---|---|---|---|---|
| **Midnight** | Electric blue (#58A6FF) | Inter / JetBrains Mono | Standard | Yes — full dark |
| **Ember** | Amber (#F59E0B) | JetBrains Mono / JetBrains Mono | Tight (6/12/20) | Yes — near-black |
| **Grove** | Forest green (#2D6A4F) | DM Sans / JetBrains Mono | Generous (12/20/32/48) | No — warm off-white |
| **Bloom** | Rose (#F43F5E) | Plus Jakarta Sans / JetBrains Mono | Standard | No — white |
| **Volt** | Near-black + yellow (#FACC15) | Space Grotesk / JetBrains Mono | Standard | No — white, black borders |

**Already have a design system?**

| Preset | Primary | Fonts | |
|---|---|---|---|
| **Material Design 3** | Purple (#6750A4) | Roboto / Roboto Mono | MD3 baseline |
| **Minimal / Neutral** | Near-black (#171717) | system-ui / ui-monospace | Blank slate |

Or answer 6 questions during setup to configure your own.

---

## Example Prompts

```
Using pencil-pro, propagate the color-primary change from #2563EB to #1D4ED8 across onboarding.pen.
```

```
Using pencil-pro, scaffold a new List screen for the user management page.
```

```
Using pencil-pro, add a second screen to settings-v1.pen showing the edit state.
```

---

## File Structure

```
.claude/skills/pencil-pro/
├── SKILL.md                   # Workflows, scaffolds, token map, session checklist
├── setup.md                   # Setup wizard — run once to configure
├── presets/
│   ├── tailwind.json
│   ├── shadcn.json
│   ├── material.json
│   └── minimal.json
└── references/
    └── tool-reference.md      # Full parameter docs for all 12 Pencil MCP tools
```

---

## Contributing

Issues and PRs welcome. Particularly useful contributions:

- Additional presets (Bootstrap, Ant Design, Chakra UI)
- Additional scaffold archetypes (modal, wizard/stepper, mobile screen)
- Workflow additions (multi-file token sync, component extraction)
- Tool reference updates when Pencil MCP adds new tools

---

## License

MIT — see [LICENSE](LICENSE).
