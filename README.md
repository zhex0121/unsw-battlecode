# UNSW Battlecode 2026 — Technical Portfolio

## Overview

This repository presents selected technical work from my participation in UNSW Battlecode 2026, a competition involving autonomous agents.

My work involved strategy design, C++ agent development, experimental evaluation, and iteration on a collaborative game-playing system.

This is an individual portfolio, not a mirror of the team's private repository. It focuses on selected investigations, the evidence available for them, and the limitations of the conclusions.

## Selected Work

### 1. Safer Stride: End-State Safety in Multi-Step Movement

**Focus:** Reducing dangerous endpoints for long dragons and the queen during multi-step movement.

I investigated an end-state safety constraint intended to prevent units from completing strides at unsafe locations. In the reported local evaluation, risky endpoints during calm multi-step moves fell from 46.8% to 0.2%.

The change did not establish a reliable improvement in match outcomes: head-on deaths of long dragons did not decrease, and pearl collection was lower. The candidate was nevertheless merged into the team's active bot.

[Read the case study](case-studies/safer-stride.md)

### 2. Queen Fallback: Evaluating a Portal-Based Escape Strategy

**Focus:** Testing whether positioning the queen near a protected corner and portal could reduce early deaths.

The investigation measured how often the intended positioning and escape behaviour occurred, then compared queen survival and match outcomes. The mechanism was rarely activated, and the available results did not demonstrate an overall improvement.

[Read the case study](case-studies/queen-fallback.md)

### 3. Final Bot Integration: Reconstructing the `one21` Submission

**Focus:** Version tracking, server-profile reconstruction, and integration of the final live bot into the team's main development branch.

PR #62 documents a five-commit sequence tracking the `one15` through `one21` versions. Its description identifies `one21` as submission `21857`, the last upload before the competition deadline, and reports that the reconstruction script produced a `main.cpp` identical to the uploaded source.

The pull request remains open in the supplied record. This case study therefore distinguishes the already reported submission and reconstruction evidence from the proposed merge into `main`.

[Read the case study](case-studies/final-bot-integration.md)

## Experimental Approach

The case studies distinguish intended mechanism behaviour, measured gameplay outcomes, source reconstruction, and deployment status. Results are reported with their test conditions and limitations.

[Read the evaluation-methods note](methodology/evaluation-methods.md)

## Collaboration and Attribution

This portfolio documents my individual contributions within a collaborative UNSW Battlecode 2026 project. It distinguishes my own design, implementation, testing, and analysis from shared team contributions and AI-assisted development.

The portfolio includes links to the relevant source code, issues, pull requests, experimental artifacts, maps, and replays to support inspection and reproducibility.

## Evidence and Reproducibility

The repository provides selected source code, experimental records, maps, replays, and technical documentation from the project. These materials are organized to make the relationship between strategy changes, test configurations, observed results, and the final bot easier to inspect.

The final live bot discussed here is `one21`, submission `21857`. Its source reconstruction and configuration are documented alongside the relevant version history.

The portfolio is an individual technical record of work within a collaborative project, not a claim of sole authorship of the complete bot or every artifact.

## Limitations

These case studies document engineering investigations in a competitive game environment. Their results are specific to the tested configurations and should not be interpreted as proof of general algorithmic superiority.

Negative, mixed, or inconclusive findings are included where they are necessary to represent the evidence accurately.
