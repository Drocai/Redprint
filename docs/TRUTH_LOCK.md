# Redprint Project Truth Lock

## What this is
This is the single guide and tutorial for Redprint. Its job is to keep future edits aligned with the real project instead of whatever unnecessary architecture fantasy someone feels like importing that day.

Redprint is a **mobile-first single-file web app** that turns a short core concept into a structured build brief.

It supports:
- **Templates:** `quick`, `production`, `collab`
- **Output modes:** `prompt`, `json`
- **Project types:** `Game`, `Tool`, `Web`

The main app lives in a root file named `V1`. That name is intentional and part of the CI contract.

---

## Project truth in one paragraph
Redprint is not a framework app, not a compiled frontend, and not an npm project. It is a lean browser-first generator built as a single HTML document with inline CSS and inline JavaScript, styled with Tailwind from a CDN. Its function is simple: take a short “core concept” and output either a polished AI prompt or structured JSON based on the selected template and project type.

---

## Non-negotiable rules
1. The root file must stay named `V1`.
2. `V1` must continue to contain `<!DOCTYPE html>`.
3. No build step is required.
4. No package manager is required.
5. No bundler or framework should be introduced unless explicitly requested.
6. Single-file architecture is the default and should be preserved.

Why this matters:
- CI checks for the literal file name `V1`.
- CI checks for the literal string `<!DOCTYPE html>`.
- Renaming the file to `V1.html`, moving it, or replacing it with a build pipeline breaks the repo contract.

---

## Repository map
| Path | Role | Guidance |
|---|---|---|
| `V1` | Main application | Treat as the actual product source |
| `README.md` | Minimal placeholder | Do not rely on it for project context |
| `.github/workflows/main.yml` | CI workflow | Preserve its assumptions when editing |

---

## How to run it
### Recommended preview flow
Serve the repo locally and open `V1` through a local server.

```bash
python3 -m http.server
```

Then open:

```text
http://localhost:8000/V1
```

### Why this matters
Because `V1` has no `.html` extension, some browsers do a lousy job rendering it directly from disk.

### Safe local-only workaround
```bash
cp V1 V1.html
```

Use `V1.html` only for local preview if needed.

**Do not commit `V1.html`.**

---

## CI tutorial
The workflow in `.github/workflows/main.yml` runs on pushes and pull requests to `main`.

It enforces two checks:

### 1) `V1` must exist
If `V1` is renamed, moved, deleted, or replaced, CI fails.

### 2) `V1` must contain `<!DOCTYPE html>`
If the doctype is removed or changed away from that literal string, CI fails.

### Safe editing rule
Before wrapping up any change, ask:
- Is the file still named `V1`?
- Does it still contain `<!DOCTYPE html>`?

If not, the edit is wrong.

---

## Architecture tutorial
### Overall structure
The app is one HTML document containing:
- markup for the mobile UI
- a small custom `<style>` block
- a `<script>` block with all behavior
- Tailwind loaded from CDN

No import graph. No build output. No hidden app shell.

### State model
The app uses one central `state` object. Based on the captured snapshot, it holds:
- `template`
- `outputMode`
- `type`
- `concept`

**Rule:** extend this object instead of creating parallel state elsewhere.

### Event model
Controls are wired with inline `onclick="..."` handlers.

That is intentional for this repo.

**Rule:** new controls should follow the same pattern unless an explicit refactor is requested.

### UI helper layer
The helper functions include:
- `setTemplate`
- `setOutputMode`
- `setField`
- `updateActiveState`
- `autoResize`
- `clearAll`

### Core generation layer
The main logic flows through:
- `generate()` as dispatcher
- `generatePrompt()` for text output
- `generateJSON()` for structured output
- `getSuggestedFeatures()` and `getNextSteps()` as lookup helpers

---

## Key reference points in `V1`
These line references come from the captured snapshot. Refresh them if `V1` changes materially.

| Area | Snapshot reference |
|---|---|
| `state` object | `V1:169-174` |
| `generate()` dispatcher | `V1:224-249` |
| `generateJSON()` | `V1:251-263` |
| `generatePrompt()` | `V1:265-318` |
| `getSuggestedFeatures()` / `getNextSteps()` | `V1:320-336` |
| `copyOutput()` | `V1:338-358` |

---

## Style system
Redprint already has a defined design language.

### Core tokens
- **Background:** `#0a0a0a`
- **Accent:** `#ff3b3b`, `#dc2626`
- **Surfaces:** zinc-800 / zinc-900
- **Type:** `Courier New`, monospace
- **Layout:** mobile-first, `max-w-md`

### Styling rule
Use Tailwind utility classes plus small custom CSS inside the existing `<style>` block.

### Do not do this unless explicitly requested
- migrate to Tailwind build tooling
- replace the CDN with local compilation
- introduce CSS modules, Sass, or component styling systems

---

## Safe change patterns
### Adding a new template mode
1. Add the chip/button in the UI.
2. Update the `state` object.
3. Update active-state visuals.
4. Add the new template logic in `generatePrompt()`.
5. Verify the new mode generates correctly.

### Adding a new output mode
1. Add the UI control.
2. Extend state.
3. Add a new branch in `generate()`.
4. Create or extend the generator function.
5. Verify copy behavior still works.

### Adding a new project type
1. Add the type selector.
2. Extend state.
3. Add prompt or JSON handling.
4. Update `getSuggestedFeatures()`.
5. Update `getNextSteps()`.

If a new type is not wired through the lookup tables, the UI will feel unfinished.

---

## Unsafe change patterns
Do not do any of the following unless explicitly requested:
- rename `V1`
- add `.html` to `V1`
- move `V1` into `src/`
- add `package.json`
- add Vite, Webpack, Parcel, or another bundler
- split CSS or JS into separate files
- replace inline `onclick` wiring with a new event system for no reason
- remove the doctype
- swap CDN Tailwind for local build tooling

These are architecture changes, not harmless cleanup.

---

## Git workflow
### Branches
Active task branch:

```text
claude/add-claude-documentation-sgPg6
```

Default branch:

```text
main
```

### Rule
Do not push directly to `main`.
Use the task branch for this documentation work.

Expected flow:

```bash
git checkout claude/add-claude-documentation-sgPg6
git status
```

For this task, `git status` should show only the documentation files intentionally added.

---

## Minimal verification checklist
```text
[ ] V1 still exists at repo root
[ ] V1 still contains <!DOCTYPE html>
[ ] No build tooling was introduced accidentally
[ ] No temporary preview file like V1.html is being committed
[ ] Any new mode/type was wired through UI, state, and generation logic
[ ] Styling still matches the dark-red mobile-first design language
```

---

## Recommended documentation split
- **`CLAUDE.md`** = compact AI operating brief
- **This file** = human + AI truth lock, tutorial, and guardrail set

That gives future assistants the short version and the full version.

---

## Canonical summary
The safest one-sentence description of the repo is:

> Redprint is a mobile-first, single-file browser app named `V1` that turns a short concept into a structured prompt or JSON brief using inline JavaScript and Tailwind CDN, with CI enforcing that the root file remains named `V1` and still contains `<!DOCTYPE html>`.
