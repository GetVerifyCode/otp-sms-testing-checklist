# OTP / SMS Verification Testing Checklist

A practical, vendor-neutral checklist for QA engineers and developers who test
SMS one-time-passcode (OTP) flows in staging before release. Maintained by
[GetVerifyCode](https://getverifycode.com/).

## Why this exists

SMS verification breaks in small ways: codes arrive late, expire early, get
rate-limited, or fail on certain country formats. Most of these bugs only show
up with real inbound SMS, not mocks.

## Checklist

### Input and formatting

- [ ] Accepts E.164 numbers (`+14155550100`) and strips spaces/dashes
- [ ] Country picker matches the dialing code actually sent to the SMS provider
- [ ] Clear error for numbers your provider cannot deliver to

### Delivery

- [ ] Code arrives within your target time (measure p50 / p95)
- [ ] Resend button has a cooldown and shows a countdown
- [ ] Message text includes app name and the code; no links in OTP SMS
- [ ] Works with alphanumeric sender IDs and short codes where relevant

### Validation

- [ ] Code expires after the documented window (e.g. 5–10 min)
- [ ] Old code is rejected after a resend
- [ ] Attempts are rate-limited; lockout message is understandable
- [ ] Codes are single-use

### Security and privacy

- [ ] OTPs never appear in logs, analytics or error trackers
- [ ] Verification endpoints are rate-limited per number and per IP
- [ ] Consider stronger factors (passkeys, authenticator apps) for high-risk actions

## Testing with real inbound SMS

For end-to-end tests across countries you can use temporary inbound numbers.
See the [QA checklist write-up](https://getverifycode.com/blog/qa-checklist-sms-otp-testing)
and the [SMS verification API guide](https://getverifycode.com/blog/sms-verification-api-developers).
Only test apps you own or are authorized to test.

## Contributing

Issues and pull requests with additional checks are welcome.

## License

CC BY 4.0
