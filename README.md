# degen-skills

AI-powered sports betting analysis system built on [Claude Code](https://claude.com/claude-code). A team of specialized handicapping agents that provide the same caliber of research professional sports bettors and sharp syndicates use — systematic, data-driven, and disciplined.

> **Advisory only.** This system does not place bets, connect to sportsbooks, or execute transactions. All analysis is for informational and entertainment purposes. You make all final betting decisions.

## How It Works

This is a set of project-local Claude Code skills (slash commands) that orchestrate specialized AI agents. Each command spawns parallel sub-agents with domain expertise — line shopping, statistical modeling, situational analysis, sharp money tracking — then synthesizes their findings into actionable analysis.

The system maintains power ratings per sport (NBA, NHL), calculates fair lines, identifies edges against the market, and sizes recommendations using Kelly Criterion-based bankroll management. The `/lookup` command works for any sport — even obscure markets like Olympics hockey.

## Getting Started

```bash
git clone https://github.com/elliottrjacobs/degen-skills.git
cd degen-skills
claude
```

Then run the onboarding wizard:

```
/onboard
```

This sets up your bettor profile — bankroll, sportsbooks, risk tolerance, sport selection, preferences — and initializes models with current power ratings for your chosen sports.

### Optional: The Odds API

Sign up for a free API key at [the-odds-api.com](https://the-odds-api.com) (500 requests/month) for structured odds data from 40+ sportsbooks. The system falls back to web search if unavailable.

## Commands

### NBA Handicapping

| Command | What it does | Agents |
|---------|-------------|--------|
| `/nba-picks` | Full daily NBA slate analysis with bet sizing | 4 parallel (Line Scout, Stat Nerd, Situational Analyst, Sharp Tracker) |
| `/nba-props` | NBA player prop analysis and scanning | 3 parallel (Usage Modeler, Matchup Analyst, Market Scanner) |
| `/nba-briefing` | NBA morning overview — injuries, line moves, sharp action, schedule edges | 4 parallel (Injury Wire, Line Movement, Sharp Action, Schedule Scan) |
| `/nba-model` | View, update, or refresh NBA power ratings | Single agent |

### NHL Handicapping

| Command | What it does | Agents |
|---------|-------------|--------|
| `/nhl-picks` | Full daily NHL slate analysis with bet sizing | 4 parallel (Line Scout, Stat Modeler, Goaltender Analyst, Sharp Tracker) |
| `/nhl-props` | NHL player prop analysis and scanning | 3 parallel (Production Modeler, Matchup Analyst, Market Scanner) |
| `/nhl-briefing` | NHL morning overview — goalie confirmations, injuries, line moves, sharp action | 4 parallel (Goalie Confirmations, Injury Wire, Line Movement, Sharp Action/Schedule) |
| `/nhl-model` | View, update, or refresh NHL power ratings and goaltender ratings | Single agent |

### Cross-Sport Tools

| Command | What it does | Agents |
|---------|-------------|--------|
| `/lookup` | Quick single-game take for ANY sport (no report saved) | Inline, no sub-agents |
| `/journal` | Log bets, settle results, track CLV, review performance (all sports) | Single agent |
| `/postmortem` | Deep performance analysis over a period (filterable by sport) | 3 parallel (CLV Tracker, Model Accuracy, Agent Scorer) |
| `/onboard` | Setup bankroll, books, sport selection, preferences, risk config | Single agent |

## Daily Workflow

```
/nba-briefing      → NBA morning intelligence scan
/nhl-briefing      → NHL morning intelligence scan
/nba-picks         → Full NBA slate analysis with recommendations
/nhl-picks         → Full NHL slate analysis with recommendations
/nba-picks --fade  → Same analysis + adversarial counter-argument for each pick
/lookup NHL TOR vs MTL  → Quick take on any game, any sport
/journal log       → Record bets you place
/journal settle    → Settle results, update bankroll
/postmortem        → Weekly/monthly performance deep dive
```

## Key Concepts

- **Power Ratings** — Per-sport efficiency ratings per team, updated with recency weighting
- **Fair Line** — Model's line estimate from power ratings + home advantage + rest/travel + injury/goaltender adjustments
- **Edge** — Difference between model's fair line and market line. Only plays above your minimum threshold are recommended
- **CLV (Closing Line Value)** — Did you beat the closing line? The north-star metric for long-term profitability
- **Kelly Criterion** — Mathematical bet sizing based on edge and bankroll. Default: quarter-Kelly (conservative)
- **Goaltender Matchup (NHL)** — The single most important factor in NHL betting. Starting goalie confirmation can swing lines 30-50 cents

## Project Structure

```
profile/          Bettor profile (bankroll, books, preferences, risk config)
models/nba/       NBA power ratings, fair lines, pace, defense, adjustments
models/nhl/       NHL power ratings, goaltender ratings, advanced stats, adjustments
reports/          Agent-generated analysis and picks (per sport)
briefings/        Daily betting briefings (per sport)
journal/          Bet ledger, entries, and periodic reviews (multi-sport)
memory/           Persistent agent memory and learnings
```

## Disclaimer

*This system is for informational and entertainment purposes only. It does not constitute gambling advice. Sports betting involves substantial risk of loss. Never bet more than you can afford to lose. If you or someone you know has a gambling problem, call 1-800-GAMBLER.*
