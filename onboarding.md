# Pencil Pro — First Screen

Walk a new user from zero to a real screen on canvas. Runs automatically after setup, or any time via: `build my first screen with pencil-pro`

---

## Instructions for Claude

You are guiding the user through building their first screen with pencil-pro. The goal is a real, scaffolded screen visible in Pencil by the end of this flow — not a tutorial, not documentation. Just build the thing.

Keep it conversational. Move fast. Each step should feel like a natural next question, not a form.

---

### Step 1 — Pick a screen type

Say:

> **What do you want to build first?**
>
> 1. **Dashboard** — metrics, KPIs, activity feed. Good for the main landing screen of an app.
> 2. **List / Queue** — a table or queue of items. Good for asset libraries, user lists, task queues.
> 3. **Detail / Review** — two-panel layout for reviewing or editing a single item.
> 4. **Marketing Page** — landing page, product page, or hero section.
>
> Pick one, or describe what you're building and I'll choose.

Wait for their answer. If they describe something (e.g. "a user profile page"), map it to the closest scaffold type. Detail/Review covers most single-item views.

---

### Step 2 — Name it

Ask:

> What's this screen for? Just a short description — e.g. "main dashboard", "asset library", "vendor review", "pricing page". I'll use it to name the file and screen.

Store as `screenName`. Derive a slug: lowercase, hyphens, no spaces (e.g. "main dashboard" → `main-dashboard`).

---

### Step 3 — File location

Ask:

> Where should I put this?
>
> 1. **New file** — I'll create `mockups/[slug]-v1.pen`
> 2. **Existing file** — give me the path

If new: set `filePath = "mockups/[slug]-v1.pen"`
If existing: use their path. Note — you'll add the new screen to the existing file without touching what's already there (use `find_empty_space_on_canvas` for placement).

---

### Step 4 — Run session startup

Before building anything:

```
1. get_editor_state()
2. open_document("[filePath]")
3. get_guidelines(topic="web-app")     — or "landing-page" for Scaffold D
4. set_variables([token map from SKILL.md])
5. get_variables()                     — confirm tokens stored
6. snapshot_layout()                   — confirm canvas state
```

If opening an existing file: after `snapshot_layout()`, use `find_empty_space_on_canvas(direction="right", desiredWidth=SCAFFOLD_CANVAS_WIDTH, desiredHeight=SCAFFOLD_CANVAS_HEIGHT)` to find placement for the new screen. Use the returned x/y as the root frame position.

---

### Step 5 — Build the scaffold

Run the appropriate scaffold from SKILL.md using `batch_design`. Use the configured `SCAFFOLD_*` values — do not hardcode dimensions.

After the scaffold operations run, immediately call:
```
snapshot_layout()
```

Check that key nodes have non-zero dimensions:
- `pageArea.width` = SCAFFOLD_PAGE_AREA_WIDTH ✓
- `contentInner.width` = SCAFFOLD_CONTENT_MAX ✓
- No node shows `width: 0` or `height: 0`

If anything is zero: diagnose before continuing (usually a missing explicit width on a flex child).

---

### Step 6 — Add a title

One `batch_design` call only. Add a single text node inside `pageHeader` with the screen name as the title. Nothing else.

```
titleNode=I("pageHeader", {name:"PageTitle", text:"[screenName]", fontSize:18, fontWeight:600, color:"[color-text token value]"})
```

That's it. Do not add cards, rows, grids, or any other content. The scaffold structure is the demo.

---

### Step 7 — Screenshot

```
get_screenshot(nodeId="[root frame node id]")
```

Show the screenshot to the user.

---

### Step 8 — Wrap up

Say:

> **Your first screen is live in Pencil.**
>
> File: `[filePath]`
> Screen: `[screenName]` — [scaffold type]
>
> From here you can:
> - **Iterate:** "Add a chart to the dashboard" / "Add a search bar to the list"
> - **Add a screen:** "Add a detail view for the selected item"
> - **Export:** "Export this screen as PNG for the handoff doc"
> - **Propagate a token change:** "Change color-primary to #1D4ED8 across this file"
>
> Or just describe what you want to build next.

---

## Notes for Claude

- If the user seems uncertain about screen type, ask: "What would a user be trying to do on this screen?" — the answer usually makes the scaffold type obvious.
- If Pencil isn't running or `get_editor_state()` returns an error, pause and tell the user: "Make sure Pencil is running and the MCP is connected, then try again."
- Keep the whole flow under 10 tool calls total. Setup + scaffold + title + screenshot. That's it.
- Do not ask permission to proceed between Steps 4, 5, 6, and 7 — run them as one continuous flow. Only pause if something fails.
