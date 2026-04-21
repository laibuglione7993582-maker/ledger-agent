# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.1] - 2026-04-21

### Added
- Banner image in README and published tarball.
- README badges: npm version, downloads, bundle size, license, types.
- Architecture diagram explaining the `402 → budget check → retry` flow.
- Features table and structured API reference in the README.
- `repository`, `homepage`, and `bugs` fields in `package.json` so they surface
  on npmjs.com.
- Community files: `SECURITY.md`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`,
  issue and pull request templates.
- GitHub Actions CI that builds the package on every push and PR.
- Dependabot configuration for weekly dependency updates.

### Changed
- README restructured for clarity: what is this → features → quick start →
  how it works → API → tech stack.

## [0.1.0] - 2026-04-21

### Added
- Initial release.
- `createOversight(options)` factory that wraps `fetch` and enforces spend caps.
- `BudgetLimits` with `perCall`, `hourly`, `daily`, and `lifetime` caps.
- `BudgetExceededError` thrown before the paid request when any cap would be
  breached.
- `TransactionRecord` emitted via `onRecord` callback for every outcome
  (success, blocked, error).
- `oversight.snapshot()` for inspecting the running totals and configured
  budget.
- Dual ESM + CJS build with TypeScript declarations.

[Unreleased]: https://github.com/laibuglione7993582-maker/x402-oversight/compare/v0.1.1...HEAD
[0.1.1]: https://github.com/laibuglione7993582-maker/x402-oversight/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/laibuglione7993582-maker/x402-oversight/releases/tag/v0.1.0
