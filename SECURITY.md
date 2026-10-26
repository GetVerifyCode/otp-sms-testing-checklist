# Security policy

## Scope

This repository is a public QA checklist. It does not process production credentials, phone numbers, or OTP codes.

## Reporting

If you find a security issue in GetVerifyCode itself, email **abuse@getverifycode.com** or **support@getverifycode.com**. Do not open a public issue with secrets, OTP samples, or customer data.

## Checklist guidance

When you apply this checklist to your own app:

- Never log OTP codes
- Rate-limit verification endpoints
- Prefer passkeys or authenticator apps for high-risk actions
