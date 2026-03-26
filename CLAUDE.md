# Investment Research Agent Team

A 6-agent team that researches and evaluates investment opportunities across stocks, sectors, and crypto (XRP specialty). Uses an iterative research-and-review loop until the board reaches consensus on the most promising opportunities.

## Usage

```
/invest <amount>
```

Where `<amount>` is the USD budget to deploy (e.g., `/invest 500`).

## Architecture

`/invest` command → `orchestrate-invest` skill (team lead) → spawns 6 agent teammates

### Agent Roster

| Agent | Role | Skill |
|-------|------|-------|
| Equity Analyst | Research: stocks, earnings, fundamentals | equity-analyst |
| Macro Strategist | Research: sectors, economic indicators, themes | macro-strategist |
| Crypto/XRP Specialist | Research: XRP ecosystem, crypto markets | crypto-specialist |
| Risk Skeptic | Board: downside analysis, fragility scoring | risk-skeptic |
| Opportunity Evaluator | Board: upside assessment, timing, conviction | opportunity-evaluator |
| Portfolio Integrator | Board: synthesis, allocation, consensus (tie-breaker) | portfolio-integrator |

### Workflow

1. **Research Sprint** — 3 researchers work in parallel, produce pitches
2. **Board Review** — 3 board members evaluate through their lenses
3. **Verdict** — Advance / Deepen / Reject per pitch
4. **Iterate** — address feedback, loop back (max 3 cycles)
5. **Present** — Portfolio Integrator compiles ranked recommendations with $ allocations

## Key Rules

- Agent teams must be enabled: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` (set in `.claude/settings.json`)
- This is NOT financial advice — analysis for user consideration only
- Fractional shares assumed available
- Portfolio Integrator has tie-breaking authority on board verdicts
