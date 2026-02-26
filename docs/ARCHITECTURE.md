# Architecture

## Overview

Open Demand Letter is a **static, client-side web application** with zero server-side logic. The entire application is a collection of HTML, CSS, and JavaScript files served by Cloudflare Pages.

```
┌─────────────────────────────────────────────────────────┐
│                    User's Browser                       │
│                                                         │
│  ┌──────────┐   ┌───────────┐   ┌──────────────────┐   │
│  │  Landing  │──▶│  App Form │──▶│ Letter Generator │   │
│  │  Page     │   │           │   │  (JS in-browser) │   │
│  └──────────┘   └───────────┘   └────────┬─────────┘   │
│                                          │              │
│                                 ┌────────▼─────────┐   │
│                                 │  File Download    │   │
│                                 │  (auto-triggered) │   │
│                                 └──────────────────┘   │
│                                                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Optional: Opt-in amount share (POST amount only)│   │
│  └──────────────────────────────────────────────────┘   │
│                                                         │
└─────────────────────────────────────────────────────────┘
         │ HTTP GET (static files only)
         ▼
┌─────────────────────┐
│  Cloudflare Pages   │
│  (static hosting)   │
│  - HTTPS            │
│  - Basic analytics  │
│  - CDN edge cache   │
└─────────────────────┘
```

## Pages

### Landing Page (`index.html`)

- Explains what the tool does in plain language
- Emphasizes privacy and open-source nature
- Single CTA button leading to `app.html`
- GitHub icon/link to the repository (always visible)
- Legal disclaimer

### App Page (`app.html`)

- The form-based demand letter generator
- All logic runs client-side
- GitHub icon/link remains visible
- No navigation away from page required to generate/download

## JavaScript Modules

### `js/app.js` — Main Application Logic

- Initializes the form
- Handles form validation (soft — all fields optional)
- Coordinates between modules
- Manages UI state (form → preview → download)

### `js/letter.js` — Letter Template Engine

- Contains the demand letter template(s)
- Performs string interpolation with form values
- Handles conditional sections (omit sections when data is missing)
- Formats dates, currency amounts, addresses
- Generates the final letter as formatted HTML

### `js/fragment-api.js` — Hash Parameter Parser

- Reads `window.location.hash` on page load
- Parses key=value pairs (URL-decoded)
- Maps parameters to form fields
- Supports `auto=1` for automatic generation and download
- Never interacts with the server — `#` fragments are client-side only

### `js/download.js` — File Generation & Download

- Converts the generated letter HTML to a downloadable file
- Triggers browser download via `Blob` + `URL.createObjectURL`
- Primary format: HTML (styled, print-ready)
- Future consideration: PDF generation via client-side library

### `js/telemetry.js` — Optional Amount Sharing

- **Disabled by default**
- After letter generation, presents opt-in prompt:
  "Would you like to anonymously share the demand amount? This helps us understand how people use this tool."
- If user opts in: sends a single POST request containing ONLY the numeric amount
- No identifying information, no timestamps correlatable to user activity
- Endpoint: a simple Cloudflare Worker that appends to an aggregate counter

## Data Flow

```
1. User opens app.html
   └─▶ Browser fetches static HTML/CSS/JS from Cloudflare (normal HTTP GET)

2. fragment-api.js checks window.location.hash
   └─▶ If hash params exist, pre-fills form fields (client-side only)

3. User fills out form (or form is pre-filled)
   └─▶ Data lives in DOM form elements — never transmitted

4. User clicks "Generate Letter"
   └─▶ letter.js reads form values, generates HTML letter
   └─▶ Preview displayed in-browser

5. User clicks "Download" (or auto-triggered)
   └─▶ download.js creates Blob, triggers browser download
   └─▶ File saved to user's device

6. (Optional) Opt-in amount share prompt
   └─▶ If accepted: POST { amount: 5000 } to endpoint
   └─▶ If declined: nothing happens
```

## Security Considerations

- **No server-side processing**: Eliminates entire classes of attacks (SQL injection, server-side data breaches, etc.)
- **No query parameters for sensitive data**: Fragment identifiers (`#`) are never sent in HTTP requests
- **No third-party scripts**: No analytics SDKs, no ad trackers, no external JS
- **Content Security Policy**: Strict CSP headers via Cloudflare to prevent XSS
- **Subresource Integrity**: If any external resources are ever added, SRI hashes required
- **HTTPS only**: Enforced by Cloudflare

## Deployment

```
GitHub repo (main branch)
    │
    ▼ (Cloudflare Pages auto-deploy)
Cloudflare CDN Edge
    │
    ▼ (HTTPS, cached globally)
User's browser
```

- No build command. No CI/CD pipeline needed.
- Publish directory: `/` (repository root)
- Branch: `main` triggers production deploy
