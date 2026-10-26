# OTP / SMS Verification Testing Checklist

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Maintained by GetVerifyCode](https://img.shields.io/badge/maintained%20by-GetVerifyCode-3f72af.svg)](https://getverifycode.com/)
[![Docs site](https://img.shields.io/badge/docs-getverifycode.github.io-112d4e.svg)](https://getverifycode.github.io/)

Vendor-neutral QA checklist for engineers who ship SMS one-time-passcode (OTP) flows. Use it in staging before release, when mocks are not enough.

Maintained by [GetVerifyCode](https://getverifycode.com/). Written for real inbound SMS testing, not marketing filler.

| Resource | Link |
| --- | --- |
| Interactive docs | [getverifycode.github.io](https://getverifycode.github.io/) |
| Guides (GitBook) | [getverifycode.gitbook.io/getverifycode-docs](https://getverifycode.gitbook.io/getverifycode-docs/) |
| Copy-paste checklist | [CHECKLIST.md](CHECKLIST.md) |
| Release test matrix | [docs/test-matrix.md](docs/test-matrix.md) |

---

## Table of contents

- [Why this exists](#why-this-exists)
- [How to use it](#how-to-use-it)
- [Checklist](#checklist)
- [Suggested test matrix](#suggested-test-matrix)
- [Testing with real inbound SMS](#testing-with-real-inbound-sms)
- [Pass criteria](#pass-criteria)
- [Repository layout](#repository-layout)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)

---

## Why this exists

SMS verification fails in small, expensive ways:

| Failure mode | What users see |
| --- | --- |
| Slow delivery | Code never appears before they abandon |
| Early expiry | “Invalid or expired code” after a resend |
| Rate limits | Temporary blocks after spam-clicking resend |
| Format mismatch | Country picker and dialing code disagree |
| Log leaks | OTP strings land in analytics or error trackers |

Most of these only show up with **real inbound SMS**, not with mocked providers.

## How to use it

1. Copy [CHECKLIST.md](CHECKLIST.md) into your QA ticket, or use the [interactive docs](https://getverifycode.github.io/).
2. Run it against a staging build that can send real SMS.
3. Record p50 / p95 delivery time for at least one country.
4. Fail the release if any **Security and privacy** item is unchecked.
5. Prefer passkeys or authenticator apps for high-risk actions once the SMS path is proven.

## Checklist

### Input and formatting

- [ ] Accepts E.164 numbers (`+14155550100`) and strips spaces / dashes
- [ ] Country picker matches the dialing code actually sent to the SMS provider
- [ ] Clear error for numbers your provider cannot deliver to
- [ ] Leading zeros are handled correctly for the selected country

### Delivery

- [ ] Code arrives within your target time (measure p50 / p95)
- [ ] Resend button has a cooldown and shows a countdown
- [ ] Message text includes the app name and the code
- [ ] OTP SMS contains no tracking links
- [ ] Works with alphanumeric sender IDs and short codes where relevant

### Validation

- [ ] Code expires after the documented window (for example 5–10 minutes)
- [ ] Old code is rejected after a resend
- [ ] Attempts are rate-limited; lockout copy is understandable
- [ ] Codes are single-use
- [ ] Clock skew on the device does not silently invalidate fresh codes

### Security and privacy

- [ ] OTPs never appear in logs, analytics, or error trackers
- [ ] Verification endpoints are rate-limited per number and per IP
- [ ] Codes are stored hashed (or equivalent) with a hard expiry
- [ ] Stronger factors (passkeys, authenticator apps) are offered for high-risk actions

## Suggested test matrix

| Scenario | Assert |
| --- | --- |
| Domestic long number | Code arrives within SLO; E.164 paste works |
| International number | Country picker and dial code stay aligned |
| Immediate resend | Cooldown shows; previous code is rejected |
| Expired code | Clear expired/invalid message; new code works |
| Provider reject | Readable error; no silent hang |
| Log / analytics spot check | OTP string never appears in events or traces |

Full printable sheet: [docs/test-matrix.md](docs/test-matrix.md).

## Testing with real inbound SMS

For end-to-end tests across countries, temporary inbound numbers help you exercise live carrier routes without tying QA to personal SIMs.

- Deep dive: [QA checklist for SMS OTP testing](https://getverifycode.com/blog/qa-checklist-sms-otp-testing)
- API angle: [SMS verification API for developers](https://getverifycode.com/blog/sms-verification-api-developers)
- Product: [GetVerifyCode](https://getverifycode.com/)

Only test apps you own or are authorized to test. Follow each platform’s terms and local law.

## Pass criteria

Treat the release as **not ready** if:

- Any security / privacy box is unchecked, or
- p95 delivery exceeds your product SLO, or
- Resend does not invalidate the previous code

## Repository layout

```text
.
├── README.md                 # Overview and full checklist
├── CHECKLIST.md              # Copy-paste version for tickets
├── CONTRIBUTING.md
├── SECURITY.md
├── LICENSE
├── docs/test-matrix.md       # Release matrix template
└── .github/ISSUE_TEMPLATE/   # Checklist item proposal form
```

## Contributing

Issues and pull requests with additional checks are welcome. Prefer concrete, reproducible items over marketing language. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Security

See [SECURITY.md](SECURITY.md). Do not open public issues with OTP samples or secrets.

## License

[CC BY 4.0](LICENSE)
