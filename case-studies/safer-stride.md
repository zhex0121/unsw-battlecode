# Safer Stride: Evaluating End-State Safety in Multi-Step Agent Movement

## Overview

I contributed to the investigation of a safer-stride policy for an autonomous agent in UNSW Battlecode. The goal was to reduce avoidable deaths among long dragons and the queen without sacrificing most of the economic benefit of multi-step movement.

The work is documented in the team's Issue #29 and its associated implementation and evaluation workflow.

## Problem

The existing stride implementation evaluated movement steps individually but did not adequately constrain the final position of an entire multi-step move.

Earlier testing had indicated that enabling stride increased multi-step movement from 52% to 70% online, while local testing showed a 10–15% increase in deaths among dragons of length five or greater.

The hypothesis was that checking the final tile's safety and escape options would reduce dangerous movement outcomes while retaining much of stride's economic benefit.

## Method

The candidate added a `STRIDE_SAFE_END` switch, disabled by default during the initial evaluation. The base and candidate versions were identified by their respective commit hashes.

The local evaluation used four large development maps, two seeds, and both sides. Sixteen switch-on games were compared with the corresponding base-bot results. Selected replays were also inspected to examine individual movement decisions and death sequences.

The recorded measurements included:

- Risky-end frequency during calm multi-step moves by long dragons and the queen
- Multi-step movement rates by unit length
- Queen survival and death timing
- Long-dragon deaths, including head-on collisions
- Pearl collection and total agent length at round 300
- Longest dragon at the end of a game

A separate online check against external opponents was also recorded. 
## Results

### Mechanism

The proportion of risky ends among calm multi-step moves by the queen and length-eight-or-greater dragons decreased from 46.8% with the switch off to 0.2% with it on.

The multi-step rate for length-five-to-seven dragons remained similar, at 81.1% versus 80.5%. For length-eight-or-greater dragons, it decreased from 78.3% to 64.1%; for the queen, it decreased from 84.5% to 49.2%.

These results show that the safety constraint changed the intended movement behaviour.

### Combat outcomes

In the recorded counts for length-eight-or-greater dragons, head-on deaths caused by enemy movement increased from 645 to 676. Deaths occurring immediately after the agent's own multi-step move decreased from 427 to 292, while those occurring immediately after its own single-step move increased from 198 to 363.

The queen died in all 16 switch-on games. Its median death round increased from 105 to 133.5, but the sample did not establish improved queen survival.

The candidate won 6 of 16 local games by the longest-dragon criterion, compared with 10 of 16 for the baseline. The online check was 8 wins to 6, within the recorded noise assessment. Neither result established a reliable overall win-rate improvement.

### Economic outcomes

Average pearl collection decreased from 5,703.8 to 5,085.4 per game, while average total length at round 300 increased from 475.4 to 495.5. Average longest-dragon length at the end decreased from 115.4 to 95.8.

The economic trade-off was therefore mixed: lower pearl collection and shorter final longest dragons, alongside slightly higher total length at round 300.

## Interpretation

The experiment supported the narrow mechanism claim: the implementation substantially reduced risky stride endpoints under the tested conditions.

It did not establish the broader hypothesis that safer stride reduced head-on deaths or improved overall match outcomes. Some deaths associated with multi-step movement were reduced, but head-on deaths after single-step moves increased. Queen deaths also occurred through behaviours outside the scope of the stride-specific change.

The results suggest that end-state safety is only one part of the combat-risk problem. A separate evaluation of single-step decisions and enemy reach may be needed to address the remaining failure modes.

## Limitations

The local evaluation used a small number of development maps and seeds, and the two versions were not evaluated on a broad held-out map set or against external opponents in the supplied report. CPU impact was not measured. The reported mechanism counts also do not, by themselves, establish a causal explanation for the observed death patterns.

The switch was ultimately shipped as part of the team's active bot, but deployment should not be interpreted as proof of a statistically reliable performance improvement.

## Evidence

- Issue #29: Safer stride
- PR #35: Candidate implementation
- Base commit: `9a4f3fc440d768b237ee9bc9b78665a03f53c39a`
- Candidate commit: `82407e034cc4adb516c6a96a719110af8f02e20a`
