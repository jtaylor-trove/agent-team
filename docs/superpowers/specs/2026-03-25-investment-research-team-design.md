# Investment Research Agent Team — Design Spec

## Overview

An agent team that researches and evaluates investment opportunities across stocks, sectors, and crypto (XRP specialty). The team uses an iterative research-and-review loop: 3 research agents develop pitches, 3 board agents critically evaluate them, and the cycle repeats until consensus is reached on the most promising opportunities.

Invoked via `/invest <amount>` where `<amount>` is the USD budget the user is prepared to deploy.

## Team Structure

### Research Team (3 agents)

| Role | Home Base | Cross-over Permission |
|------|-----------|----------------------|
| **Equity Analyst** | Individual stocks, earnings, company fundamentals | Crypto-exposed public companies (e.g., Coinbase, MicroStrategy) |
| **Macro/Sector Strategist** | Sector rotations, economic indicators, rate environment, geopolitical catalysts | Crypto macro — regulatory shifts, institutional adoption trends |
| **Crypto/XRP Specialist** | XRP ecosystem (XRPL, partnerships, legal/regulatory), broader crypto market | Equities with significant crypto catalysts or exposure |

Each researcher has a home base but can cross into adjacent territory when they spot a compelling opportunity.

### Review Board (3 agents)

| Role | Lens | Key Questions |
|------|------|---------------|
| **Risk Skeptic** | Downside & fragility | "What kills this trade? What's the max loss? What are we missing?" |
| **Opportunity Evaluator** | Upside & timing | "Why now? How big is the upside? What's the conviction level?" |
| **Portfolio Integrator** | Fit & synthesis | "How does this combine with other picks? Concentration risk? Final ranking?" |

## Workflow

### Phase 1 — Research Sprint

- All 3 researchers work in parallel on their home base territory.
- Each produces 1-3 pitches in the standardized format (see below).
- Researchers can flag cross-over opportunities outside their lane.
- Depth starts light; the board can request deeper dives.

### Phase 2 — Board Review

- Researchers present pitches to all 3 board members simultaneously.
- Each board member evaluates through their specific lens:
  - **Risk Skeptic** scores each pitch on fragility (low/medium/high risk).
  - **Opportunity Evaluator** scores on conviction and timing (weak/moderate/strong).
  - **Portfolio Integrator** considers how pitches fit together as a set.

### Phase 3 — Board Verdict

The board issues one of three outcomes per pitch:

| Verdict | Meaning | What Happens |
|---------|---------|--------------|
| **Advance** | Promising, worth presenting to user | Goes into the final shortlist |
| **Deepen** | Interesting but needs more work | Sent back to researcher with specific questions |
| **Reject** | Not compelling enough | Researcher pivots to a new idea |

### Phase 4 — Iteration

- Researchers address "deepen" feedback and develop new pitches to replace "reject" items.
- Loop back to Phase 2.
- Iteration continues until the board reaches consensus on a shortlist (target: top 2-3 opportunities).

### Phase 5 — Final Presentation

- The Portfolio Integrator synthesizes approved pitches into a ranked recommendation.
- Presented to the user with: thesis summary, bull/bear case, risk assessment, and suggested dollar allocations summing to the budget.

### Exit Conditions

- Board consensus on at least 1 opportunity (ideally 2-3).
- Max 3 iteration cycles to prevent infinite loops. If no consensus after 3 rounds, the board presents the best candidates with noted disagreements.

## Communication Protocol

### Research Phase

- Researchers work independently.
- Optional cross-talk: researchers can message each other to flag cross-over findings (e.g., Equity Analyst spots an XRP-related catalyst and pings the Crypto Specialist).

### Handoff to Board

Researchers send pitches to all 3 board members using the standardized format:

```
PITCH: [Asset/Opportunity Name]
THESIS: [1-2 sentences]
EVIDENCE: [Key data points]
RISKS: [Top 2-3 risks]
TIMEFRAME: [Short/Medium/Long]
CONVICTION: [Low/Medium/High]
```

### Board Deliberation

- Board members message each other to discuss pitches.
- Each shares their assessment through their lens.
- Portfolio Integrator drives toward consensus and drafts the verdict for each pitch.

### Feedback to Researchers

- Board sends structured feedback per pitch (advance/deepen/reject + specific notes).
- "Deepen" verdicts include the exact questions the board wants answered.

### Final Output

- Portfolio Integrator compiles the ranked shortlist.
- The team lead (main Claude session) presents it to the user.

## Skill Invocation

**Name:** `/invest`

**Usage:** `/invest <amount>`

**Examples:**
- `/invest 500` — research opportunities for a $500 budget
- `/invest 10000` — same process, sizing and recommendations scale accordingly

### How Budget Flows Through the Team

- **Researchers** — informed of budget so they filter out opportunities that don't make sense at the given scale. Fractional shares are assumed available.
- **Risk Skeptic** — factors in position sizing risk at this budget level.
- **Opportunity Evaluator** — considers whether upside is meaningful at this scale.
- **Portfolio Integrator** — produces final recommendation with dollar allocations summing to the budget.

### Validation

- Amount must be a positive number.
- Currency is USD unless specified otherwise.
- No amount provided = prompt the user to provide one.

## Assumptions

- Fractional shares are available (most modern brokerages support this).
- Budget is the total to deploy, not per-position.
- Research uses publicly available information via web search and current market context.
- The Portfolio Integrator has tie-breaking authority if the board cannot reach full consensus on a pitch verdict.
- This is not financial advice — the team produces analysis for the user's consideration.
