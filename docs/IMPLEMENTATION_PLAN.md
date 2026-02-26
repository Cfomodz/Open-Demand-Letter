# Implementation Plan

## Phase 1: Foundation (MVP)

**Goal:** A working demand letter generator that can be used end-to-end.

### 1.1 Landing Page (`index.html`)
- [ ] Clean, minimal HTML structure
- [ ] Headline + description + privacy callout
- [ ] "Get Started" CTA button → links to `app.html`
- [ ] GitHub icon + link to repo (visible in header or footer)
- [ ] Legal disclaimer
- [ ] Responsive CSS (mobile-first)
- [ ] Favicon

### 1.2 App Page Structure (`app.html`)
- [ ] Form layout with three sections: Recipient, Sender, Demand Details
- [ ] All form fields as specified in PRODUCT_SPEC.md (all optional)
- [ ] Issue field: toggleable between free-text and dropdown
- [ ] Dropdown populated with common issue categories
- [ ] Amount field with currency formatting
- [ ] Deadline field with 30-day default
- [ ] "Generate Letter" button
- [ ] GitHub icon + link (consistent with landing page)
- [ ] Legal disclaimer
- [ ] Responsive CSS matching landing page

### 1.3 Letter Generation (`js/letter.js`)
- [ ] Basic demand letter template with professional formatting
- [ ] Conditional sections: skip blank fields gracefully
- [ ] Date formatting (current date for letter date)
- [ ] Currency formatting for amount
- [ ] Deadline calculation (user-specified or +30 days from generation)
- [ ] Address block formatting
- [ ] Issue description integration
- [ ] Return formatted HTML string

### 1.4 Core App Logic (`js/app.js`)
- [ ] Form submission handler (prevent default, no server POST)
- [ ] Read all form values
- [ ] Pass to letter generator
- [ ] Display preview in-page
- [ ] "Download" button activation
- [ ] "Start Over" functionality (clear form + preview)

### 1.5 Download (`js/download.js`)
- [ ] Generate HTML file with inline styles (print-ready)
- [ ] Create Blob from HTML string
- [ ] Trigger download via `<a>` element + `URL.createObjectURL`
- [ ] Auto-name file: `demand-letter-YYYY-MM-DD.html`
- [ ] Auto-trigger download after generation

## Phase 2: Fragment API

**Goal:** Enable link-based pre-filling for programmatic/integration use.

### 2.1 Hash Parser (`js/fragment-api.js`)
- [ ] Parse `window.location.hash` on DOMContentLoaded
- [ ] URL-decode key=value pairs
- [ ] Map known keys to form field IDs
- [ ] Populate form fields with parsed values
- [ ] Handle `auto=1` parameter (auto-generate + download)
- [ ] Ignore unknown parameters gracefully
- [ ] Clear hash from URL bar after parsing (optional, for cleanliness)

### 2.2 Documentation
- [ ] Write docs/FRAGMENT_API.md with full parameter reference
- [ ] Add usage examples
- [ ] Note privacy properties of fragment identifiers

## Phase 3: Opt-In Telemetry

**Goal:** Allow users to optionally share demand amount for aggregate stats.

### 3.1 Opt-In UI (`js/telemetry.js`)
- [ ] After letter generation, show non-blocking prompt
- [ ] Clear language: "Share just the amount anonymously?"
- [ ] Two buttons: "Share Amount" / "No Thanks"
- [ ] If opted in: single POST with `{ amount: <number> }`
- [ ] Endpoint: Cloudflare Worker (simple append to KV or D1)
- [ ] Show "Thanks!" confirmation, then dismiss
- [ ] If declined: dismiss immediately, no action

### 3.2 Cloudflare Worker (Telemetry Endpoint)
- [ ] Minimal Worker: accept POST, validate it's a number, store
- [ ] No logging of IP, headers, or any identifying info
- [ ] Rate limiting to prevent abuse
- [ ] Simple aggregate storage (total count + amount buckets)

## Phase 4: Polish

**Goal:** Refine UX, accessibility, and cross-browser support.

### 4.1 Accessibility
- [ ] ARIA labels on all form fields
- [ ] Keyboard navigation support
- [ ] Screen reader testing
- [ ] Sufficient color contrast (WCAG AA)
- [ ] Focus management during form → preview → download flow

### 4.2 Cross-Browser Testing
- [ ] Chrome (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Edge (latest)
- [ ] Mobile Safari (iOS)
- [ ] Chrome for Android

### 4.3 Print Styles
- [ ] Downloaded HTML letter has clean print stylesheet
- [ ] Proper margins, font sizes for standard letter paper
- [ ] No UI elements in print output

### 4.4 Error Handling
- [ ] Graceful handling of empty form (generate letter with minimal content)
- [ ] Invalid amount formatting → gentle correction
- [ ] Fragment API with malformed params → ignore and continue

## Phase 5: Future Considerations (Not Committed)

These are ideas, not plans. They may never be implemented.

- PDF generation via client-side library (jsPDF or similar)
- Multiple letter templates (formal, informal, final notice)
- Multi-language support
- "Share via link" button that constructs a fragment API URL
- Dark mode
- Letter preview with edit-in-place capability

## Definition of Done (Per Phase)

- [ ] All checklist items complete
- [ ] Tested in at least Chrome and Firefox
- [ ] No network requests contain user data (verified via DevTools)
- [ ] Mobile responsive
- [ ] GitHub link visible
- [ ] Legal disclaimer present
- [ ] Code is clean, commented only where non-obvious
