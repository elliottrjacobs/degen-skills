# degen-skills

AI-powered NBA betting analysis system built on [Claude Code](https://claude.com/claude-code). A team of specialized handicapping agents that provide the same caliber of research professional sports bettors and sharp syndicates use — systematic, data-driven, and disciplined.

> **Advisory only.** This system does not place bets, connect to sportsbooks, or execute transactions. All analysis is for informational and entertainment purposes. You make all final betting decisions.

## How It Works

This is a set of project-local Claude Code skills (slash commands) that orchestrate specialized AI agents. Each command spawns parallel sub-agents with domain expertise — line shopping, statistical modeling, situational analysis, sharp money tracking — then synthesizes their findings into actionable analysis.

The system maintains NBA power ratings, calculates fair lines, identifies edges against the market, and sizes recommendations using Kelly Criterion-based bankroll management.

## Getting Started

```bash
cd ~/degenerates
claude
```

Then run the onboarding wizard:

```
/onboard
```

This sets up your bettor profile — bankroll, sportsbooks, risk tolerance, preferences — and initializes the NBA model with current power ratings.

### Optional: The Odds API

Sign up for a free API key at [the-odds-api.com](https://the-odds-api.com) (500 requests/month) for structured odds data from 40+ sportsbooks. The system falls back to web search if unavailable.

## Commands

### Core Handicapping

| Command | What it does | Agents |
|---------|-------------|--------|
| `/picks` | Full daily slate analysis with bet sizing | 4 parallel (Line Scout, Stat Nerd, Situational Analyst, Sharp Tracker) |
| `/props` | Player prop analysis and scanning | 3 parallel (Usage Modeler, Matchup Analyst, Market Scanner) |
| `/lookup` | Quick single-game take (no report saved) | Inline, no sub-agents |

### Intelligence

| Command | What it does | Agents |
|---------|-------------|--------|
| `/briefing` | Morning overview — injuries, line moves, sharp action, schedule edges | 4 parallel (Injury Wire, Line Movement, Sharp Action, Schedule Scan) |

### Discipline

| Command | What it does | Agents |
|---------|-------------|--------|
| `/journal` | Log bets, settle results, track CLV, review performance | Single agent |
| `/postmortem` | Deep performance analysis over a period | 3 parallel (CLV Tracker, Model Accuracy, Agent Scorer) |

### Model Management

| Command | What it does | Agents |
|---------|-------------|--------|
| `/model` | View, update, or refresh NBA power ratings | Single agent |

## Daily Workflow

```
/briefing          → Morning intelligence scan
/picks             → Full slate analysis with recommendations
/picks --fade      → Same analysis + adversarial counter-argument for each pick
/journal log       → Record bets you place
/journal settle    → Settle results, update bankroll
/postmortem        → Weekly/monthly performance deep dive
```

## Key Concepts

- **Power Ratings** — Offensive/defensive efficiency ratings per team, updated with recency weighting
- **Fair Line** — Model's spread/total estimate from power ratings + home court + rest/travel + injury adjustments
- **Edge** — Difference between model's fair line and market line. Only plays above your minimum threshold are recommended
- **CLV (Closing Line Value)** — Did you beat the closing line? The north-star metric for long-term profitability
- **Kelly Criterion** — Mathematical bet sizing based on edge and bankroll. Default: quarter-Kelly (conservative)

## Project Structure

```
profile/          Bettor profile (bankroll, books, preferences, risk config)
models/           NBA power ratings, fair lines, pace, defense, adjustments
reports/          Agent-generated analysis and picks
briefings/        Daily betting briefings
journal/          Bet ledger, entries, and periodic reviews
memory/           Persistent agent memory and learnings
```

## Disclaimer

*This system is for informational and entertainment purposes only. It does not constitute gambling advice. Sports betting involves substantial risk of loss. Never bet more than you can afford to lose. If you or someone you know has a gambling problem, call 1-800-GAMBLER.*
