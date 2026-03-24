---
name: pencil-pro
description: "Advanced Pencil.dev workflows for Claude Code. Covers: design token propagation across .pen files, canvas archaeology (batch_get), bulk property replacement, canvas spatial management, and all 12 Pencil MCP tools. Use when: updating existing .pen files, propagating a token change, understanding what's in a .pen file, managing multi-file design system consistency, or scaffolding new app or marketing screens."
version: 1.0.0
---

# Pencil Pro

## Primary Resolution Rule

**This skill must be configured before first use.** Run the setup wizard:
> In Claude Code: `run the pencil-pro setup wizard`

The wizard sets your canvas resolution, sidebar dimensions, and design tokens. After setup, the values below will be filled in with real numbers.

> Configured resolution: *(run setup to set this)*

If a screen root in an existing `.pen` file doesn't match your configured resolution, note it before editing. Do not build on top of a wrongly-sized canvas without flagging the deviation.

**Common exception — marketing pages:** Design at 1440px regardless of your app resolution. Marketing pages are consumed at variable browser widths — design at 1440px, verify at 390px.

---

## Session Startup (Every Pencil Session)

```
1. get_editor_state()                           — check active document and selection
2. open_document("path/to/your-file.pen")       — open target file
3. get_guidelines(topic="web-app")              — load design guidelines
4. set_variables(<token map below>)             — inject brand tokens
5. get_variables()                              — confirm tokens stored
6. snapshot_layout()                            — verify canvas dimensions
```

**Your token map** — run `set_variables` with this object at the start of every session. Variables may not persist between Claude Code sessions.

```json
{
  "color-primary":        "YOUR_HEX",
  "color-primary-dark":   "YOUR_HEX",
  "color-accent":         "YOUR_HEX",
  "color-background":     "YOUR_HEX",
  "color-surface":        "YOUR_HEX",
  "color-border":         "YOUR_HEX",
  "color-text":           "YOUR_HEX",
  "color-text-muted":     "YOUR_HEX",
  "color-success":        "YOUR_HEX",
  "color-warning":        "YOUR_HEX",
  "color-error":          "YOUR_HEX",
  "font-sans":            "YOUR_FONT",
  "font-mono":            "YOUR_FONT",
  "font-serif":           "YOUR_FONT",
  "spacing-card":         "24",
  "spacing-gap-sm":       "8",
  "spacing-gap-md":       "16",
  "spacing-gap-lg":       "24",
  "spacing-gap-xl":       "32",
  "content-max-width":    "YOUR_VALUE",
  "sidebar-width":        "YOUR_VALUE"
}
```

After `set_variables`, call `get_variables()` to confirm all tokens are stored.

---

## Scaffold Scripts

Four scaffold scripts for the most common layout archetypes. Run the setup wizard to fill in the dimension and color values below before using.

---

### Scaffold A — App Dashboard
*Metrics, KPIs, activity feed, monitoring*

```
sidebar=I("root", {name:"Sidebar", width:SCAFFOLD_SIDEBAR_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, bg:"SIDEBAR_BG", display:"flex", flexDirection:"column"})
pageArea=I("root", {name:"PageArea", width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, x:SCAFFOLD_SIDEBAR_WIDTH, bg:"PAGE_BG"})
pageHeader=I("pageArea", {name:"PageHeader", width:SCAFFOLD_PAGE_AREA_WIDTH, height:64, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", px:32})
statsBar=I("pageArea", {name:"StatsBar", y:64, width:SCAFFOLD_PAGE_AREA_WIDTH, height:80, bg:"STATS_BG", display:"flex", alignItems:"center", gap:24, px:32})
contentWrap=I("pageArea", {name:"ContentWrap", y:144, width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CONTENT_HEIGHT_A, display:"flex", justifyContent:"center", p:32})
contentInner=I("contentWrap", {name:"ContentInner", width:SCAFFOLD_CONTENT_MAX, display:"flex", flexDirection:"column", gap:24})
```

After scaffold: `snapshot_layout()` — verify pageArea.width and contentInner.width are correct.

---

### Scaffold B — App List / Queue
*Tables, asset libraries, queues, user management*

