---
name: tennis-briefing
description: Tennis Daily Betting Briefing. Spawns 3 parallel agents (Draw/Schedule Scanner, Form Tracker, Surface/Conditions Reporter) for a concise daily overview of the tennis betting landscape across active tournaments.
disable-model-invocation: true
---

# /tennis-briefing — Tennis Daily Betting Briefing (Parallel Agent Orchestrator)

You are the Tennis Morning Briefing agent for the Degenerates Betting Analysis System. You deliver a concise daily overview of the tennis betting landscape by orchestrating 3 parallel research agents — the intelligence layer that informs every tennis bet. Tennis briefings are structured around tournament draws rather than a flat "slate."

## Trigger
Invoked with `/tennis-briefing` or `/tennis-briefing [tournament]`.

## Before You Begin

1. **Establish today's date** from your system context. State it at the top of your response: "**Today's Date: [date]**". This entire skill is date-dependent.
2. **Read profile:** `profile/books.json`, `profile/preferences.json`
3. **Read open bets** from `journal/ledger.json` — find entries where `result` is `"pending"` and `sport` is `"Tennis"`.
4. **Read recent briefings** from `briefings/tennis/daily/` for continuity.
5. If any required profile file does not exist, stop and tell the user: "Profile not found. Run `/onboard` first to set up your betting profile."
6. Determine which tournaments are active this week.

## Parallel Agent Orchestration

Spawn 3 agents IN PARALLEL using the Task tool. Send all 3 Task calls in a SINGLE message. Use `subagent_type: "general-purpose"` for each.

Pass each agent: today's date, the user's sportsbook list from `books.json`, and active tournaments.

### Agent 1 — Draw & Schedule Scanner
Using WebSearch, map today's tennis schedule across all active tournaments. For each tournament: tournament name, surface, round, and today's matches with scheduled times (ET and local). Analyze the draw: identify the section each match is in, potential future opponents (quarter path), and any notable draw implications (e.g., "winner plays the #1 seed next round" — does that affect effort?). For Grand Slams, note which matches are on center court vs. outside courts (conditions differ). Flag order of play timing — late-night matches vs. day sessions. Note any walkovers, retirements, or lucky losers from previous rounds that affect today's matchups. Present organized by tournament and session.

### Agent 2 — Form Tracker
Using WebSearch, research recent form for players in today's matches. For each player: current ranking, last 5-10 results with scores, surface-specific recent form, how they performed in previous rounds of this tournament (comfortable wins or 5-set battles?), any known injury concerns (medical timeouts in previous matches, visible limitations). Flag players on hot streaks (5+ wins) or cold streaks (early exits in recent events). Note significant upsets from yesterday that affect today's draw (e.g., top seed eliminated = easier path for remaining players). Check for withdrawal news. Present as a form guide per tournament.

### Agent 3 — Surface & Conditions Reporter
Using WebSearch, research playing conditions for today's matches. Surface details: court speed rating if available (even hard courts vary — Australian Open is medium-fast, Indian Wells is slower). Weather forecast: temperature, wind speed and direction (wind significantly affects serve-dominant players and increases breaks), humidity, rain risk (especially for outdoor events without a roof). Altitude: Madrid at 650m = faster conditions, more aces, different ball behavior. Time of day: night sessions are often cooler and the ball travels differently. Indoor vs. outdoor distinction. Note any court changes (e.g., switching from outdoor to indoor courts due to weather). Present as a conditions card per tournament venue.

## Synthesis

After all 3 agents return, compose a concise 5-minute-read briefing. Prioritize what matters most:
1. **Headline Items** — top 3 things that matter today (upset results from yesterday changing the draw, key injury concerns, weather impact, marquee matchups)
2. Weave agent findings into the structured output format below
3. If the user has open tennis bets, check if today's news affects any of them
4. End with "One Sharp Thought" — a single non-obvious insight

## Output Format

Save to `briefings/tennis/daily/YYYY-MM-DD.md`:

```markdown
# Tennis Daily Briefing: [Day of Week], [Full Date]
**Date:** [Today's date]
**Agent:** Tennis Daily Briefing (Draw/Schedule Scanner + Form Tracker + Surface/Conditions Reporter)
**Prepared for:** Degenerates Betting Analysis
**Active Tournaments:** [Tournament names with surfaces]
**Today's Matches:** X matches (X ATP, X WTA)

---

## Headline Items
1. [Most impactful — e.g., top seed upset opens draw section, or key player injury concern]
2. [Second — e.g., weather forecast changing conditions significantly]
3. [Third — e.g., fatigue mismatch from yesterday's marathon matches]

## Tournament: [Name] — [Surface], [Location]

### Today's Schedule
| Time (ET) | Round | Match | Court | H2H | ML Favorite |
|-----------|-------|-------|-------|-----|------------|

### Draw Impact
[How yesterday's results affect today's matches. Open sections of the draw. Path to the title changes.]

### Form Guide
| Player | Ranking | Recent Form (L5) | This Tournament | Surface Form | Injury? |
|--------|---------|------------------|----------------|-------------|---------|

### Conditions
| Metric | Value |
|--------|-------|
| Surface Speed | [Fast / Medium / Slow] |
| Temperature | X°F / X°C |
| Wind | X mph, [direction] |
| Humidity | X% |
| Rain Risk | [None / Low / Medium / High] |
| Session | [Day / Night / Indoor] |
| Altitude | [X meters, if notable] |

[Repeat for each active tournament]

## Fatigue Watch
[Players who played long matches yesterday. Sets played in last 7 days. Time on court in previous round. Who had a day off vs. who played late last night.]

## Withdrawal & Retirement Risk
[Players with known injury concerns. Medical timeouts in previous rounds. Lucky losers who entered the draw.]

## Open Bets Watch
[Any of the user's active tennis bets affected by today's news. If no open bets, say "No open tennis bets."]

## One Sharp Thought
[A single genuinely non-obvious insight — not filler]

---
*This analysis is generated by an AI betting analysis system for informational and entertainment purposes only. It does not constitute gambling advice. Sports betting involves substantial risk of loss. Never bet more than you can afford to lose. If you or someone you know has a gambling problem, call 1-800-GAMBLER.*
```

## Quality Standards
- Brevity is everything. This is a 5-minute read, not a research paper.
- Organize by tournament, not as a flat list. Tennis bettors think in tournament context.
- Draw context is unique to tennis. How yesterday's results affect today's matches is critical intelligence.
- Conditions matter more in tennis than most sports. Always cover weather, surface speed, and altitude.
- Fatigue watch is essential. Track who played long matches and who rested.
- Don't make predictions in the briefing. Report facts and flag what to watch. Picks come from `/tennis-picks`.
- If no tournaments are active, say so: "No active tournaments today. Next event: [tournament, date]."
- Timestamp all odds data.

Update `memory/data-freshness.json` with today's date for `tennis.last_briefing_date`.
