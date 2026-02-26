# Open Demand Letter

**A free, open-source, privacy-first demand letter generator that runs entirely in your browser.**

No accounts. No data collection. No servers storing your information. Your letter never leaves your device.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/View_Source-GitHub-black?logo=github)](https://github.com/Cfomodz/Open-Demand-Letter)

---

## What Is This?

A simple web app that helps you create a demand letter. Fill in what you know, leave blank what you don't, and download a ready-to-send letter. That's it.

**A demand letter is a formal written request** asking someone to fulfill an obligation — typically to pay money owed, fix a problem, or take a specific action. It's often the first step before legal action and can resolve disputes without going to court.

## How It Works

1. **Enter information** (all fields optional):
   - **To**: Name, company, address, phone, email, fax, additional info
   - **From**: Name, company, address, phone, email, fax, additional info
   - **Issue**: Describe in plain language or select from common categories
   - **Amount**: The money being demanded
   - **Deadline**: When you need a response (defaults to 30 days)

2. **Click Generate** — your letter is created instantly in your browser.

3. **Download** — the file saves directly to your device.

Nothing is sent to any server. Ever.

## Privacy

This project was built with a single obsession: **your data stays on your machine**.

- All letter generation happens in JavaScript, in your browser
- No form data is transmitted to any server
- No database, no accounts, no cookies tracking your activity
- The only data that *can* be shared is the **demand amount** — and only if you explicitly opt in after your letter is generated
- Basic web telemetry (page views, country-level geo) is handled automatically by Cloudflare — this contains no personal information and no letter content
- The source code is open for anyone to verify: [View on GitHub](https://github.com/Cfomodz/Open-Demand-Letter)

For the full privacy design, see [docs/PRIVACY.md](docs/PRIVACY.md).

## Fragment API (Link-Based Pre-Fill)

You can pre-fill the form by constructing a URL with hash parameters:

```
https://opendemandletter.com/app.html#to_name=John+Doe&amount=5000&issue=unpaid+invoice
```

**The `#` fragment is never sent to the server** — it's parsed entirely by JavaScript in your browser. This means you can share pre-filled links without any sensitive data touching our infrastructure.

Add `&auto=1` to automatically generate and download the letter on page load.

Full parameter documentation: [docs/FRAGMENT_API.md](docs/FRAGMENT_API.md).

## Tech Stack

Intentionally minimal:

- **HTML** — Semantic, accessible markup
- **CSS** — Mobile-first responsive design, no frameworks
- **Vanilla JavaScript** — No dependencies, no build step, no npm
- **Cloudflare Pages** — Static hosting with automatic HTTPS

No React. No Tailwind. No webpack. No node_modules. Just files that work.

## Running Locally

```bash
# Clone the repo
git clone https://github.com/Cfomodz/Open-Demand-Letter.git
cd Open-Demand-Letter

# Serve with any static file server
python3 -m http.server 8000
# or
npx serve .
# or just open index.html in your browser
```

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](.github/CONTRIBUTING.md) before submitting a PR.

The core principles are non-negotiable:
- Privacy-first (no data leaves the browser)
- Simplicity (no frameworks, no build steps)
- All fields remain optional
- No monetization, no accounts, no upsells

## License

[MIT](LICENSE) — do whatever you want with it.

## Disclaimer

This tool generates a basic demand letter template. **It is not legal advice.** For complex disputes or significant amounts, consult a licensed attorney in your jurisdiction. The generated letter is a starting point, not a substitute for professional legal counsel.
