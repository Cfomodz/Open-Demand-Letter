# CLAUDE.md — Open Demand Letter

## Project Overview

Open Demand Letter is a privacy-first, client-side web application that generates
demand letters. **No data ever leaves the user's browser** unless they explicitly
opt in to share the demand amount (and only the amount) after generation.

## Architecture

- **Static site**: HTML + CSS + vanilla JS. No frameworks, no build step.
- **Hosting**: Cloudflare Pages (provides automatic HTTPS + basic web telemetry).
- **Storage**: None. Zero server-side storage. No database. No cookies (beyond Cloudflare).
- **Privacy model**: All form data stays in the browser. Letter generation and PDF/DOCX
  creation happen entirely in JavaScript. The generated file is downloaded directly.

## Key Design Principles

1. **Simplicity over features** — This is intentionally basic. Resist scope creep.
2. **Privacy by design** — Never transmit user data. Fragment API (`#`) not query params (`?`).
3. **All fields optional** — Users may have limited info. Every field can be blank.
4. **No accounts, no pricing, no selling** — Just a landing page and the app.
5. **Open source and transparent** — GitHub link visible in the app at all times.

## File Structure

```
/
├── index.html              # Landing page
├── app.html                # The demand letter web app
├── css/
│   └── style.css           # All styles (landing + app)
├── js/
│   ├── app.js              # Main app logic, form handling
│   ├── letter.js           # Letter template engine / generation
│   ├── fragment-api.js     # Hash-based parameter parsing (#key=val&...)
│   ├── download.js         # File generation and download (HTML/PDF)
│   └── telemetry.js        # Optional opt-in amount sharing (POST only amount)
├── assets/
│   └── icons/              # GitHub icon, favicon, etc.
├── docs/                   # Project documentation
├── .github/                # GitHub templates, workflows
├── CLAUDE.md               # This file
├── agents.md               # Sub-agent coordination doc
├── README.md               # Project README
└── LICENSE                 # MIT License
```

## Code Conventions

- **No build tools**: No webpack, vite, npm, etc. Just static files.
- **Vanilla JS only**: No jQuery, no React, no frameworks. ES6+ is fine.
- **Semantic HTML**: Use proper elements. Accessible by default.
- **Mobile-first CSS**: Responsive design, clean and minimal.
- **No external API calls** from the app except the optional telemetry endpoint.
- **Comments**: Only where logic is non-obvious. Code should be self-documenting.

## Fragment API (Hash Parameters)

The app supports pre-filling via URL hash fragments:
```
app.html#to_name=Acme+Corp&amount=5000&issue=breach+of+contract
```
The `#` portion is **never sent to the server** in the HTTP request — it is
parsed entirely client-side by `fragment-api.js`. This enables "API-like"
behavior where a link can pre-fill and even auto-generate a letter without
any data touching the server.

Supported hash parameters: `to_name`, `to_company`, `to_address`, `to_phone`,
`to_email`, `to_fax`, `to_info`, `from_name`, `from_company`, `from_address`,
`from_phone`, `from_email`, `from_fax`, `from_info`, `issue`, `amount`,
`deadline`, `auto` (if `auto=1`, generate and download immediately).

## Testing

- Manual browser testing is the primary method.
- Ensure form works with all fields empty, all fields filled, and partial fills.
- Test fragment API with various parameter combinations.
- Test download in Chrome, Firefox, Safari, Edge.
- Verify no network requests contain user data (use browser DevTools Network tab).

## Deployment

- Push to `main` → Cloudflare Pages auto-deploys.
- No build command needed. Publish directory is `/` (root).

## Privacy Rules (Non-Negotiable)

- **NEVER** add analytics that captures form content.
- **NEVER** transmit recipient/sender details to any server.
- **NEVER** use query parameters (`?`) for sensitive data — only fragments (`#`).
- **NEVER** add third-party scripts that could exfiltrate data.
- The ONLY data that may be sent is the demand **amount**, and ONLY after
  explicit user opt-in, ONLY after the letter is generated, via a single
  POST request with no other identifying information.
