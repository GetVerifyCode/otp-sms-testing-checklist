# Copy-paste checklist

Use this in tickets or test plans. Interactive version: https://getverifycode.github.io/

## Input and formatting

- [ ] Accepts E.164 numbers (`+14155550100`) and strips spaces / dashes
- [ ] Country picker matches the dialing code actually sent to the SMS provider
- [ ] Clear error for numbers your provider cannot deliver to
- [ ] Leading zeros are handled correctly for the selected country

## Delivery

- [ ] Code arrives within your target time (measure p50 / p95)
- [ ] Resend button has a cooldown and shows a countdown
- [ ] Message text includes the app name and the code
- [ ] OTP SMS contains no tracking links
- [ ] Works with alphanumeric sender IDs and short codes where relevant

## Validation

- [ ] Code expires after the documented window (for example 5–10 minutes)
- [ ] Old code is rejected after a resend
- [ ] Attempts are rate-limited; lockout copy is understandable
- [ ] Codes are single-use
- [ ] Clock skew on the device does not silently invalidate fresh codes

## Security and privacy

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