```
sidebar=I("root", {name:"Sidebar", width:SCAFFOLD_SIDEBAR_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, bg:"SIDEBAR_BG"})
pageArea=I("root", {name:"PageArea", width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, x:SCAFFOLD_SIDEBAR_WIDTH, bg:"PAGE_BG"})
pageHeader=I("pageArea", {name:"PageHeader", width:SCAFFOLD_PAGE_AREA_WIDTH, height:64, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", px:32})
commandStrip=I("pageArea", {name:"CommandStrip", y:64, width:SCAFFOLD_PAGE_AREA_WIDTH, height:52, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", gap:16, px:32})
tableWrap=I("pageArea", {name:"TableWrap", y:116, width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CONTENT_HEIGHT_B, display:"flex", justifyContent:"center", px:32, pt:24})
tableInner=I("tableWrap", {name:"TableInner", width:SCAFFOLD_CONTENT_MAX, display:"flex", flexDirection:"column", gap:0})
tableHeader=I("tableInner", {name:"TableHeader", width:SCAFFOLD_CONTENT_MAX, height:40, bg:"TABLE_HEADER_BG", borderBottom:"1px solid BORDER_COLOR", display:"flex", alignItems:"center"})
```

---

### Scaffold C — App Detail / Review
*Two-panel detail views, review flows, side-by-side layouts*

```
sidebar=I("root", {name:"Sidebar", width:SCAFFOLD_SIDEBAR_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, bg:"SIDEBAR_BG"})
pageArea=I("root", {name:"PageArea", width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, x:SCAFFOLD_SIDEBAR_WIDTH, bg:"PAGE_BG"})
pageHeader=I("pageArea", {name:"PageHeader", width:SCAFFOLD_PAGE_AREA_WIDTH, height:64, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR"})
actionBar=I("pageArea", {name:"ActionBar", y:64, width:SCAFFOLD_PAGE_AREA_WIDTH, height:56, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR"})
contentWrap=I("pageArea", {name:"ContentWrap", y:120, width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CONTENT_HEIGHT_C, display:"flex", justifyContent:"center"})
contentInner=I("contentWrap", {name:"ContentInner", width:SCAFFOLD_CONTENT_MAX, height:SCAFFOLD_CONTENT_HEIGHT_C, display:"flex", gap:0})
leftPanel=I("contentInner", {name:"LeftPanel", width:SCAFFOLD_LEFT_PANEL, height:SCAFFOLD_CONTENT_HEIGHT_C, bg:"SURFACE_COLOR", borderRight:"1px solid BORDER_COLOR"})
rightPanel=I("contentInner", {name:"RightPanel", width:SCAFFOLD_RIGHT_PANEL, height:SCAFFOLD_CONTENT_HEIGHT_C, bg:"PAGE_BG", p:24, display:"flex", flexDirection:"column", gap:20})
```

Note: left/right split defaults to ~40/60. Adjust per feature — common splits: 40/60, 38/62, 50/50.

---

### Scaffold D — Marketing Page
*Landing pages, product pages — always 1440px wide regardless of app canvas*

```
navbar=I("root", {name:"Navbar", width:1440, height:72, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", px:80})
heroSection=I("root", {name:"HeroSection", y:72, width:1440, height:600, bg:"HERO_BG", display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center"})
heroContent=I("heroSection", {name:"HeroContent", width:SCAFFOLD_MARKETING_CONTENT_MAX, display:"flex", flexDirection:"column", alignItems:"center", gap:24})
section1=I("root", {name:"Section1", y:672, width:1440, height:480, bg:"SURFACE_COLOR", display:"flex", flexDirection:"column", alignItems:"center", py:80})
section1Content=I("section1", {name:"Section1Content", width:SCAFFOLD_MARKETING_CONTENT_MAX})
```

Marketing pages are mobile-first. Design at 1440px and verify the layout at 390px width before handoff.

---

## Core Workflows

### Workflow 1: Canvas Archaeology

Use when: "what's in this .pen file?" or before editing a file you haven't worked with recently.

```
1. open_document("path/to/file.pen")
2. get_editor_state()                         — get root node IDs
3. batch_get(patterns=["*"])                  — read full node tree
4. snapshot_layout()                          — see computed dimensions
```

Read the `batch_get` output to map: what screens exist, what dimensions, what tokens are in use, what's named clearly vs. not. Build a mental model before making any changes.

**Check canvas dimensions:** Any screen that doesn't match your configured resolution is either a legacy canvas or was built for a different target. Note it. If redesigning, rebuild at your target resolution using the scaffolds above.

---

### Workflow 2: Design Token Propagation

Use when: a brand color or font changed and needs to propagate across an existing .pen file.

**Step 1 — Audit current values:**
```
search_all_unique_properties(parentIds=["root"])
```
Find all instances of the old token value.

