# PencilPlaybook — First Screen

Walk a new user to a real screen on canvas in under 2 minutes. Runs after setup, or any time via: `build my first screen with PencilPlaybook`

---

## Instructions for Claude

One question. Then build. No more pauses until the screenshot is showing.

---

### Step 1 — Preflight check

Before asking anything, call:
```
get_editor_state()
```

If this errors or Pencil is not detected: **stop immediately.** Say:
> Pencil doesn't appear to be running or the MCP isn't connected. Open Pencil, wait for it to load, then try again.

Do not proceed past this point until `get_editor_state()` succeeds.

---

### Step 2 — One question

Ask:

> **What do you want to build?**
> Dashboard · List · Detail · Marketing Page
>
> (Or just describe it and I'll pick.)

Wait for the answer. That's the only question. Do not ask for a name, file path, or any other information.

Map descriptions to scaffold types:
- Home screen, overview, metrics → Dashboard
- Table, queue, library, list of anything → List
- Profile, review, edit, single item → Detail
- Landing page, product page, hero → Marketing Page

---

### Step 3 — Build (no more pauses)

Run all of the following without stopping. Do not ask permission between any of these calls.

**3a. Open a new file:**
```
open_document("mockups/first-screen-v1.pen")
```

**3b. Load guidelines and inject tokens in parallel where possible:**
```
get_guidelines(topic="web-app")   — use "landing-page" for Marketing
set_variables([token map from SKILL.md])
```

**3c. Run the scaffold** using `batch_design` with the SCAFFOLD_* values from SKILL.md. Include a title node in the same call — do not make a separate call for it:

```
— scaffold nodes (sidebar, pageArea, pageHeader, etc.) —
titleNode=I("pageHeader", {name:"PageTitle", text:"[scaffold type name]", fontSize:18, fontWeight:600, color:"#111827"})
```

Replace `#111827` with the `color-text` value from the configured token map.

**3d. Verify and screenshot:**
```
snapshot_layout()
get_screenshot(nodeId="[pageArea node id]")
```

If `snapshot_layout` shows any node with `width: 0` or `height: 0`, fix it before taking the screenshot — usually a missing explicit width on a flex child.

---

### Step 4 — Done

Show the screenshot, then say:

> **Done — your first screen is in Pencil.**
>
> File: `mockups/first-screen-v1.pen`
>
> What do you want to do next? You can describe a change, add another screen, or just start designing.

That's it. No list of features, no tutorial, no next steps menu. Let them drive from here.

---

## Notes for Claude

- Total tool calls: preflight (1) + open + guidelines + set_variables (3) + scaffold+title (1) + snapshot + screenshot (2) = **8 calls max.** If you're exceeding that, you're doing too much.
- The title in the scaffold call prevents a separate round-trip. Always include it in the scaffold `batch_design`.
- If the user says "dashboard" or any single word, don't confirm — just build it.
- The file path `mockups/first-screen-v1.pen` is always the default. Never ask where to save it.
