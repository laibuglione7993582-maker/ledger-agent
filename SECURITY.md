# Security Policy

## Supported versions

Security fixes are applied to the latest minor release on the `main` branch.
Older pre-1.0 versions are not maintained.

| Version | Supported |
| ------- | --------- |
| 0.1.x   | ✅        |
| < 0.1   | ❌        |

## Reporting a vulnerability

If you discover a security issue, please **do not** open a public issue. Instead:

1. Open a private advisory at
   https://github.com/laibuglione7993582-maker/x402-oversight/security/advisories/new
2. Include a description, reproduction steps, affected versions, and (if you have one)
   a suggested fix.

You should receive an initial response within a few business days. Once the issue is
confirmed, we will coordinate a fix, release a patched version, and publish a GitHub
Security Advisory crediting you (unless you prefer to remain anonymous).

## Scope

In scope:
- Bugs that let a caller bypass budget caps
- Incorrect handling of the `402 Payment Required` response
- Leakage of transaction data through unexpected channels (logs, errors)

Out of scope:
- Vulnerabilities in the x402 protocol itself — please report those upstream
- Vulnerabilities in Node.js or npm — please report those upstream
- Issues that require an already-compromised host
