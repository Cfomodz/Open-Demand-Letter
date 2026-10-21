# Privacy Design Document

## Core Principle

**Your data stays on your device.** This is not a marketing claim — it is an architectural decision enforced by the design of the application.

## What We Collect

### Automatically (via Cloudflare)

We do **not** enable Cloudflare Web Analytics, which would add a Cloudflare script to
every page. No analytics script runs on this site.

Like any web host, Cloudflare necessarily receives your IP address and the page you
requested in order to serve it, and its dashboard shows us aggregate request counts
(for example, total requests and country). This never includes:

- Any form field contents
- The URL fragment (`#...`), so pre-filled values are never seen
- Any information about your demand letter
- Any information about who you are or who your letter is addressed to

### Optionally (explicit opt-in only)

After generating a letter, users may choose to share **only the demand amount** (the dollar figure). This is:

- **Opt-in only** — you must click "Share Amount" explicitly
- **Amount only** — the numeric value (e.g., `5000`), nothing else
- **Not stored with any identifier** — we store only the amount: no user ID, no session ID,
  no IP address, no timestamp. (Cloudflare necessarily sees your IP address to route the
  request, as with any page load; our code does not record it.)
- **One-time** — a single POST request, no ongoing tracking

This data is used solely to understand the general range of demands people create (e.g., "most demands are under $10,000").

## What We Never Collect

Under no circumstances does this application collect, transmit, or store:

- Names (yours or the recipient's)
- Addresses
- Phone numbers, emails, or fax numbers
- The content or subject of your demand
- The generated letter text
- Your IP address (our code never records it)
- Cookies or tracking identifiers
- Analytics scripts of any kind
- Browser fingerprints

## How the Fragment API Preserves Privacy

The app supports pre-filling via URL hash parameters:
```
app.html#to_name=John+Doe&amount=5000
```

The `#` (fragment identifier) portion of a URL is **never sent to the server** in an HTTP request. This is defined by [RFC 3986](https://datatracker.ietf.org/doc/html/rfc3986#section-3.5) and enforced by all browsers.

When you visit the URL above:
- The browser sends: `GET /app.html HTTP/1.1`
- The browser does NOT send: `to_name=John+Doe&amount=5000`
- JavaScript reads the hash client-side via `window.location.hash`

This means even pre-filled links containing sensitive data are private by design.

## Technical Enforcement

Privacy is enforced at the architectural level:

1. **No server-side code**: The app is static files. There is no backend to receive data.
2. **No API calls during form use**: Network tab will show zero requests between page load and download.
3. **No third-party scripts**: No Google Analytics, no Facebook Pixel, no tracking SDKs, and Cloudflare Web Analytics is left off.
4. **Content Security Policy**: CSP headers prevent unauthorized script execution.
5. **Open source**: Anyone can verify these claims by reading the code.

## Verification

You don't have to trust us. Verify it yourself:

1. Open the app in your browser
2. Open Developer Tools (F12) → Network tab
3. Fill out the form and generate a letter
4. Observe: zero network requests were made
5. The download is a local blob URL, not a server fetch

Alternatively, read the source: [GitHub Repository](https://github.com/Cfomodz/Open-Demand-Letter)

## Data Flow Diagram

```
┌──────────────────────────────────────────────────┐
│                  YOUR BROWSER                     │
│                                                   │
│  Form Data ──▶ JS Processing ──▶ Letter HTML     │
│                                      │            │
│                                      ▼            │
│                              File Download        │
│                              (to YOUR device)     │
│                                                   │
│  ┌─────────────────────────────────────────────┐  │
│  │ Optional opt-in:                            │  │
│  │ POST { amount: 5000 } ──────────────────────│──│──▶ Aggregate counter
│  │ (ONLY if you click "Share Amount")          │  │
│  └─────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────┘

Nothing else leaves this box. ▲
```

## Changes to This Policy

This privacy design is a core project principle. Any changes must:
- Be discussed publicly via GitHub issue
- Maintain the "no data leaves the browser" default
- Never make data sharing automatic or opt-out
- Be reflected in updated documentation before code changes
