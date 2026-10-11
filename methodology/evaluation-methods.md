# Evaluation Methods

## Purpose

This note explains how to interpret the measurements reported in the portfolio's Battlecode case studies. It does not replace the original test logs or establish results beyond those recorded in the individual investigations.

## Baselines and Candidate Versions

A strategy change should be evaluated against an identified baseline. Where available, the case studies report the relevant version or commit, the candidate configuration, and whether the change was enabled or disabled.

A successful build or a merged pull request establishes that a change was implemented or integrated; it does not establish that the strategy improved performance.

## Match Conditions

Battlecode outcomes can depend on the map, seed, side, and opponent configuration. Matched comparisons help reduce some sources of variation, but they do not automatically eliminate dependence between games or prove causation.

The number of games, maps, seeds, and sides is reported separately where the evidence allows it. Different test sets are not combined unless they measure the same outcome under compatible conditions.

## Mechanism and Outcome Metrics

The investigations distinguish between:

- **Mechanism metrics:** whether the intended behaviour actually changed.
- **Survival and combat metrics:** how often units died and, where recorded, how they died.
- **Economic metrics:** such as pearl collection and total agent length at a specified round.
- **Match outcomes:** wins under the reported evaluation criterion.
- **Computational constraints:** such as CPU usage, where measured.

A mechanism metric can confirm that a rule is active without demonstrating that the overall strategy is beneficial.

## Replays and Failure Analysis

Replay inspection can reveal individual decisions and plausible failure mechanisms. A small number of selected replays is useful for debugging, but it should not be treated as a representative estimate of overall performance.

Claims based on replay inspection are identified as observations unless they are supported by systematic measurements.

## Uncertainty and Limitations

Small samples can produce substantial variation in match outcomes. Results may also be correlated across map transformations, seeds, sides, or repeated configurations.

Reported differences are therefore interpreted in the context of sample size, test coverage, and the evaluation procedure. Where no formal statistical test was conducted, the portfolio does not claim statistical significance.

## Reproducibility and Access

Commit references, test configurations, and links to available evidence are provided in each case study where publication is permitted. Some original implementation details and artifacts remain in a private collaborative repository and may not be independently accessible to readers.
