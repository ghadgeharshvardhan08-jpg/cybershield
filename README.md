# Cyber Shield Analyzer

An educational URL risk analyzer that checks a link for common phishing-style warning signs and returns an explainable risk score from 0 to 100.

> **Static analysis only.** Cyber Shield reads the text of a URL. It never opens the link, so nothing is visited or downloaded. A low score does **not** prove a site is safe.

### Medium risk result (phishing-style URL)
![Medium risk assessment](screenshots/medium-risk.png)

### Findings breakdown
![Findings](screenshots/findings.png)

### Low risk result (legitimate domain)
![Low risk assessment](screenshots/low-risk.png)

## Features

- Analyzes a pasted URL without visiting it
- Gives a 0-100 risk score with a risk level (Low / Medium / High)
- Shows each finding with its category, severity, and points
- Redacts query-string values (for example `user=[REDACTED]`) to protect privacy
- Clear disclaimers that the result is preliminary, not a guarantee

## How the scoring works

Each rule that matches adds points to the total. The score is capped at 100.

| Finding | Severity | Points | What it checks |
|---|---|---|---|
| `INSECURE_TRANSPORT` | Medium | +15 | The URL uses `http://` instead of `https://` |
| `SUSPICIOUS_KEYWORDS` | Low | +8 | Wording like `login`, `signin`, `verify`, `account` |
| `MISLEADING_DOMAIN` | High | +20 | A known brand name appears in the hostname, but the hostname does not end in the brand's real domain |

**Example:** `http://paypal-login.verify-account.example.com/signin` triggers all three rules: 15 + 8 + 20 = **43 (Medium risk)**.

## Example results

| URL | Result |
|---|---|
| `https://paypal.com` | 0 / Low, no findings |
| `http://paypal-login.verify-account.example.com/signin?user=test` | 43 / Medium, 3 findings |

## Tech stack

<!-- Replace with what you actually used -->
- Frontend: _e.g. React, Vite, Tailwind CSS_
- Backend: _e.g. Node.js, Express_
- Database: _e.g. PostgreSQL / none_
- Built and first deployed on Replit

## Getting started

```bash
# 1. Clone the repository
git clone https://github.com/ghadgeharshvardhan08-jpg/cybershield.git
cd cybershield

# 2. Install dependencies
npm install

# 3. Set up environment variables
cp .env.example .env
# then edit .env with your own values

# 4. Run the app
npm run dev
```

<!-- Adjust the commands above to match your project (npm / pnpm / pip, etc.) -->

## Testing

Try these URLs to check the detection logic:

| URL | Expected |
|---|---|
| `https://example.com` | Low risk |
| `https://paypal.com` | Low risk, no findings |
| `https://www.paypal.com` | Low risk (legitimate subdomain) |
| `http://paypal-login.verify-account.example.com/signin` | Medium risk |
| `https://paypal.com.evil-site.example/login` | Flagged (misleading domain) |
| `http://192.0.2.1/secure/update-password.php` | Flagged |
| `http://example.com/login@bank.com` | Flagged |
| empty input / plain text | Clear error message |

All example domains use reserved test addresses (`example.com`, `192.0.2.x`), so none lead to real sites.

## Limitations

- Rule-based and limited to the patterns listed above
- Does not check reputation databases, page content, or SSL certificates
- Can produce false positives and false negatives
- Not a replacement for a professional security tool or audit

## Roadmap

- [ ] Detect IP addresses used as hostnames
- [ ] Detect the `@` trick in URLs
- [ ] Detect punycode / lookalike characters
- [ ] Flag URL shorteners and unusual TLDs
- [ ] Add a bonus when brand impersonation and login keywords appear together

## Disclaimer

This is a learning project. Results are automated and preliminary, and they are not a guarantee, a confirmed threat, or a professional security audit.

## License

MIT. See the [LICENSE](LICENSE) file.
