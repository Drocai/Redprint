# CLAUDE.md

## Project overview
Redprint is a mobile-first, single-file web tool that turns a short core concept into a structured build brief. The app outputs either an AI prompt or JSON using one of three templates (`quick`, `production`, `collab`) and one of three project types (`Game`, `Tool`, `Web`).

## Repository layout
| Path | Purpose |
|---|---|
| `V1` | Main application. Single-file HTML app with inline CSS and inline JavaScript. **No file extension by design. Do not rename it.** |
| `README.md` | Minimal placeholder README. Not a reliable source of project context. |
| `.github/workflows/main.yml` | CI validation. Preserves the contract around the root `V1` file. |

## Running and previewing
- There is no build step, package manager, or dependency install.
- Primary preview path: serve the repo locally and open `/V1` in a browser.
- Recommended local server:
  - `python3 -m http.server`
  - then navigate to `http://localhost:8000/V1`
- Because `V1` has no `.html` extension, some browsers or tools may not auto-detect it correctly when opened directly from disk.
- If a browser refuses to render it cleanly, you may make a temporary local copy named `V1.html` for preview only. **Do not commit that copy.**

## CI contract
The workflow at `.github/workflows/main.yml` enforces two hard requirements:
1. A file named `V1` must exist at the repository root.
2. `V1` must contain the literal string `<!DOCTYPE html>`.

Any refactor that renames, removes, relocates, or replaces `V1` will break CI.
Any edit that removes the document doctype will also break CI.

## Code conventions
### Single-file architecture
- Keep HTML, CSS, and JavaScript inside `V1`.
- Do not introduce a build system, bundler, or dependency pipeline unless explicitly requested.
- Do not split the file into separate assets unless explicitly requested.

### Styling
- The UI uses Tailwind via CDN: `https://cdn.tailwindcss.com`
- The visual language is dark, mobile-first, and compact.
- Match the existing palette and feel:
  - background: `#0a0a0a`
  - accent reds: `#ff3b3b`, `#dc2626`
  - surfaces: zinc-800 / zinc-900
  - typography: `Courier New`, monospace
  - layout: mobile-first with `max-w-md`

### Event handling
- Existing controls use inline `onclick="..."` handlers.
- This is intentional for the current single-file pattern.
- New controls should follow the same pattern unless the repo owner explicitly asks for a different architecture.

### State management
- Extend the existing single `state` object.
- Do not introduce parallel state stores or framework-like abstractions.

### Extending modes
When adding a new template mode or output mode:
- add the UI chip/button
- wire it into state
- update active-state visuals
- add the corresponding branch in `generate()` and the template/output logic

## Key functions and snapshot references
These references are based on the current repository snapshot and should be refreshed if `V1` changes materially.

- `state` object: `V1:169-174`
- `generate()` dispatcher: `V1:224-249`
- `generateJSON()`: `V1:251-263`
- `generatePrompt()` templates: `V1:265-318`
- `getSuggestedFeatures()` / `getNextSteps()`: `V1:320-336`
- `copyOutput()` clipboard logic with `execCommand` fallback: `V1:338-358`

## Git workflow
- Active task branch: `claude/add-claude-documentation-sgPg6`
- Default branch: `main`
- Follow the branch-per-task workflow.
- Do not push directly to `main`.

## What not to do
- Do not add `package.json`, npm, Vite, Webpack, or any build tooling.
- Do not rename `V1`.
- Do not add a file extension to `V1`.
- Do not remove `<!DOCTYPE html>`.
- Do not replace the Tailwind CDN setup with a local install unless explicitly requested.
