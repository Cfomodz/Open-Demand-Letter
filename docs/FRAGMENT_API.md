# Fragment API Reference

## Overview

The Fragment API allows pre-filling the demand letter form via URL hash parameters. This enables programmatic use, bookmarklets, integrations, and shareable pre-filled links — all without any data touching the server.

## Why Fragments Instead of Query Parameters?

URL structure: `https://example.com/path?query=params#fragment=params`

| Part | Sent to server? | Visible in server logs? |
|------|-----------------|------------------------|
| Path (`/app.html`) | Yes | Yes |
| Query (`?name=John`) | **Yes** | **Yes** |
| Fragment (`#name=John`) | **No** | **No** |

Per [RFC 3986 §3.5](https://datatracker.ietf.org/doc/html/rfc3986#section-3.5), the fragment identifier is processed purely by the client (browser). It is never included in HTTP requests.

**This means:** You can put sensitive data in the fragment, and it will never appear in Cloudflare logs, server access logs, or any network request to our infrastructure.

## Usage

### Base URL
```
https://opendemandletter.com/app.html#<parameters>
```

### Parameter Format
Standard URL-encoded key=value pairs separated by `&`:
```
#key1=value1&key2=value2&key3=value+with+spaces
```

Values should be URL-encoded (spaces as `+` or `%20`, special characters as `%XX`).

## Parameters

### Recipient Fields

| Parameter | Description | Example |
|-----------|-------------|---------|
| `to_name` | Recipient's full name | `to_name=John+Doe` |
| `to_company` | Recipient's company/org | `to_company=Acme+Corp` |
| `to_address` | Recipient's mailing address | `to_address=123+Main+St%0ACity+ST+12345` |
| `to_phone` | Recipient's phone number | `to_phone=555-123-4567` |
| `to_email` | Recipient's email | `to_email=john%40acme.com` |
| `to_fax` | Recipient's fax number | `to_fax=555-123-4568` |
| `to_info` | Additional recipient info | `to_info=Attn%3A+Legal+Dept` |

### Sender Fields

| Parameter | Description | Example |
|-----------|-------------|---------|
| `from_name` | Sender's full name | `from_name=Jane+Smith` |
| `from_company` | Sender's company/org | `from_company=Smith+LLC` |
| `from_address` | Sender's mailing address | `from_address=456+Oak+Ave%0ATown+ST+67890` |
| `from_phone` | Sender's phone number | `from_phone=555-987-6543` |
| `from_email` | Sender's email | `from_email=jane%40smith.com` |
| `from_fax` | Sender's fax number | `from_fax=555-987-6544` |
| `from_info` | Additional sender info | `from_info=Licensed+Contractor+%23A1234` |

### Demand Fields

| Parameter | Description | Example |
|-----------|-------------|---------|
| `issue` | Issue description (free text) | `issue=Unpaid+invoice+%23456` |
| `issue_type` | Pre-select dropdown category | `issue_type=unpaid_invoice` |
| `amount` | Demand amount (numeric) | `amount=5000` |
| `deadline` | Days until deadline (default: 30) | `deadline=14` |

### Control Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `auto` | Auto-generate and download (`1` = enabled) | `auto=1` |

### Issue Type Values

When using `issue_type` to select a dropdown category:

| Value | Dropdown Label |
|-------|---------------|
| `unpaid_invoice` | Unpaid invoice / Money owed |
| `breach_of_contract` | Breach of contract |
| `property_damage` | Property damage |
| `security_deposit` | Security deposit not returned |
| `defective_product` | Defective product / Service not rendered |
| `personal_injury` | Personal injury / Medical bills |
| `loan_repayment` | Loan repayment |
| `insurance_claim` | Insurance claim dispute |
| `warranty_claim` | Warranty claim |
| `other` | Other |

If both `issue` and `issue_type` are provided, `issue_type` selects the dropdown and `issue` fills the description textarea.

## Examples

### Basic: Pre-fill recipient and amount
```
app.html#to_name=John+Doe&to_company=Acme+Corp&amount=5000
```

### Full: All recipient details with auto-generate
```
app.html#to_name=John+Doe&to_company=Acme+Corp&to_address=123+Main+St%0ASpringfield+IL+62701&to_email=john%40acme.com&from_name=Jane+Smith&issue=Unpaid+invoice+for+consulting+services+rendered+in+January+2026&amount=7500&deadline=14&auto=1
```

### Programmatic: Bookmarklet
```javascript
javascript:void(window.open('https://opendemandletter.com/app.html#to_name='+encodeURIComponent(recipientName)+'&amount='+amount))
```

### Integration: Link from an invoicing app
```html
<a href="https://opendemandletter.com/app.html#to_name=Client+Name&amount=1500&issue=Invoice+%23INV-2026-042+overdue+since+Jan+15">
  Send Demand Letter
</a>
```

## Behavior

- **Unknown parameters** are silently ignored
- **Missing parameters** leave the corresponding form fields empty
- **Malformed values** are used as-is (no server-side validation — the form is forgiving)
- **`auto=1`** will generate the letter and trigger download immediately on page load
  - The form is still shown (pre-filled) so the user can see what was generated
  - The user can modify and re-generate if needed
- **Hash is optionally cleared** from the URL bar after parsing for cleanliness

## Security Notes

- Fragment data never appears in HTTP requests, server logs, or Cloudflare analytics
- Links containing fragments can be safely bookmarked — the data stays local
- Be cautious sharing fragment URLs through channels that might log full URLs (some chat apps may log full URLs including fragments in link previews)
- For maximum privacy, users can fill in the form manually instead of using fragment URLs
