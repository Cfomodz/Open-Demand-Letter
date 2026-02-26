# Contributing to Open Demand Letter

Thanks for your interest in contributing! This project values simplicity and privacy above all else.

## Core Principles (Non-Negotiable)

Before contributing, understand that these principles cannot be compromised:

1. **Privacy-first**: No user data ever leaves the browser (except the optional opt-in amount)
2. **No frameworks**: Vanilla HTML, CSS, and JavaScript only. No React, no Tailwind, no npm.
3. **No build step**: The app is static files served directly. No webpack, no vite, no compilation.
4. **All fields optional**: Never make any form field required.
5. **No monetization**: No pricing pages, no premium tiers, no upsells, no ads.
6. **No third-party scripts**: No analytics SDKs, no external JavaScript.

If your contribution requires violating any of these, it will not be accepted.

## How to Contribute

### Reporting Bugs

1. Open a [GitHub Issue](https://github.com/Cfomodz/Open-Demand-Letter/issues/new?template=bug_report.md)
2. Include: browser/OS, steps to reproduce, expected vs actual behavior
3. Screenshots or DevTools console output are helpful

### Suggesting Features

1. Open a [GitHub Issue](https://github.com/Cfomodz/Open-Demand-Letter/issues/new?template=feature_request.md)
2. Explain the use case (not just the feature)
3. Consider whether it aligns with the core principles above

### Submitting Code

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Make your changes
4. Test in at least Chrome and Firefox
5. Verify no network requests contain user data (DevTools → Network tab)
6. Submit a Pull Request

### Pull Request Guidelines

- Keep PRs focused — one feature or fix per PR
- Describe what changed and why
- If adding JavaScript, ensure it works without a build step
- If touching the form, test with all fields empty, all filled, and partial fills
- If touching download functionality, test the downloaded file renders correctly

## Code Style

- **HTML**: Semantic elements, proper indentation (2 spaces)
- **CSS**: Mobile-first, BEM-ish naming, 2-space indentation
- **JavaScript**: ES6+, 2-space indentation, no semicolons or semicolons — just be consistent with existing code
- **Comments**: Only where logic is non-obvious. The code should speak for itself.

## Development Setup

```bash
git clone https://github.com/Cfomodz/Open-Demand-Letter.git
cd Open-Demand-Letter
python3 -m http.server 8000
# Open http://localhost:8000
```

That's it. No `npm install`. No build commands.

## Testing Checklist

Before submitting a PR, verify:

- [ ] Form works with all fields empty
- [ ] Form works with all fields filled
- [ ] Form works with various partial fills
- [ ] Letter generates correctly
- [ ] Download works in Chrome
- [ ] Download works in Firefox
- [ ] Fragment API pre-filling works (if relevant)
- [ ] No network requests contain user data
- [ ] Mobile layout looks correct
- [ ] GitHub link is visible
- [ ] Legal disclaimer is present

## Questions?

Open an issue. Keep it simple.
