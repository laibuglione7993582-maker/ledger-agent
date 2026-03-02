---
name: Bug report
about: Report a behaviour that doesn't match the documented API
title: "bug: "
labels: bug
---

### What happened

<!-- A clear and concise description of the bug. -->

### Reproduction

<!-- A minimal code snippet or repo that reproduces the issue. -->

```ts
import { createLedgerAgent } from "ledger-agent";

const agent = createLedgerAgent({ agent: "...", budget: { perCall: 0.01 } });
// ...
```

### Expected behaviour

### Environment

- `ledger-agent` version:
- Node version (`node --version`):
- OS:
- Package manager (npm / pnpm / yarn):

### Additional context
