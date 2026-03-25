---
name: pencilplaybook
description: "PencilPlaybook is the UI Skills / Taste-Skill for Pencil.dev + Claude Code — a design playbook that gives Claude real perceptual psychology and senior-level guardrails so it stops producing averaged-out AI slop."
version: 1.1.0
---

# Pencil Pro

## Primary Resolution Rule

**This skill must be configured before first use.**

**If the user says "run the PencilPlaybook setup wizard" or asks to set up or configure PencilPlaybook:**
→ Read `.claude/skills/pencilplaybook/setup.md` and follow the instructions there exactly.

**If the user says "build my first screen with PencilPlaybook" or asks to start designing:**
→ Read `.claude/skills/pencilplaybook/onboarding.md` and follow the instructions there exactly.

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

Nine scaffold scripts covering the most common layout archetypes. Run the setup wizard to fill in the dimension and color values below before using.

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

### Scaffold E — Modal / Dialog
*Confirmations, destructive actions, quick-edit forms, side peek panels*

Run Scaffold A, B, or C first. Then add this modal overlaid on the pageArea — do not insert into the root.

```
backdrop=I("pageArea", {name:"ModalBackdrop", width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, bg:"rgba(0,0,0,0.48)", display:"flex", alignItems:"center", justifyContent:"center"})
modal=I("backdrop", {name:"Modal", width:480, bg:"SURFACE_COLOR", borderRadius:8, border:"1px solid BORDER_COLOR", display:"flex", flexDirection:"column", overflow:"hidden"})
modalHeader=I("modal", {name:"ModalHeader", height:56, px:24, display:"flex", alignItems:"center", justifyContent:"space-between", borderBottom:"1px solid BORDER_COLOR"})
modalBody=I("modal", {name:"ModalBody", px:24, py:20, display:"flex", flexDirection:"column", gap:16})
modalFooter=I("modal", {name:"ModalFooter", height:60, px:24, display:"flex", alignItems:"center", justifyContent:"flex-end", gap:12, borderTop:"1px solid BORDER_COLOR"})
```

After scaffold: `snapshot_layout()` — confirm modal is visible and centered. Adjust modal width per use case: 380px (compact confirmation), 480px (standard form), 600px (wide form), 720px (detail/preview).

---

### Scaffold F — Wizard / Stepper
*Multi-step forms, onboarding flows, setup sequences, checkout*

```
sidebar=I("root", {name:"Sidebar", width:SCAFFOLD_SIDEBAR_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, bg:"SIDEBAR_BG"})
pageArea=I("root", {name:"PageArea", width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, x:SCAFFOLD_SIDEBAR_WIDTH, bg:"PAGE_BG"})
pageHeader=I("pageArea", {name:"PageHeader", width:SCAFFOLD_PAGE_AREA_WIDTH, height:64, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", px:32})
stepperBar=I("pageArea", {name:"StepperBar", y:64, width:SCAFFOLD_PAGE_AREA_WIDTH, height:60, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", justifyContent:"center"})
contentWrap=I("pageArea", {name:"ContentWrap", y:124, width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CONTENT_HEIGHT_F, display:"flex", justifyContent:"center", px:32, pt:32})
contentInner=I("contentWrap", {name:"ContentInner", width:640, display:"flex", flexDirection:"column", gap:24})
actionFooter=I("pageArea", {name:"ActionFooter", y:SCAFFOLD_WIZARD_FOOTER_Y, width:SCAFFOLD_PAGE_AREA_WIDTH, height:72, bg:"SURFACE_COLOR", borderTop:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", justifyContent:"space-between", px:32})
```

Note: ContentInner is 640px wide by default — focused single-column form. Back button left, Next/Submit right in ActionFooter (`justifyContent:"space-between"`).

---

### Scaffold G — Mobile Screen
*390×844px (iPhone 14 standard). Use this for all mobile screens — hardcoded, no setup values required.*

```
statusBar=I("root", {name:"StatusBar", width:390, height:44, bg:"PAGE_BG"})
navBar=I("root", {name:"NavBar", y:44, width:390, height:56, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", px:16})
scrollContent=I("root", {name:"ScrollContent", y:100, width:390, height:688, bg:"PAGE_BG", display:"flex", flexDirection:"column", overflow:"hidden"})
contentPad=I("scrollContent", {name:"ContentPad", width:390, display:"flex", flexDirection:"column", gap:16, px:16, pt:16})
bottomNav=I("root", {name:"BottomNav", y:788, width:390, height:56, bg:"SURFACE_COLOR", borderTop:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", justifyContent:"space-around"})
```

Note: Remove `statusBar` and set `navBar.y=0` if not showing system chrome. Remove `bottomNav` for apps without tab navigation. Touch targets inside this scaffold must be ≥44×44px.

---

### Scaffold H — Form / Data Entry
*Settings pages, profile editing, heavy data entry — single-column, centered form*

