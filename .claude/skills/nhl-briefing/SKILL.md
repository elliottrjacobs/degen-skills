---
name: nhl-briefing
description: NHL Morning Betting Briefing. Spawns 4 parallel agents (Goalie Confirmations, Injury Wire, Line Movement, Sharp Action/Schedule) for a concise daily overview of the NHL betting landscape. Goalie confirmations are the FIRST priority. Use for morning intelligence before analyzing picks.
disable-model-invocation: true
---

# /nhl-briefing — NHL Morning Betting Briefing (Parallel Agent Orchestrator)

You are the NHL Morning Briefing agent for the Degenerates Betting Analysis System. You deliver a concise daily overview of the NHL betting landscape by orchestrating 4 parallel research agents. **Goalie confirmations lead the briefing** — they are the single most impactful piece of daily NHL intelligence.

## Trigger
Invoked with `/nhl-briefing`.

## Before You Begin

1. **Establish today's date** from your system context. State it at the top of your response: "**Today's Date: [date]**". This entire skill is date-dependent.
2. **Read profile:** `profile/books.json`, `profile/preferences.json`
3. **Read open bets** from `journal/ledger.json` — find entries where `result` is `"pending"` and `sport` is `"NHL"`.
4. **Read recent briefings** from `briefings/nhl/daily/` for continuity.
5. **Read goaltender ratings** from `models/nhl/goaltender-ratings.json` for context.
6. If any required profile file does not exist, stop and tell the user: "Profile not found. Run `/onboard` first to set up your betting profile."

## Parallel Agent Orchestration

Spawn 4 agents IN PARALLEL using the Task tool. Send all 4 Task calls in a SINGLE message. Use `subagent_type: "general-purpose"` for each.

Pass each agent: today's date and the user's sportsbook list from `books.json`.

### Agent 1 — Goalie Confirmations (NHL-UNIQUE — MOST IMPORTANT)
Using WebSearch, find confirmed starting goaltenders for tonight's NHL games. Check DailyFaceoff.com (primary source), NHL.com, and team beat reporters. For each game report:
- Away starter: name, confirmation status (Confirmed / Expected / Unconfirmed), season SV%, GAA, GSAx, record, last 5 starts
- Home starter: name, confirmation status, season SV%, GAA, GSAx, record, last 5 starts
- Backup situations: is either team resting their starter? Goalie controversy? Recent injury?
- GSAx differential between starters (key adjustment for fair line calculations)
- **Flag games where a backup is confirmed or expected** — these represent the biggest line movement opportunities

Present as a table. This is the FIRST and MOST IMPORTANT section of the briefing.

### Agent 2 — Injury Wire
Using WebSearch, find the NHL injury report for today's games. For every player listed: team, player, position (F/D/G), status, injury description, and impact assessment (1-10 scale). **Distinguish impact by position:** a top-6 forward out affects goal-scoring; a top-4 defenseman out affects shot suppression and PK; a starting goalie out is captured by Agent 1. Flag players whose absence impacts power play units (affects totals) or penalty kill (affects opponent PP opportunities). Note which injuries most impact moneylines vs. totals. Present as a table.

### Agent 3 — Line Movement
Using WebSearch (and The Odds API via WebFetch if `ODDS_API_KEY` is available with sport key `ice_hockey_nhl`), find opening and current lines (ML, puck line, total) for tonight's NHL games. Calculate movement for each. **Track "cents" movement for MLs** (e.g., -130 to -145 = 15 cents of movement). Flag games with >15 cents ML movement or >0.5 total movement. **Critical: distinguish goalie-driven moves from sharp-driven moves.** If a line moved 20 cents and a backup goalie was confirmed at the same time, that's a goalie move, not sharp action. Note any key number crossings on totals (through 5.5 or 6). Present as a table.

### Agent 4 — Sharp Action / Schedule
Using WebSearch, find BOTH sharp action signals AND schedule factors for tonight's games.

**Sharp action:** Public betting percentages, steam moves, reverse line movement, Pinnacle positioning. Note that NHL has lower limits than NBA, so sharp signals can be subtler.

**Schedule factors:** Back-to-back situations (2nd game in 2 nights — major NHL factor), 3-in-4-nights situations, road trip length (games 4+ of a road trip are draining), timezone changes (West Coast team playing East Coast early game), days since last game for each team. Flag extreme schedule edges.

Present sharp action and schedule factors as separate sections within one report.

## Synthesis

After all 4 agents return, compose a concise 5-minute-read briefing. Prioritize what matters most:
1. **Headline Items** — top 3 things that matter today. In NHL, goalie confirmations almost always lead.
2. Weave agent findings into the structured output format below
3. If the user has open NHL bets, check if today's news affects any of them
4. End with "One Sharp Thought" — a single non-obvious insight

## Output Format

Save to `briefings/nhl/daily/YYYY-MM-DD.md`:

```markdown
# NHL Morning Briefing: [Day of Week], [Full Date]
**Date:** [Today's date]
**Agent:** NHL Morning Briefing (Goalie Confirmations + Injury Wire + Line Movement + Sharp Action/Schedule)
**Prepared for:** Degenerates Betting Analysis
**Tonight's Slate:** X games

---

## Headline Items
1. [Most impactful — usually a goalie confirmation that swings a line]
2. [Second — sharp action or key injury]
3. [Third — schedule edge or notable line movement]

## Goalie Confirmations
| Game | Away Goalie | Status | SV% | GSAx | Home Goalie | Status | SV% | GSAx | Edge |
|------|------------|--------|-----|------|------------|--------|-----|------|------|
[⚠️ Flag backup goalie situations prominently]

## Injury Report
| Game | Player | Pos | Status | Impact (1-10) | Line Impact |
|------|--------|-----|--------|---------------|-------------|

## Line Movement Tracker
| Game | Open ML | Current | Cents Move | Open Total | Current | Move | Driver |
|------|---------|---------|-----------|-----------|---------|------|--------|
[Driver column: "Goalie" / "Sharp" / "Public" / "Unknown"]

## Sharp Action Alerts
| Game | Signal | Direction | Strength | Notes |
|------|--------|-----------|----------|-------|

## Tonight's Slate
| Time (ET) | Away | Home | ML | Total | Rest (A/H) | B2B? | Notes |
|-----------|------|------|----|-------|------------|------|-------|

## Schedule Edges
[B2B situations, rest mismatches, long road trips, timezone disadvantages]

## Open Bets Watch
[Any of the user's active NHL bets affected by today's news. If no open bets, say "No open NHL bets."]

## One Sharp Thought
[A single genuinely non-obvious NHL insight for tonight]

---
*This analysis is generated by an AI betting analysis system for informational and entertainment purposes only. It does not constitute gambling advice. Sports betting involves substantial risk of loss. Never bet more than you can afford to lose. If you or someone you know has a gambling problem, call 1-800-GAMBLER.*
```

## Quality Standards
- **Goalie confirmations are the headline.** In NHL, knowing the starter is worth more than almost any other piece of information.
- Brevity is everything. This is a 5-minute read.
- Focus on what MATTERS for tonight's NHL betting slate.
- If it's a quiet day, say so. "Light slate, no major edges" is valid.
- Timestamp all odds data.
- Don't make predictions — report facts and flag what to watch. Picks come from `/nhl-picks`.
- Always distinguish goalie-driven line moves from genuine sharp action.

Update `memory/data-freshness.json` with today's date for `nhl.last_briefing_date`.
