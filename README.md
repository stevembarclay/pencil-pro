# pencil-pro

> Advanced Pencil.dev workflows for Claude Code — design token propagation, canvas archaeology, bulk property replacement, spatial management, and complete MCP tool reference.

A Claude Code skill that gives Claude structured, opinionated knowledge of the [Pencil.dev MCP server](https://pencil.dev). Instead of fumbling through tool calls, Claude follows proven workflows: how to explore a `.pen` file before touching it, how to propagate a token change across an entire file, how to place new screens without overlap, and how to scaffold app and marketing layouts from scratch.

Comes with a **setup wizard** that configures the skill for your design system in under a minute — pick a preset (Tailwind, shadcn/ui, Material, Minimal) or walk through 6 questions to configure your own.

---

## What's Inside

**5 core workflows:**

| Workflow | Use when |
|---|---|
| Canvas Archaeology | "What's in this .pen file?" — explore before editing |
| Design Token Propagation | A color or font changed — push it across the file |
| Canvas Spatial Management | Adding a new screen — find where to put it |
| Style Guide Pull | Building a page type you haven't designed before |
| Bulk Property Inspection | Audit for consistency before a token replacement |

**4 scaffold archetypes** — ready-to-run `batch_design` scripts for Dashboard, List/Queue, Detail/Review, and Marketing Page layouts.

**4 design system presets** — Tailwind CSS, shadcn/ui, Material Design 3, Minimal/Neutral.

**Complete tool reference** for all 12 Pencil MCP tools — every parameter documented.

---

## Prerequisites

### 1. Pencil.dev

Install the Pencil desktop app from **[pencil.dev](https://pencil.dev)**. The Pencil MCP server starts automatically when Pencil is running — there is no separate MCP install step.

To verify it's working, open Claude Code and ask:
```
get_editor_state()
```
If you see a response about the active document (or "no document open"), you're connected.

### 2. Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

---

## Installation

Clone into your project's `.claude/skills/` directory:

```bash
git clone https://github.com/YOUR_USERNAME/pencil-pro .claude/skills/pencil-pro
```

Or if you prefer copying files manually:

```bash
mkdir -p .claude/skills/pencil-pro/references .claude/skills/pencil-pro/presets
# copy SKILL.md, setup.md, references/tool-reference.md, presets/*.json
```

The `.claude/skills/` directory is per-project. If you want `pencil-pro` available across all projects, use `~/.claude/skills/` instead.

---

## Setup (required before first use)

After installing, run the setup wizard to configure the skill for your design system:

```
In Claude Code, say: run the pencil-pro setup wizard
```

The wizard will:
1. Offer 4 preset design systems (30-second fast path)
2. Or walk you through 6 questions for a custom configuration
3. Show you a summary and ask for confirmation
4. Write all values into `SKILL.md` — canvas resolution, sidebar width, token map, scaffold dimensions

**You only need to run setup once.** Re-run it any time your design system changes.

---

## Usage

Once setup is complete, reference `pencil-pro` in your Claude Code prompts:

```
Using pencil-pro, open docs/mockups/dashboard-v1.pen and tell me what screens are in it.
```

```
Using pencil-pro, propagate the color-primary change from #2563EB to #1D4ED8 across docs/mockups/onboarding.pen.
```

```
Using pencil-pro, scaffold a new List screen for the user management page.
```

```
Using pencil-pro, add a second screen to docs/mockups/settings-v1.pen showing the edit state.
```

---

## Preset Design Systems

| Preset | Primary | Fonts | Canvas |
|---|---|---|---|
| **Tailwind CSS** | Blue (#2563EB) | Inter / JetBrains Mono | 1440×900 |
| **shadcn/ui** | Zinc near-black (#18181B) | Geist Sans / Geist Mono | 1440×900 |
| **Material Design 3** | Purple (#6750A4) | Roboto / Roboto Mono | 1440×900 |
| **Minimal / Neutral** | Near-black (#171717) | system-ui / ui-monospace | 1440×900 |

All presets start at 1440×900 (laptop-first). You can override the canvas size during setup.

---

## Scaffold Archetypes

| Scaffold | Layout type |
|---|---|
| **A — Dashboard** | Metrics, KPIs, activity feeds, monitoring |
| **B — List / Queue** | Tables, asset libraries, queues, user lists |
| **C — Detail / Review** | Two-panel detail views, review flows |
| **D — Marketing Page** | Landing pages — 1440px regardless of app canvas |

---

## How It Works

Claude Code skills are Markdown files that load into Claude's context when invoked. `pencil-pro` is structured as:

```
.claude/skills/pencil-pro/
├── SKILL.md                   # Workflows, scaffolds, token map, session checklist
├── setup.md                   # Setup wizard — run once to configure
├── presets/
│   ├── tailwind.json          # Tailwind CSS preset values
│   ├── shadcn.json            # shadcn/ui preset values
│   ├── material.json          # Material Design 3 preset values
│   └── minimal.json           # Minimal/Neutral preset values
└── references/
    └── tool-reference.md      # Full parameter docs for all 12 Pencil MCP tools
```

When you reference `pencil-pro` in a prompt, Claude loads `SKILL.md` and follows the workflows defined there. During setup, Claude reads `setup.md` and the relevant preset file, then edits `SKILL.md` with your actual values.

---

## Contributing

Issues and PRs welcome. Particularly useful contributions:

- Additional preset design systems (Bootstrap, Ant Design, Chakra UI)
- Additional scaffold archetypes (modal, wizard/stepper, mobile screen)
- Workflow additions (multi-file token sync, component extraction)
- Tool reference updates when Pencil MCP adds new tools

---

## License

MIT — see [LICENSE](LICENSE).
