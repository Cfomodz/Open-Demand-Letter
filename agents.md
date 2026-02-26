# Agents — Sub-Agent Coordination

This file defines how Claude Code sub-agents should approach work on this project.

## Project Context

Open Demand Letter is a static, client-side web app. There is no backend, no database,
no build step, and no npm. All work happens in plain HTML, CSS, and vanilla JavaScript.

## Agent Roles

### Frontend Agent

**Scope:** `index.html`, `app.html`, `css/style.css`

**Instructions:**
- Write semantic HTML5 with proper ARIA attributes
- CSS is mobile-first, responsive, minimal — no frameworks
- Landing page (`index.html`) is a single-screen marketing page
- App page (`app.html`) is the form + preview + download flow
- GitHub icon + repo link must be visible on every page
- Legal disclaimer must appear on every page
- All form fields are optional — never add `required` attributes
- Test at 320px, 768px, and 1024px+ widths

### JavaScript Agent

**Scope:** `js/app.js`, `js/letter.js`, `js/fragment-api.js`, `js/download.js`, `js/telemetry.js`

**Instructions:**
- Vanilla ES6+ JavaScript only. No libraries, no npm, no imports from CDN.
- Each file is a self-contained module loaded via `<script>` tags
- Never make network requests containing user form data
- `letter.js`: Handle all combinations of present/absent fields gracefully
- `fragment-api.js`: Parse `window.location.hash`, never `window.location.search`
- `download.js`: Use Blob + createObjectURL pattern. File must be print-ready HTML.
- `telemetry.js`: Only the numeric amount. Only on explicit opt-in. Single POST.
- Test with DevTools Network tab open to verify no data leaks

### Documentation Agent

**Scope:** `docs/`, `README.md`, `CLAUDE.md`, `.github/`

**Instructions:**
- Keep docs concise and accurate
- Update docs when code changes affect documented behavior
- Fragment API docs must stay in sync with actual parameter support
- Privacy claims must remain accurate — verify against code

### Review Agent

**Scope:** All files (read-only analysis)

**Instructions:**
- Verify no network requests contain user data
- Check that all form fields remain optional
- Confirm no third-party scripts are loaded
- Validate HTML semantics and accessibility
- Ensure legal disclaimer is present on all pages
- Check that GitHub link is visible on all pages
- Verify fragment API uses `#` not `?`

## Coordination Rules

1. **Privacy is a hard constraint.** If an agent's changes could transmit user data,
   the Review Agent must verify before merge.
2. **No dependencies.** Never add npm packages, CDN scripts, or external resources.
   Exception: a single font from Google Fonts is acceptable if needed.
3. **File structure is defined in CLAUDE.md.** Don't create files outside the
   established structure without updating CLAUDE.md.
4. **All changes must be tested** with: empty form, full form, partial form,
   and fragment API pre-fill.
