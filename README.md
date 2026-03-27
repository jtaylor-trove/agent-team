# Investment Research Agent Team

A multi-agent system built on [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that researches and evaluates investment opportunities. Six specialized agents — three researchers and three board members — collaborate through an iterative review loop to produce portfolio recommendations.

> **Disclaimer:** This tool produces research analysis for consideration only. It is not financial advice.

## Quick Start

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed
- Agent teams enabled (already configured in `.claude/settings.json`)

### Run

```
/invest <amount>
```

Where `<amount>` is the USD budget to deploy:

```
/invest 500
/invest 10000
```

## How It Works

The `/invest` command launches a team lead (the `orchestrate-invest` skill) that coordinates six agents through a structured research-and-review process.

### The Team

**Research Agents** work in parallel, each covering their domain:

| Agent | Domain |
|-------|--------|
| Equity Analyst | Stocks, earnings, fundamentals |
| Macro Strategist | Sectors, economic indicators, themes |
| Crypto/XRP Specialist | XRP ecosystem, crypto markets |

**Board Members** evaluate the researchers' pitches:

| Agent | Lens |
|-------|------|
| Risk Skeptic | Downside analysis, fragility scoring |
| Opportunity Evaluator | Upside assessment, timing, conviction |
| Portfolio Integrator | Synthesis, allocation, consensus (tie-breaker) |

### Workflow

```
Research Sprint ──> Board Review ──> Verdict ──> Iterate (max 3x) ──> Final Report
     │                   │              │
  3 researchers      3 board members    ├── Advance → shortlist
  work in parallel   evaluate pitches   ├── Deepen  → back to researcher
                                        └── Reject  → pivot to new idea
```

1. **Research Sprint** — Researchers scan their domains and produce investment pitches
2. **Board Review** — Board members evaluate every pitch through their specialized lens
3. **Verdict** — Each pitch is advanced, sent back for deeper research, or rejected
4. **Iterate** — Researchers address feedback; the cycle repeats (up to 3 rounds)
5. **Final Report** — Portfolio Integrator compiles ranked recommendations with dollar allocations
6. **Output** — A standalone HTML report is saved to `outputs/`

## Output

Each run produces an HTML report saved to:

```
outputs/invest-<amount>-<date>.html
```

The report includes board verdicts, position details (thesis, bull/bear cases, risk), allocation breakdown, key dates, trim/exit triggers, and macro context. Open it in any browser — no dependencies needed.

> Note: `outputs/` is gitignored since reports contain time-sensitive market data.

## Project Structure

```
.claude/
  settings.json            # Agent teams env var
  skills/
    orchestrate-invest/    # Team lead — coordinates the full workflow
    equity-analyst/        # Research: stocks & fundamentals
    macro-strategist/      # Research: sectors & macro
    crypto-specialist/     # Research: crypto/XRP
    risk-skeptic/          # Board: downside analysis
    opportunity-evaluator/ # Board: upside assessment
    portfolio-integrator/  # Board: synthesis & allocation
outputs/                   # Generated HTML reports (gitignored)
docs/                      # Design specs and implementation plans
```

## Configuration

Agent teams are enabled via the environment variable in `.claude/settings.json`:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

This is already set in the repo — no additional configuration needed.

## Key Design Decisions

- **Fractional shares assumed** — Allocations aren't constrained to whole-share prices
- **Portfolio Integrator breaks ties** — When the board splits, this agent has final say
- **Max 3 iteration cycles** — Prevents endless loops; after 3 rounds the board presents best candidates with noted disagreements
- **Researchers pitch, board decides** — Clean separation between analysis and evaluation prevents confirmation bias
