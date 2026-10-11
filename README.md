# UNSW Battlecode 2026 — Technical Portfolio

## Overview

This repository presents selected technical work from my participation in UNSW Battlecode 2026, a competition involving autonomous agents.

My work involved strategy design, C++ agent development, experimental evaluation, and iteration on a collaborative game-playing system.

This is an individual portfolio, not a mirror of the team's private repository. It presents selected strategy investigations, supporting evidence, and the final competition bot.

## Selected Investigations

### 1. Safer Stride: End-State Safety in Multi-Step Movement

**Focus:** Reducing dangerous endpoints for long dragons and the queen during multi-step movement.

I investigated an end-state safety constraint intended to prevent units from completing strides at unsafe locations. In the reported local evaluation, risky endpoints during calm multi-step moves fell from 46.8% to 0.2%.

The change did not establish a reliable improvement in match outcomes: head-on deaths of long dragons did not decrease, and pearl collection was lower. The candidate was nevertheless merged into the team's active bot.

[Read the case study](case-studies/safer-stride.md)

### 2. Queen Fallback: Evaluating a Portal-Based Escape Strategy

**Focus:** Testing whether positioning the queen near a protected corner and portal could reduce early deaths.

The investigation measured how often the intended positioning and escape behaviour occurred, then compared queen survival and match outcomes. The mechanism was rarely activated, and the available results did not demonstrate an overall improvement.

[Read the case study](case-studies/queen-fallback.md)

## Final Competition Bot

### `one21` — Final Live Submission

The final competition bot represents the culmination of strategy development, implementation, testing, and iteration throughout UNSW Battlecode 2026.

The submitted version, `one21` (submission `21857`), brought together the team's bot logic and server-specific configuration for the final competition submission. The available submission record identifies it as the last upload before the competition deadline.

The source includes the C++ agent implementation and supporting strategy logic developed across the project. The final bot is presented here as an engineering artifact in its own right, alongside the focused investigations above.

- [Final bot source](evidence/final-bot/main.cpp)
- [Server profile](evidence/final-bot/server_profile.txt)
- [Source-generation script](evidence/final-bot/server_bot.py)

The source and configuration files are provided to make the final implementation inspectable. The linked files should correspond to the recorded `one21` submission; the source-generation script is supporting evidence of how that submitted source was reproduced.

## Experimental Approach

The case studies distinguish intended mechanism behaviour, measured gameplay outcomes, source reconstruction, and deployment status. Results are reported with their test conditions and limitations.

[Read the evaluation-methods note](methodology/evaluation-methods.md)

## Evidence and Reproducibility

This portfolio includes selected source code, experimental records, and technical documentation from the project. These materials are organized to make strategy implementation, evaluation, and the final competition artifact easier to inspect.

The portfolio is an individual technical record of work within a collaborative project, not a claim of sole authorship of the complete bot or every artifact.

## Limitations

The reported experimental results are specific to their tested configurations and should not be interpreted as proof of general algorithmic superiority. The final bot reflects collaborative development under competition constraints; the included materials identify selected implementation and evaluation evidence rather than claiming that every component was independently developed or exhaustively validated.
