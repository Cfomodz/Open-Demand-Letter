# Product Specification

## Product Name

**Open Demand Letter**

## One-Line Description

A free, private, browser-based tool to generate demand letters — no accounts, no data collection, no nonsense.

## Problem Statement

People who need to send a demand letter face several barriers:
- Legal services are expensive for a simple letter
- Online generators collect personal data and often upsell
- Templates require manual formatting and legal knowledge
- Many people don't know where to start

## Solution

A dead-simple web app that fills in a demand letter template from basic inputs. Everything happens in the browser. The result downloads as a file. Nothing is stored or transmitted.

**Templates, not legal documents.** Every letter is fixed, fill-in-the-blank text. The app fills the blanks; it never drafts or tailors legal language. See [templates/README.md](../templates/README.md) for the template library.

## Target Users

- Individuals owed money by a person or business
- Freelancers with unpaid invoices
- Tenants with security deposit disputes
- Small business owners with contract breaches
- Anyone who needs to formally demand action before considering legal steps

## User Experience

### Landing Page

**Content:**
- Headline explaining the tool (e.g., "Generate a Free Demand Letter")
- 3-4 bullet points on what it does
- Privacy callout ("Your data never leaves your browser")
- "Get Started" button → navigates to app.html
- GitHub icon + link to repo in header/footer
- Legal disclaimer at bottom

**Design:**
- Clean, minimal, professional
- Single page, no scrolling required on desktop
- Mobile responsive

### App Page

**Form Sections:**

#### Recipient ("To")
| Field | Type | Required | Notes |
|-------|------|----------|-------|
| Name | Text | No | Person's full name |
| Company | Text | No | Business/organization name |
| Address | Textarea | No | Full mailing address |
| Phone | Tel | No | Phone number |
| Email | Email | No | Email address |
| Fax | Tel | No | Fax number |
| Additional Info | Textarea | No | Any other relevant info |

#### Sender ("From")
| Field | Type | Required | Notes |
|-------|------|----------|-------|
| Name | Text | No | Person's full name |
| Company | Text | No | Business/organization name |
| Address | Textarea | No | Full mailing address |
| Phone | Tel | No | Phone number |
| Email | Email | No | Email address |
| Fax | Tel | No | Fax number |
| Additional Info | Textarea | No | Any other relevant info |

#### Demand Details
| Field | Type | Required | Notes |
|-------|------|----------|-------|
| Issue | Text/Select | No | Free-text description OR dropdown selection |
| Amount | Currency | No | Dollar amount being demanded |
| Deadline | Date/Number | No | Response deadline (defaults to 30 days) |

**Issue Dropdown Options:**
- *(Write your own)* — default, shows text area
- Unpaid invoice / Money owed
- Breach of contract
- Property damage
- Security deposit not returned
- Defective product / Service not rendered
- Personal injury / Medical bills
- Loan repayment
- Insurance claim dispute
- Warranty claim
- Other (with free-text field)

**Actions:**
- "Generate Letter" button → produces the letter in-browser
- Preview panel showing the formatted letter
- "Download" button → saves file to device
- "Start Over" link → clears form

### Post-Generation

After the letter is generated and ready to download:

1. Letter preview is displayed
2. Download begins automatically (or via button)
3. **Opt-in prompt appears** (non-blocking):
   > "Would you like to anonymously share just the demand amount ($X,XXX)?
   > This helps us understand how this tool is used. No other information is shared."
   >
   > [Share Amount] [No Thanks]
4. If "Share Amount": POST the numeric amount. Show "Thanks!"
5. If "No Thanks": dismiss. No action taken.

## Non-Features (Explicitly Out of Scope)

- User accounts / authentication
- Server-side storage of any kind
- Payment / pricing / premium tiers
- Sending the letter (email, fax, mail) — that's on the user
- Legal advice, or language tailored to the user's facts (templates may include clearly labeled optional sections, such as the California CCPA notice)
- Letter tracking or read receipts
- Free-form letter customization beyond filling the template's blanks
- Multi-language support (English only for v1)

## Legal Disclaimer

The canonical text lives in [DISCLAIMER.md](DISCLAIMER.md). The short version appears in the footer of every page; the full version appears on the app page before the Generate button.

In short: these are letter templates, not legal documents or legal advice; they are provided "as is" with no warranty; no attorney-client relationship is created; and the user is solely responsible for how they use them.
