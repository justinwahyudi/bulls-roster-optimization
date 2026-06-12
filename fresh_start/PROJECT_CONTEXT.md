# Bulls Roster Optimization Project Context

This project asks:

> How can the Chicago Bulls optimize roster construction given performance gaps, financial constraints, and market availability?

The working basketball decision is whether the Bulls should retool or rebuild after the April 2026 front-office reset, then identify draft and free-agent targets that best support that path.

## Source Context From Project PDF

The PDF frames the Bulls as an underperforming team with a young, mobile roster and major offseason flexibility. It highlights:

- Recent front-office change after the firing of Arturas Karnisovas and Marc Eversley.
- A 2025-26 record noted in the document as 29-49 and 12th place as of April 7, 2026.
- Core young pieces: Matas Buzelis and Josh Giddey.
- Strengths: youth, mobility, and very high pace.
- Weaknesses: post defense, lack of true size, and no listed player over 7 feet.
- Major 2026 offseason cap room, while still wanting to remain below the salary cap or first apron depending on strategy.
- Need to evaluate free agents, draft prospects, unsigned players, and current roster decisions together.

## Current Roster Buckets

Guards:

- Josh Giddey
- Rob Dillingham
- Tre Jones
- Collin Sexton
- Anfernee Simons
- Yuki Kawamura
- Mac McClung

Forwards:

- Matas Buzelis
- Isaac Okoro
- Patrick Williams
- Leonard Miller
- Noa Essengue

Centers:

- Jalen Smith
- Nick Richards
- Guerschon Yabusele
- Zach Collins
- Lachlan Olbrich

## Performance Gap Framework

Use the current Bulls roster and league-wide player data to compare Chicago against the league in these categories:

- Shooting: `FG_PCT`, `FG3_PCT`, `FT_PCT`, `FGM`, `FGA`, `FG3M`, `FG3A`
- Scoring and playmaking: `PTS`, `AST`, `TOV`, `PLUS_MINUS`
- Rebounding: `REB`, `OREB`, `DREB`
- Defense: `STL`, `BLK`, `BLKA`, `PF`
- Winning context: `W_PCT`, `W`, `L`

The PDF specifically calls out defensive evaluation with emphasis on:

- `DREB`
- `REB`
- `BLK`
- `W_PCT`
- `PTS`

## Financial Constraint Framework

Model the 2026 offseason as a constrained optimization problem. The PDF lists these cap parameters:

- Salary cap: `$166M`
- Salary floor: `$149.4M`
- Luxury tax: `$201.7M`
- First apron: `$210.3M`
- Second apron: `$223.1M`
- Room exception: `$9.4M`
- Non-taxpayer mid-level exception: `$15.1M`
- Taxpayer mid-level exception: `$6.1M`
- Bi-annual exception: `$5.5M`

Important cap mechanics to model:

- Cap holds reduce practical cap space until renounced or replaced by signed contracts.
- Renouncing a free agent can open cap space but gives up Bird Rights.
- Bird Rights can allow the team to exceed the cap to re-sign its own free agents.
- Mid-level exceptions depend on whether the team is under the cap, over the cap, under the first apron, or under the second apron.

## Data Science Objective

Build a ranked decision system, not just a list of good players.

The output should answer:

1. What are the Bulls' biggest measurable roster gaps?
2. Which draft prospects best address those gaps?
3. Which free agents best address those gaps?
4. Which combinations of draft pick plus free-agent signings are feasible under cap/apron constraints?
5. How does the answer change under a retool strategy versus a rebuild strategy?

## Recommended Project Phases

### Phase 1: Baseline Team Diagnosis

Goal: quantify what Chicago lacks.

Tasks:

- Compare Bulls averages/ranks against league averages by position and team.
- Create z-scores or percentile ranks for major stat categories.
- Identify the 3-5 biggest weaknesses.
- Separate team-level needs from player-level outliers.

Deliverables:

- Team gap table.
- Visual ranking of Bulls weaknesses.
- Short written diagnosis.

### Phase 2: Player Fit Scores

Goal: score possible targets according to Bulls needs.

Create a composite score with separate sub-scores:

- Defense score: blocks, steals, defensive rebounds, fouls, size if available.
- Shooting score: three-point percentage, three-point volume, free throws, true shooting if added later.
- Creation score: assists, turnovers, points, usage proxy.
- Age/upside score: age, experience, development curve.
- Role fit score: position, minutes, roster redundancy.

Start simple with weighted normalized stats. Later, improve with regression, clustering, or similarity models.

### Phase 3: Financial Feasibility

Goal: prevent unrealistic recommendations.

Tasks:

- Add salary/cap-hold data.
- Create player acquisition cost estimates.
- Define cap scenarios:
  - Max cap-space scenario after renouncing selected holds.
  - Retool scenario with core players kept.
  - Rebuild scenario prioritizing age, picks, and flexibility.
- Filter targets by affordability.

Deliverables:

- Feasible free-agent target table.
- Scenario-specific cap sheet.
- Remaining cap room after each proposed move.

### Phase 4: Draft Model

Goal: rank draft prospects by need, value, and upside.

Tasks:

- Build a prospect dataset with college/international stats, measurements, age, position, and draft projection.
- Translate college stats into standardized scores.
- Add a draft availability probability, because the best prospect is irrelevant if unavailable at Chicago's pick.
- Score prospects under retool and rebuild weights.

Deliverables:

- Draft board.
- Bulls-specific prospect ranking.
- Sensitivity chart showing how rankings change by strategy.

### Phase 5: Optimization Model

Goal: choose combinations, not isolated players.

Decision variables:

- Whether to draft each prospect.
- Whether to sign each free agent.
- Whether to keep, waive, renounce, or re-sign selected current players.

Objective function:

- Maximize projected roster value and team fit.

Constraints:

- Salary cap or apron threshold.
- Roster spots.
- Positional balance.
- Minimum defense/rebounding improvement.
- Strategy preference: retool or rebuild.

Start with brute-force combinations for learning. Then move to integer linear programming if needed.

## Tutor Notes

The best learning path is:

1. Make the scoring model transparent before making it complex.
2. Normalize stats before weighting them.
3. Keep weights editable so basketball assumptions can be tested.
4. Treat the final ranking as a decision aid, not an oracle.
5. Always show why a player ranks highly.

The first notebook milestone should be:

> Identify the Bulls' top 3 performance gaps from the existing CSVs and build the first version of a player target score.