```
sidebar=I("root", {name:"Sidebar", width:SCAFFOLD_SIDEBAR_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, bg:"SIDEBAR_BG"})
pageArea=I("root", {name:"PageArea", width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, x:SCAFFOLD_SIDEBAR_WIDTH, bg:"PAGE_BG"})
pageHeader=I("pageArea", {name:"PageHeader", width:SCAFFOLD_PAGE_AREA_WIDTH, height:64, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", justifyContent:"space-between", px:32})
formWrap=I("pageArea", {name:"FormWrap", y:64, width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CONTENT_HEIGHT_H, display:"flex", justifyContent:"center", px:32, pt:32})
formInner=I("formWrap", {name:"FormInner", width:600, display:"flex", flexDirection:"column", gap:28})
formSection=I("formInner", {name:"FormSection1", display:"flex", flexDirection:"column", gap:16})
formActions=I("formInner", {name:"FormActions", display:"flex", gap:12, justifyContent:"flex-end"})
```

Note: FormInner width defaults to 600px. Use 480px for simple flows, 800px for wide two-column layouts. Add more FormSection nodes inside formInner to group related fields.

---

### Scaffold I — Empty State / Onboarding Step
*Zero-data views, first-use prompts, feature discovery, blank-slate screens*

```
sidebar=I("root", {name:"Sidebar", width:SCAFFOLD_SIDEBAR_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, bg:"SIDEBAR_BG"})
pageArea=I("root", {name:"PageArea", width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CANVAS_HEIGHT, x:SCAFFOLD_SIDEBAR_WIDTH, bg:"PAGE_BG"})
pageHeader=I("pageArea", {name:"PageHeader", width:SCAFFOLD_PAGE_AREA_WIDTH, height:64, bg:"SURFACE_COLOR", borderBottom:"1px solid BORDER_COLOR"})
emptyCenter=I("pageArea", {name:"EmptyCenter", y:64, width:SCAFFOLD_PAGE_AREA_WIDTH, height:SCAFFOLD_CONTENT_HEIGHT_H, display:"flex", alignItems:"center", justifyContent:"center"})
emptyContent=I("emptyCenter", {name:"EmptyContent", width:400, display:"flex", flexDirection:"column", alignItems:"center", gap:20})
illustrationArea=I("emptyContent", {name:"IllustrationArea", width:200, height:160, borderRadius:12, bg:"SURFACE_COLOR", border:"1px solid BORDER_COLOR", display:"flex", alignItems:"center", justifyContent:"center"})
textStack=I("emptyContent", {name:"TextStack", width:400, display:"flex", flexDirection:"column", alignItems:"center", gap:8})
emptyActions=I("emptyContent", {name:"EmptyActions", display:"flex", gap:12})
```

Note: Replace `IllustrationArea` with `G("illustrationArea", "ai", "your prompt")` to generate an illustration. Keep empty state copy short: one headline (16–18px), one supporting line (14px muted), one primary CTA.

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

### Workflow 6: Version Snapshot (Protect Your Work)

Use when: before any major redesign, refactor, or when switching between files.

**Prompt template:**
```
Using PencilPlaybook, create a version snapshot comment at the top of the
current canvas. Include: current date/time, list of all screens, active
design tokens summary, and a short note of what is about to change.
```

This acts as a lightweight undo checkpoint and helps maintain consistency across multiple .pen files. Run it before:
- Token propagation across a large file
- A structural refactor (changing layout, splitting panels)
- Handing off to another session or collaborator

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

## Perceptual Design Defaults

Quick-lookup tables for building components in Pencil. Science-backed defaults — use them unless your design system specifies otherwise.

### Typography
| Property | Size Range | Default Value | Rationale |
|---|---|---|---|
| Letter spacing | 56px+ (display) | −0.03em | Counters appear open at large sizes |
| Letter spacing | 32–48px (heading) | −0.015em | Moderate tightening |
| Letter spacing | 14–18px (body) | 0 (normal) | Typefaces optimized here |
| Letter spacing | 10–12px (caption) | +0.015em | Counters close up at small sizes |
| Line height | Body text | 1.5× minimum | WCAG SC 1.4.12 |
| Line height | Headings | 1.1–1.2× | Tighter for visual density |
| Line length | Body text | 45–75 chars (65ch optimal) | Baymard Institute |
| Minimum font size | Any text | 10px | Below 10px is illegible on screen |

### Color
| Property | Default Value | Rationale |
|---|---|---|
| Disabled opacity | 40% | 50% creates ghosts that compete. 40% recedes fully. (MD3, Workday) |
| Hover lightness delta | 8% minimum | Below 8% is imperceptible on most monitors. (NNGroup) |
| Dark surface body text | `#E2E8F0` or `#F1F5F9` | Pure white on very dark bg causes halation. Headlines can use white. (APCA) |
| Non-text contrast | 3:1 minimum | Icons, borders, form controls vs adjacent color. (WCAG 2.2 SC 1.4.11) |
| Text contrast | 4.5:1 minimum | Body text. Large text (24px+ or 18.66px+ bold): 3:1. (WCAG AA) |

