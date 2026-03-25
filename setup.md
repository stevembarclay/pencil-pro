# Pencil Pro — Setup Wizard

This file configures `pencil-pro` for your project. Run it once after installing.

**How to invoke:** In Claude Code, say: `run the pencil-pro setup wizard`

---

## Instructions for Claude

You are running the pencil-pro setup wizard. Follow these steps in order.

### Step 1 — Ask the user to pick a path

Present this choice:

> **How would you like to configure pencil-pro?**
>
> 1. **Preset** — pick from 7 opinionated design systems. Takes 30 seconds.
> 2. **Custom** — answer 6 questions about your own design system.

---

### Step 2A — Preset path

If the user picks preset, show these options:

| # | Preset | Description |
|---|---|---|
| 1 | **Midnight** | Full dark mode — near-black surfaces, electric blue, Inter |
| 2 | **Ember** | Terminal dark — amber accent, JetBrains Mono as primary font, tight spacing |
| 3 | **Grove** | Earthy light — forest green, warm off-white, DM Sans, generous spacing |
| 4 | **Bloom** | Playful — warm rose, white surfaces, Plus Jakarta Sans |
| 5 | **Volt** | Neobrutalist — black borders, electric yellow accent, Space Grotesk |
| 6 | **Material Design 3** | MD3 baseline — purple, tonal surfaces, Roboto *(good if you're already using MD3)* |
| 7 | **Minimal / Neutral** | Grayscale, system fonts *(good if you're bringing your own design system)* |

Once the user picks one, read the corresponding file from `presets/`:
- Option 1 → `presets/midnight.json`
- Option 2 → `presets/ember.json`
- Option 3 → `presets/grove.json`
- Option 4 → `presets/bloom.json`
- Option 5 → `presets/volt.json`
- Option 6 → `presets/material.json`
- Option 7 → `presets/minimal.json`

Load the `tokens` object as your **token map** and the `scaffold` object as your **scaffold values**. Also note the `canvas` values. Skip to Step 3.

---

### Step 2B — Custom path

Ask these questions in order. Wait for each answer before proceeding.

**Question 1 — Canvas resolution**
> What resolution is your primary design target? This should match the main monitor your team designs for.
>
> 1. 1440 × 900 — Laptop
> 2. 1920 × 1080 — Desktop / 24"
> 3. 2560 × 1440 — Large monitor / 27" QHD
> 4. Custom — enter width and height

Store as: `canvasWidth`, `canvasHeight`

**Question 2 — Sidebar**
> Does your app have a persistent sidebar navigation?
>
> 1. Yes — 256px wide (common for SaaS apps)
> 2. Yes — 240px wide
> 3. Yes — custom width
> 4. No sidebar

Store as: `sidebarWidth` (0 if no sidebar)
Compute: `pageAreaWidth = canvasWidth - sidebarWidth`

Also pre-compute content heights for each scaffold (used in Step 4b):
- `contentHeightA = canvasHeight - 144` (Scaffold A: 64px header + 80px stats bar)
- `contentHeightB = canvasHeight - 116` (Scaffold B: 64px header + 52px command strip)
- `contentHeightC = canvasHeight - 120` (Scaffold C: 64px header + 56px action bar)

**Question 3 — Primary / brand color**
> What is your primary brand color? (hex value, e.g. #2563EB)
>
> If you don't have one yet, enter #171717 for a neutral dark.

Store as: `colorPrimary`
Derive: `colorPrimaryDark` — darken by ~10%. If you're not sure, ask the user to provide both or use a slightly darker shade.

**Question 4 — Background style**
> Is your app background light or dark?
>
> 1. Light (white / off-white surface)
> 2. Dark (dark surface, light text)

Based on their answer, set defaults:
- Light: `colorBackground=#FAFAFA`, `colorSurface=#FFFFFF`, `colorBorder=#E5E5E5`, `colorText=#0A0A0A`, `colorTextMuted=#737373`
- Dark: `colorBackground=#0F172A`, `colorSurface=#1E293B`, `colorBorder=#334155`, `colorText=#F8FAFC`, `colorTextMuted=#94A3B8`

Ask: "Would you like to adjust any of these border/background values, or are these defaults fine?"

**Question 5 — Fonts**
> What is your primary sans-serif font? (e.g. Inter, Geist Sans, Roboto, system-ui)

Store as: `fontSans`
Also ask: "Do you have a monospace font? (e.g. JetBrains Mono, Geist Mono — or leave blank for system default)"
Store as: `fontMono` (default: `ui-monospace`)

**Question 6 — Content max width**
> What is the maximum width of your main content area? This is the widest your page content gets before it stops expanding.
>
> 1. 1088px — tight, focused layout
> 2. 1280px — standard wide layout
> 3. 1200px — common SaaS choice
> 4. Custom — enter a value

Store as: `contentMax`
Also ask: "For marketing/landing pages, same max-width or narrower? (default: 768px for copy-heavy content)"
Store as: `marketingContentMax`

**After collecting all answers, build the token map and scaffold values:**

Token map:
```json
{
  "color-primary":        "[colorPrimary]",
  "color-primary-dark":   "[colorPrimaryDark]",
  "color-accent":         "[colorPrimary — user can refine later]",
  "color-background":     "[colorBackground]",
  "color-surface":        "[colorSurface]",
  "color-border":         "[colorBorder]",
  "color-text":           "[colorText]",
  "color-text-muted":     "[colorTextMuted]",
  "color-success":        "#16A34A",
  "color-warning":        "#D97706",
  "color-error":          "#DC2626",
  "font-sans":            "[fontSans]",
  "font-mono":            "[fontMono]",
  "font-serif":           "Georgia",
  "spacing-card":         "24",
  "spacing-gap-sm":       "8",
  "spacing-gap-md":       "16",
  "spacing-gap-lg":       "24",
  "spacing-gap-xl":       "32",
  "content-max-width":    "[contentMax]",
  "sidebar-width":        "[sidebarWidth]"
}
```

Scaffold values:
```
sidebarWidth:        [sidebarWidth]
pageAreaWidth:       [pageAreaWidth]
contentMax:          [contentMax]
marketingContentMax: [marketingContentMax]
sidebarBg:           [colorPrimary — use as dark sidebar, or ask user]
pageBg:              [colorBackground]
surface:             [colorSurface]
border:              [colorBorder]
statsBg:             [colorPrimaryDark or a dark tint of primary]
tableHeaderBg:       [colorBackground]
heroBg:              [colorPrimaryDark]
leftPanelWidth:      [round(contentMax * 0.4)]
rightPanelWidth:     [contentMax - leftPanelWidth]
```

Note on `sidebarBg`: For light-background apps, the sidebar is usually a dark color (primary or near-black). For dark-background apps, it may be even darker than the page. Ask the user if unsure:
> "What color should the sidebar be? Most apps use a dark sidebar even with a light main area. Suggested: [colorPrimaryDark]. Enter a hex or press enter to accept."

---

### Step 3 — Confirm values with the user

Before writing anything, show a summary:

> **Here's your pencil-pro configuration:**
>
> Canvas: [canvasWidth] × [canvasHeight]
> Sidebar: [sidebarWidth]px → [pageAreaWidth]px page area
> Content max: [contentMax]px
>
> **Colors:**
> Primary: [colorPrimary] · Surface: [colorSurface] · Background: [colorBackground]
> Border: [colorBorder] · Text: [colorText]
> Sidebar bg: [sidebarBg]
>
> **Fonts:** [fontSans] / [fontMono]
>
> Does this look right? (yes / adjust [field])

If the user wants to adjust anything, collect the correction and update the value. Show the summary again. Repeat until they confirm.

---

### Step 4 — Write the values into SKILL.md

Once the user confirms, edit `SKILL.md` using the Edit tool. Make these changes:

**4a. Replace the token map JSON** in the "Your token map" section.
Find the code block starting with `"color-primary":` and replace the entire JSON object with the confirmed token values.

**4b. Replace all scaffold placeholders.**

Use this mapping to replace every occurrence in the scaffold scripts:

| Placeholder | Replace with |
|---|---|
| `SCAFFOLD_CANVAS_WIDTH` | canvas width (number) |
| `SCAFFOLD_CANVAS_HEIGHT` | canvas height (number) |
| `SCAFFOLD_SIDEBAR_WIDTH` | sidebarWidth (number) |
| `SCAFFOLD_PAGE_AREA_WIDTH` | pageAreaWidth (number) |
| `SCAFFOLD_CONTENT_MAX` | contentMax (number) |
| `SCAFFOLD_MARKETING_CONTENT_MAX` | marketingContentMax (number) |
| `"SIDEBAR_BG"` | `"[sidebarBg]"` (quoted hex) |
| `"PAGE_BG"` | `"[pageBg]"` (quoted hex) |
| `"SURFACE_COLOR"` | `"[surface]"` (quoted hex) |
| `"BORDER_COLOR"` | `"[border]"` (quoted hex) |
| `"STATS_BG"` | `"[statsBg]"` (quoted hex) |
| `"TABLE_HEADER_BG"` | `"[tableHeaderBg]"` (quoted hex) |
| `"HERO_BG"` | `"[heroBg]"` (quoted hex) |
| `SCAFFOLD_CONTENT_HEIGHT_A` | contentHeightA (number) |
| `SCAFFOLD_CONTENT_HEIGHT_B` | contentHeightB (number) |
| `SCAFFOLD_CONTENT_HEIGHT_C` | contentHeightC (number) |
| `SCAFFOLD_LEFT_PANEL` | leftPanelWidth (number) |
| `SCAFFOLD_RIGHT_PANEL` | rightPanelWidth (number) |

**4c. Update the Primary Resolution Rule section.**
Find the "Configure your primary canvas resolution" note and add a line confirming the configured resolution:
`> Configured: [canvasWidth] × [canvasHeight]`

---

### Step 5 — Offer the first screen

Tell the user:

> **pencil-pro is configured.** Your SKILL.md now has real values for all scaffolds and token maps.
>
> Want to build your first screen right now? I'll open Pencil, scaffold a layout, and show you a screenshot — takes about 60 seconds.
>
> Say **yes** to continue, or **no** to start on your own.

If they say yes: read `onboarding.md` and follow it from Step 1.

If they say no:
> To start designing, say: `build my first screen with pencil-pro`
> To reconfigure at any time, say: `run the pencil-pro setup wizard`

---

## Notes for Claude

- Do not write to SKILL.md until the user confirms the values in Step 3.
- If the user is unsure about any value, suggest the most common option and let them override.
- The setup wizard can be run again at any time — it will overwrite the previous configuration.
- After editing, verify the changes by reading back the relevant sections of SKILL.md.
