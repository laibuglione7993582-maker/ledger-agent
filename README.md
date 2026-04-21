<div align="center">

<img src="banner.jpg" alt="x402-oversight" width="100%" />

# x402-oversight

Oversight and governance wrapper for x402 payments.

[![npm version](https://img.shields.io/npm/v/x402-oversight.svg?style=flat-square)](https://www.npmjs.com/package/x402-oversight)
[![npm downloads](https://img.shields.io/npm/dm/x402-oversight.svg?style=flat-square)](https://www.npmjs.com/package/x402-oversight)
[![bundle size](https://img.shields.io/bundlephobia/minzip/x402-oversight?style=flat-square)](https://bundlephobia.com/package/x402-oversight)
[![license](https://img.shields.io/npm/l/x402-oversight.svg?style=flat-square)](./LICENSE)
[![types](https://img.shields.io/npm/types/x402-oversight.svg?style=flat-square)](./dist/index.d.ts)

[npm](https://www.npmjs.com/package/x402-oversight) · [GitHub](https://github.com/laibuglione7993582-maker/x402-oversight)

</div>

---

## What is this?

A drop-in `fetch` replacement for AI agents that transact via the [x402 protocol](https://www.x402.org/). It enforces spend caps *before* any payment leaves the agent and captures a structured record of every transfer — so a runaway agent can't burn through the budget and every paid call is reconstructable after the fact.

For teams deploying agents that spend real money on paid APIs.

---

## Features

| Feature | What it does |
|--------|--------------|
| **Spend caps** | Per-call, hourly, daily, and lifetime USD limits. Rejected *before* the paid request. |
| **Transaction records** | Every outcome (success, blocked, error) emitted with id, agent, endpoint, amount, tx hash, timestamp. |
| **Zero runtime deps** | Just Node 18+ (global `fetch`). No hidden network. |
| **Dual build** | ESM + CJS, with TypeScript declarations in the package. |
| **Pluggable fetch** | Pass your own fetch for tests, proxies, or edge runtimes. |

---

## Install

```bash
npm install x402-oversight
```

Node 18+ (or any runtime with a global `fetch`).

---

## Quick start

```ts
import { createOversight } from "x402-oversight";

const oversight = createOversight({
  agent: "researcher-01",
  budget: {
    perCall: 0.05,
    hourly: 25,
    daily: 100,
    lifetime: 500,
  },
  onRecord: (record) => {
    console.log(
      "[oversight]",
      record.status,
      record.endpoint,
      `$${record.amount.toFixed(3)}`,
    );
  },
});

const { response, record } = await oversight.fetch(
  "https://api.example.com/data",
);

console.log(await response.json());
console.log("spent so far:", oversight.snapshot());
```

---

## How it works

```
agent code
    │
    ▼
oversight.fetch(url)
    │
    ├── first request ────────────► server
    │                                 │
    │                                 ▼
    │                         402 Payment Required
    │                         x-payment-amount-usd: 0.002
    │                                 │
    ▼                                 ▼
check budget caps ─────► BudgetExceededError (blocked, emitted, thrown)
    │ ok
    ▼
retry with x-payment-authorization ─► server ─► 200 OK
    │
    ▼
emit TransactionRecord ─► onRecord()
    │
    ▼
return { response, record }
```

Non-402 responses pass through untouched (with a zero-amount record emitted for the audit trail).

---

## API

### `createOversight(options): Oversight`

| Option | Type | Description |
|--------|------|-------------|
| `agent` | `string` | Identifier for the calling agent. Required. |
| `budget.perCall` | `number?` | Max spend per single call, in USD. |
| `budget.hourly` | `number?` | Max spend per rolling hour, in USD. |
| `budget.daily` | `number?` | Max spend per rolling day, in USD. |
| `budget.lifetime` | `number?` | Hard cap across the lifetime of this oversight instance, in USD. |
| `onRecord` | `(r) => void \| Promise<void>` | Called after every transaction. Use to write into your log store. |
| `fetch` | `typeof fetch` | Custom fetch implementation. Defaults to `globalThis.fetch`. |

### `oversight.fetch(input, init?): Promise<PaymentResponse>`

Same signature as the global `fetch`, but returns `{ response, record }`.

### `oversight.snapshot()`

```ts
{ agent: string, total: number, hourly: number, daily: number, budget: BudgetLimits }
```

### `BudgetExceededError`

Thrown before the paid request is sent when any cap would be breached. Fields: `kind` (which cap), `limit`, `wouldSpend`.

---

## Tech stack

- TypeScript 5
- [tsup](https://tsup.egoist.dev/) — ESM + CJS + declaration output
- Node 18+ global `fetch`
- Zero runtime dependencies

---

## License

MIT