### Motion
| Property | Default Value | Rationale |
|---|---|---|
| Duration floor | 100ms | Below this, motion is imperceptible — snap instead |
| Duration standard | 200–300ms | Most UI transitions |
| Duration ceiling | 400ms | Complex choreography only. Never exceed 500ms. |
| Enter easing | `cubic-bezier(0, 0, 0.2, 1)` | Decelerate — arriving elements settle |
| Exit easing | `cubic-bezier(0.4, 0, 1, 1)` | Accelerate — departing elements dismiss |
| Standard easing | `cubic-bezier(0.4, 0, 0.2, 1)` | Within-screen repositioning |

### Spacing
| Property | Default Value | Rationale |
|---|---|---|
| Touch target (mobile) | 44×44px | Apple HIG engineering target |
| Touch target (desktop min) | 24×24px | WCAG 2.2 SC 2.5.8 floor |
| Button height (primary) | 40–44px | Comfortable click target |
| Button height (compact) | 32px | Dense UI, secondary actions |
| Border radius (cards) | 8px max | >8px reads consumer/casual |
| Border radius (buttons) | 6px | Matches card inner radius at 8px outer with 2px offset |
| Focus ring offset | 2px | Ring radius = component radius + 2px (WCAG 2.2 SC 2.4.13) |

### Icons
| Property | Default Value | Rationale |
|---|---|---|
| Minimum size | 16×16px | Below 16px, icons lose clarity at 1x density |
| Stroke weight | 1.5px (standard), 2px (emphasis) | Match text weight hierarchy |
| Optical alignment | Shift circle icons down 1px | Circles appear to float relative to square bounds |
| Touch target padding | Extend to 44×44px with transparent hit area | Icon can be 20px visual, 44px tap target |

---

## Common Mistakes

| Mistake | Fix |
|---|---|
| Unconfigured SKILL.md | Run `run the PencilPlaybook setup wizard` before first use |
| Building at the wrong resolution | Check `snapshot_layout()` after every scaffold |
| Hardcoded hex values instead of tokens | `set_variables` first, every session |
| All screens in one giant frame | Separate frames per screen. `find_empty_space_on_canvas` for placement |
| Token replacement by eye | Use `search_all_unique_properties` + `replace_all_matching_properties` |
| Describing .pen file contents from memory | Run `batch_get` first. Never assume what's in a file |
| Skipping `snapshot_layout` after scaffold | Catches 0px dimensions before you build on a broken structure |
| More than 25 ops in one `batch_design` call | Break large builds across multiple calls |

---

## Edge Cases

### When Claude ignores your tokens

If a design decision doesn't match your configured token map (wrong color, arbitrary font size, rogue hex value):

1. Say explicitly: *"That color isn't from the token map. Use `color-primary` from the variables set at session start."*
2. Run `get_variables()` to confirm tokens are actually set — if the map is empty, `set_variables` was skipped. Re-run the session startup sequence.
3. Run `search_all_unique_properties(parentIds=["root"], propertyNames=["bg","color"])` to surface any hardcoded values that snuck in, then bulk-replace them.

If Claude continues inventing values: stop, re-read SKILL.md aloud in the prompt — *"Using PencilPlaybook, [instruction]"* — to re-anchor to the skill context.

---

### Recovering from a broken scaffold

Symptom: `snapshot_layout()` shows a node with `width: 0` or `height: 0`.

Cause: Almost always one of:
- A flex container with no explicit size and no children that force expansion
- A child with `width:"fill_container"` inside a parent that has no explicit width
- Conflicting or missing `x`/`y` coordinates on absolutely positioned nodes

Fix sequence:
```
1. snapshot_layout()                          — identify the 0px node
2. batch_get(nodeIds=["[broken-node-id]"])    — inspect its properties
3. U("[broken-node-id]", {width: N, height: N}) — add explicit dimensions
4. snapshot_layout()                          — confirm fixed before continuing
```

If the scaffold is too broken to patch: delete the root frame (`D("frame-id")`), confirm clean state with `get_editor_state()`, and re-scaffold from scratch.

---

### Multi-canvas projects

When working across multiple `.pen` files in the same session:

- **Tokens don't persist between files.** Each `open_document` call requires a fresh `set_variables`. Keep your token map in SKILL.md and paste it every time.
- **Run Canvas Archaeology on every file before editing.** Never assume a file's structure from memory — always `batch_get(patterns=["*"])` first.
- **One file at a time.** Don't interleave `batch_design` calls across two open files. Finish work on file A, then open file B.
- **Cross-file consistency.** If a component appears in multiple files, use identical node names. Mismatched names make token propagation and audit scripts unreliable.
