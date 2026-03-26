---
name: invest
description: Research investment opportunities with a 6-agent team
---

# Investment Research

Research and evaluate investment opportunities across stocks, sectors, and crypto/XRP using a specialized agent team.

## Usage

```
/invest <amount>
```

Where `<amount>` is the USD budget you are prepared to deploy (e.g., `/invest 500`).

## Validation

- If no amount is provided, ask the user how much they want to invest.
- Amount must be a positive number. If it isn't, ask again.
- Currency is USD unless specified otherwise.

## Workflow

Invoke the **orchestrate-invest** skill with the validated budget amount.
