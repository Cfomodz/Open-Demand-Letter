# Templates

Fill-in-the-blank letter templates. Each one is fixed text with `[Bracketed]` blanks. The
user fills the blanks; the app does not rewrite the letter. Sections marked `OPTIONAL
SECTION` are included only when they apply.

See [docs/DISCLAIMER.md](../docs/DISCLAIMER.md). These are templates, not legal
documents or legal advice.

| File | Use when | Main legal basis |
|------|----------|------------------|
| `TCPA.md` | Two or more robocalls within 12 months to a number on the Do Not Call Registry | 47 U.S.C. § 227(b), (c)(5) |
| `illegal_lead_gen.md` | Add-on section for `TCPA.md` when the caller bought your info as a lead | 47 C.F.R. § 64.1200 |
| `maximum.md` | Script for small claims court, not a letter: asking for $1,500 per call | 47 U.S.C. § 227(b)(3), (c)(5) |
| `data_breach.md` | A company exposed your personal information | General demand; optional CCPA notice |
| `data_breach_sensitive.md` | The exposed data included your SSN, card numbers, ID documents, or financial records | General demand; optional CCPA notice |
| `data_breach_phi.md` | A provider, health plan, or their vendor exposed your health information | HIPAA (45 C.F.R. Part 164) |

## Notes for users (shown in the app, never in the letter)

### TCPA
- Damages are $500 per violation, up to $1,500 per violation if willful or knowing.
  Some courts allow a single robocall to a Do Not Call number to count as a violation of
  both § 227(b) and § 227(c); others don't.
- The Do Not Call claim (§ 227(c)(5)) needs more than one call from or for the same
  company within 12 months. The robocall claim (§ 227(b)) does not.
- Companies get up to 31 days to remove newly registered numbers from their lists, so
  calls in your first month on the Registry may not count.
- Keep evidence: screenshots of your call log, voicemails, and your Registry
  confirmation (from donotcall.gov).

### Data breaches
- There is no general federal law that lets individuals sue over a data breach. These
  letters ask for information, reimbursement, and protection services. What you can
  recover depends on your state and your losses.
- **California residents:** include the optional CCPA section. Under Civil Code
  § 1798.150, you must give the company 30 days' written notice before suing for
  statutory damages, and this letter can serve as that notice.
- **HIPAA:** individuals cannot sue under HIPAA itself, but the access and accounting
  rights in the PHI letter are real, enforceable obligations. You can file a complaint
  with HHS OCR, usually within 180 days of when you learned of the problem.
- Free steps to take now: freeze your credit at Equifax, Experian, and TransUnion (free
  by federal law), and use IdentityTheft.gov if your information has been misused.

### Rules every template follows
- **Never trade silence for money.** No template offers to skip reporting to an agency,
  or threatens criminal charges, in exchange for payment. In many states that can be
  extortion. The letters say instead that the right to report is reserved *regardless*
  of the response.
- **Cite the statute first.** Regulations and agency guidance are cited only as
  supporting authority (see *McLaughlin Chiropractic v. McKesson*, 2025).
