# SMS OTP test matrix

Minimal matrix for a release candidate. Expand countries to match your launch markets.

| # | Scenario | Country sample | Expected result | Pass? |
| --- | --- | --- | --- | --- |
| 1 | Happy path, first send | US or local HQ | Code arrives within SLO | |
| 2 | Happy path, international | UK / DE / IN | E.164 formatting correct | |
| 3 | Resend cooldown | Any | UI blocks spam resend | |
| 4 | Resend invalidates old code | Any | Old code rejected | |
| 5 | Expired code | Any | Clear expired/invalid copy | |
| 6 | Wrong country dial code | Any | User-facing error, no hang | |
| 7 | Provider cannot deliver | Unsupported range | Clear error | |
| 8 | Analytics / logs spot check | Any | OTP absent from logs/events | |

## Notes

- Record p50 and p95 delivery time for row 1.
- Fail the release if row 8 fails.
- Only test apps you own or are authorized to test.

Maintained by [GetVerifyCode](https://getverifycode.com/).
