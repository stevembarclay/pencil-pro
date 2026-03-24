# Pencil MCP Tool Reference

Full parameter reference for all Pencil MCP tools. `SKILL.md` has the workflow patterns — this file has the parameter details.

---

## `get_editor_state()`

No parameters. Returns:
- Currently active .pen file path
- Selected node IDs
- Canvas dimensions of the active file
- Available documents

**Always call first.** Tells you what's open and what the user has selected. Never assume.

---

## `open_document(filePathOrNew)`

| Parameter | Type | Description |
|---|---|---|
| `filePathOrNew` | string | Absolute path to a .pen file, or `"new"` for blank canvas |

Returns the root node ID of the opened document.

**Pattern:**
```
open_document("path/to/your-file.pen")
```

If the file doesn't exist at the path, Pencil creates it as a new document at that path.

---

## `get_guidelines(topic)`

| Parameter | Values |
|---|---|
| `topic` | `"code"`, `"table"`, `"tailwind"`, `"landing-page"`, `"slides"`, `"design-system"`, `"mobile-app"`, `"web-app"` |

For app screens: use `"web-app"`.
For marketing/landing pages: use `"landing-page"`.
For data tables: use `"table"` in addition to `"web-app"`.

Returns design guidelines that inform `batch_design` operations in the same session.

---

## `get_style_guide_tags()`

No parameters. Returns the full list of available style guide tags.

Use before `get_style_guide` to see what combinations are available. Common tag sets:
- App: `["enterprise", "dashboard", "financial", "professional", "minimal"]`
- Marketing: `["b2b", "saas", "professional", "clean", "editorial"]`

---

## `get_style_guide(tags, name)`

| Parameter | Type | Description |
|---|---|---|
| `tags` | string[] | Array of tags to match |
| `name` | string | Specific style guide name (optional — use tags if unsure) |

Returns design inspiration. Always filter results against your existing design system — style guides provide direction, not a template to copy.

---

## `batch_get(patterns, nodeIds)`

| Parameter | Type | Description |
|---|---|---|
| `patterns` | string[] | Glob-style patterns to match node names (e.g., `["*"]`, `["Screen*"]`, `["*Header*"]`) |
| `nodeIds` | string[] | Specific node IDs to fetch (optional — use instead of patterns when you have IDs) |

Returns the node tree for matched nodes including: id, name, type, dimensions, position, children, properties.

**Use cases:**
- `batch_get(patterns=["*"])` — full file archaeology, get everything
- `batch_get(patterns=["Screen*"])` — get all top-level screens
- `batch_get(nodeIds=["abc123", "def456"])` — fetch specific nodes after a scaffold

**Depth note:** `batch_get` returns nested children. For large files, start with top-level patterns to understand structure, then drill into specific subtrees.

---

## `batch_design(operations)`

The primary build tool. Executes multiple insert/copy/update/replace/move/delete/image operations.

**Operation syntax:**
```
foo=I("parentId", { ...properties })           — Insert
bar=C("sourceId", "parentId", { ...props })    — Copy from existing node
R("nodeId1/nodeId2", { ...props })             — Replace
U(foo+"/childId", { ...props })                — Update (use variable references)
D("nodeId")                                    — Delete
M("nodeId", "newParentId", index)              — Move
G("nodeId", "ai", "prompt text")              — Generate AI image
```

**Property keys (commonly used):**
```
width, height, x, y              — dimensions and position
bg                               — background color (hex)
color                            — text color (hex)
borderRadius                     — border radius (px)
border                           — e.g. "1px solid #E2E8F0"
borderBottom, borderTop, etc.    — individual borders
display                          — "flex", "grid", "block"
flexDirection                    — "row", "column"
alignItems, justifyContent       — flex alignment
gap                              — flex/grid gap (px)
p, px, py, pt, pb, pl, pr       — padding
fontFamily                       — font name string
fontSize                         — px value
fontWeight                       — numeric weight
lineHeight                       — multiplier or px
letterSpacing                    — em value string (e.g. "0.05em")
textTransform                    — "uppercase", "lowercase", "none"
opacity                          — 0-1
overflow                         — "hidden", "visible", "scroll"
name                             — node display name in Pencil
text                             — text content (for text nodes)
```

**Max operations per call:** 25. Break large builds into multiple calls.

