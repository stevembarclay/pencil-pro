# Changelog

All notable changes to PencilPlaybook are documented here.

---

## [1.1.0] — 2026-03-24

### Added
- **Perceptual Design Defaults** — science-backed lookup tables in `SKILL.md` for typography (letter spacing, line height, line length), color contrast, spacing scale, motion (duration, easing), and icon sizing. Claude uses these automatically when building components.
- **Onboarding flow** (`onboarding.md`) — guided first screen builder. One question, preflight MCP check, then Claude scaffolds and screenshots a real screen in Pencil in under 2 minutes. 8 tool calls max.

---

## [1.0.0] — 2026-03-23

### Added
- **Setup wizard** (`setup.md`) — 4 preset design systems (Tailwind CSS, shadcn/ui, Material Design 3, Minimal/Neutral) or a 6-question custom path. Writes all values into `SKILL.md`.
- **5 core workflows** — Canvas Archaeology, Design Token Propagation, Canvas Spatial Management, Style Guide Pull, Bulk Property Inspection.
- **4 scaffold archetypes** — Dashboard, List/Queue, Detail/Review, Marketing Page. Ready-to-run `batch_design` scripts.
- **Complete tool reference** (`references/tool-reference.md`) — every parameter for all 12 Pencil MCP tools.
