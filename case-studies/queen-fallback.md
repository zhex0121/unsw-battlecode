# Evaluating a Portal-Based Queen Fallback Strategy

## Overview

I proposed and investigated a fallback policy for the queen in UNSW Battlecode. The policy aimed to reduce early queen deaths by positioning the queen near a kelp-protected corner and a nearby portal, allowing an emergency escape when threats appeared.

The investigation is documented in Issue #11 of the team's development repository.

## Problem and Hypothesis

The team's queen-survival investigation recorded 159 early queen deaths across 228 ladder games, with a median death round of 123.

The proposed policy was intended for situations in which no enclosed safe zone had been found. Rather than continuing its existing behaviour, the queen would wait and circle near a corner protected by kelp, ideally close to a portal that could provide an escape route.

The expected improvements were fewer queen deaths before round 150 and fewer head-on collisions in open space.

Potential failure modes included becoming trapped in the corner and escaping through a portal into a dangerous destination.

## My Contribution

I proposed the strategy hypothesis and participated in its tracking and evaluation through the team's development workflow.

## Evaluation Method

The candidate configuration, QG_CORNER (Z11), was compared with the baseline B61.

The recorded evaluation included:

- A 96-game matched evaluation on 24 maps, using seeds 100–101 and both sides.
- A separate outcome comparison covering 320 matched pairs across 160 map variants, including transformed variants and multiple map-generator families.
- Mechanism-level measurements, including anchor availability, portal availability, corner-proximity rounds, escape commands, queen survival, and queen-death timing.

The two test sets served different purposes and are reported separately.

## Results

In the 320-pair outcome comparison, the baseline won 25 games that the candidate lost, while the candidate won 37 games that the baseline lost. The resulting net difference was −12 wins for the candidate.

The supplied noise estimate was approximately 11.3 net wins per standard deviation. The observed difference was therefore about −1.06 standard deviations, insufficient to establish a reliable overall performance difference from this comparison alone.

In the separate 96-game mechanism evaluation:

- Queen survival at the end decreased from 11/96 to 8/96.
- Queen deaths before round 150 decreased from 65/96 to 62/96.
- The candidate issued only four escape commands across four games, despite 4,130 logged corner-near rounds.
- The aggregate number of queen head-collision deaths remained 32/96 in both configurations.

Results varied by map-generator family. The candidate performed better on blobs maps by net wins, while maze and rooms maps showed the largest negative differences.

## Interpretation and Limitations

The experiment did not establish that the proposed policy improved overall match outcomes.

Although early queen deaths decreased slightly in the mechanism evaluation, final queen survival was lower. The intended portal-escape mechanism was rarely activated, limiting what could be concluded about its effectiveness.

The available collision statistics did not separately identify open-space rams, so that part of the original hypothesis remained unresolved. The observed map-family differences are also exploratory and do not establish a causal relationship between map topology and strategy performance.

The result illustrates the importance of evaluating both overall outcomes and the mechanisms through which a strategy is expected to work.

## Evidence

- Original investigation: Issue #11, “Queen fallback: circle near a portal in a kelp corner”
- Implementation reference: commit `e7c380f`, which added the QG_SAFEZONE and QG_CORNER toggles
- Additional evidence: [link to permitted test reports and implementation changes]

This was a collaborative project. Individual implementation and analysis contributions should be attributed separately where applicable.