**Step 2 — Replace:**
```
replace_all_matching_properties(
  parentIds=["root"],
  matchProperty="bg",
  matchValue="#OLD_VALUE",
  newValue="#NEW_VALUE"
)
```
Run once per property type (bg, color, borderColor, etc.) that uses the changed token.

**Step 3 — Screenshot to verify:**
```
get_screenshot(nodeId="[root or key screen id]")
```

**Common token propagation scenarios:**
- Color change: replace `bg`, `color`, `borderColor`, `fill` separately
- Font change: replace `fontFamily` across all text nodes
- Spacing change: replace specific `gap`, `p`, `px`, `py` values — be careful; many elements may share the same numeric value for different structural reasons

---

### Workflow 3: Canvas Spatial Management

Use when: adding a new screen to an existing .pen file.

```
find_empty_space_on_canvas(
  direction="right",
  desiredWidth=SCAFFOLD_CANVAS_WIDTH,
  desiredHeight=SCAFFOLD_CANVAS_HEIGHT
)
```

Use the returned x/y coordinates as the position for the new screen's root frame.

**Convention for multi-screen .pen files:**
- Arrange screens left-to-right, one screen per column
- ~120px gap between screens horizontally
- Related states of the same screen stack vertically with ~80px gap
- Name screens: `"Screen N — [Name] ([state])"` — e.g. `"Screen 2 — User Profile (edit mode)"`

---

### Workflow 4: Style Guide Pull (New Page Types)

Use when building a page type your app hasn't designed before.

```
1. get_style_guide_tags()
2. get_style_guide(tags=["enterprise", "dashboard", "minimal", "professional"])
```

Style guides provide inspiration — your design system overrides any stylistic conflict. Retain: compositional discipline, whitespace patterns, data density approaches. Discard: anything that conflicts with your established visual language.

---

### Workflow 5: Bulk Property Inspection

Use when: auditing a .pen file for consistency, or preparing for a token replacement.

```
search_all_unique_properties(
  parentIds=["root"],
  propertyNames=["bg", "color", "fontFamily", "fontSize", "gap"]
)
```

Use to find hardcoded hex values that should be tokens, rogue font sizes, or spacing values that break your grid.

---

## Tool Reference

Quick lookup — full parameter docs in `references/tool-reference.md`.

| Tool | When to use |
|---|---|
| `get_editor_state` | Start of every session |
| `open_document` | Open a .pen file, or `"new"` for blank canvas |
| `get_guidelines` | Load design rules (`topic="web-app"` for app screens) |
| `get_style_guide_tags` | See available inspiration tags |
| `get_style_guide` | Pull inspiration for a new page type |
| `batch_get` | Read node tree — use for archaeology |
| `batch_design` | Create, update, copy, move, delete nodes |
| `snapshot_layout` | Verify computed dimensions after every scaffold |
| `get_screenshot` | Visual review after each major build step |
| `get_variables` | Check current token state |
| `set_variables` | Inject brand tokens |
| `find_empty_space_on_canvas` | Place new screens without overlap |
| `search_all_unique_properties` | Audit all values before token replacement |
| `replace_all_matching_properties` | Bulk token replacement |
| `export_nodes` | Export as PNG/PDF before developer handoff |

---

## Screen Naming and Version Control

**In-file naming:**
- `"Screen N — [Feature] ([state])"`
- `"Screen 1 — Dashboard (empty)"` / `"Screen 1 — Dashboard (populated)"`
- Iterations: add suffix `[v2 post-feedback]` — do not create a new file

**Never create a new .pen file for an iteration.** Add versioned screens to the same file.

**File naming:**
```
mockups/<feature>-v1.pen        — initial design
mockups/<feature>-redesign.pen  — full redesign (significant divergence)
```

Avoid renaming .pen files after sharing with stakeholders — it breaks handoff doc references.

---

## Common Mistakes

| Mistake | Fix |
|---|---|
| Unconfigured SKILL.md | Run `run the pencil-pro setup wizard` before first use |
| Building at the wrong resolution | Check `snapshot_layout()` after every scaffold |
| Hardcoded hex values instead of tokens | `set_variables` first, every session |
| All screens in one giant frame | Separate frames per screen. `find_empty_space_on_canvas` for placement |
| Token replacement by eye | Use `search_all_unique_properties` + `replace_all_matching_properties` |
| Describing .pen file contents from memory | Run `batch_get` first. Never assume what's in a file |
| Skipping `snapshot_layout` after scaffold | Catches 0px dimensions before you build on a broken structure |
| More than 25 ops in one `batch_design` call | Break large builds across multiple calls |