**Variable references in update operations:** After inserting `foo=I(...)`, you can reference `foo` in subsequent operations in the same call: `U(foo+"/childId", {...})`.

---

## `snapshot_layout()`

No parameters. Returns computed layout rectangles for every node in the current document — actual rendered x, y, width, height after flexbox/grid resolution.

**Always run after every scaffold.** A container that renders as 0px is invisible — `snapshot_layout` catches this before you build content on top of a broken structure.

**Read carefully:** If a node shows `width: 0` or `height: 0`, there's a layout bug. Usually caused by:
- Missing explicit dimensions on a non-flex child
- A flex container with no children and no explicit size
- Conflicting width values

---

## `get_screenshot(nodeId)`

| Parameter | Type | Description |
|---|---|---|
| `nodeId` | string | ID of the node to capture |

Returns a screenshot of the specified node. Use for:
- Approval checkpoints (show stakeholders before building the next screen)
- Design audit input
- Verifying token replacement results

**Run after each major build step.** Don't batch all screens and screenshot at the end — catch problems early.

---

## `get_variables()`

No parameters. Returns all variables currently set in the active document.

Always call after `set_variables` to confirm tokens were stored correctly. If a token is missing from the returned map, it wasn't set — retry.

---

## `set_variables(variables)`

| Parameter | Type | Description |
|---|---|---|
| `variables` | object | Key-value pairs of token name → value |

Sets design variables on the current document. See `SKILL.md` for the token map structure.

**Call at the start of every session.** Variables may not persist between Claude Code sessions. Confirm with `get_variables()` after setting.

---

## `find_empty_space_on_canvas(direction, desiredWidth, desiredHeight)`

| Parameter | Type | Description |
|---|---|---|
| `direction` | string | `"right"`, `"down"`, `"left"` — which direction to search from existing content |
| `desiredWidth` | number | Width of the space needed (px) |
| `desiredHeight` | number | Height of the space needed (px) |

Returns x/y coordinates for an empty zone of the specified size.

Use the returned coordinates as the `x` and `y` for the new screen's root frame insert.

---

## `search_all_unique_properties(parentIds, propertyNames)`

| Parameter | Type | Description |
|---|---|---|
| `parentIds` | string[] | Root node IDs to search from — use `["root"]` for entire file |
| `propertyNames` | string[] | Property names to audit (optional — omit for all properties) |

Returns every unique value in use for the specified properties, recursively through all children.

**Use before `replace_all_matching_properties`.** Know what you're replacing before you replace it.

**Common audit patterns:**
```
search_all_unique_properties(parentIds=["root"], propertyNames=["bg", "color"])
— Find all hex values in use. Spot non-token hardcoded values.

search_all_unique_properties(parentIds=["root"], propertyNames=["fontFamily"])
— Verify all text uses brand fonts. Find any rogue system fonts.

search_all_unique_properties(parentIds=["root"], propertyNames=["gap", "p", "px", "py"])
— Audit spacing for grid compliance.
```

---

## `replace_all_matching_properties(parentIds, matchProperty, matchValue, newValue)`

| Parameter | Type | Description |
|---|---|---|
| `parentIds` | string[] | Root node IDs to search — use `["root"]` for entire file |
| `matchProperty` | string | Property name to match (e.g., `"bg"`, `"color"`) |
| `matchValue` | string | Current value to find |
| `newValue` | string | New value to substitute |

Replaces every occurrence recursively. **One property type per call.** To replace a color that appears as both `bg` and `color`, make two separate calls.

**Always `search_all_unique_properties` first.** Know what you're replacing. Don't use this blindly.

**Verify with `get_screenshot` after.** Token replacement can have unexpected effects if multiple node types share a value.

---

## `export_nodes(nodeIds, format, folder)`

| Parameter | Type | Description |
|---|---|---|
| `nodeIds` | string[] | Node IDs to export |
| `format` | string | `"png"`, `"jpeg"`, `"webp"`, `"pdf"` |
| `folder` | string | Output directory path |

**Export before handoff to developers.** Developers typically don't have Pencil MCP access — give them PNG exports to build from.

**Standard export call:**
```
export_nodes(
  nodeIds=["screen-1-id", "screen-2-id"],
  format="png",
  folder="path/to/screenshots/YYYY-MM-DD-feature/"
)
```

The folder will be created if it doesn't exist.
