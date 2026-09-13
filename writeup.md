# Writeup: Simulated vs. Real-World Cooperation

## What I built
An agent-based simulation of evolutionary game theory, starting from a simple 2-player Iterated Prisoner's Dilemma with replicator dynamics, then extended in four directions to test how robust the original result is and how it compares to real human behavior.

## Part 1: The baseline result (2-player Iterated Prisoner's Dilemma)
Three strategies — Always Cooperate, Always Defect, Tit-for-Tat — competing under replicator dynamics from an even 3-way starting split converge to roughly **86% Tit-for-Tat, 14% Always Cooperate, 0% Always Defect**, with an actual move-by-move cooperation rate of **~100%**.

This is the classic Axelrod-style result: Tit-for-Tat's ability to cooperate with cooperators while punishing defectors makes it extremely hard to invade, and once it dominates, mutual cooperation is nearly universal.

## Part 2: Four deeper-dive experiments

### Experiment 1 — Does this survive contact with mistakes? (noise robustness)
Real players sometimes mis-click, misremember, or act on a bad signal. I extended the model with a "noise" parameter (probability an intended move accidentally flips) and added two more literature-standard strategies: Grim Trigger (defects forever after the first betrayal) and Generous Tit-for-Tat (like Tit-for-Tat, but forgives defection ~10% of the time).

| Noise level | Outcome |
|---|---|
| 0% | Mixed reciprocity-based population (no single strategy fully dominates with 5 competing) |
| 5–10% | Tit-for-Tat still leads, Grim Trigger collapses (it can't recover from a single accidental defection) |
| 20% | **Always Defect takes over almost entirely (~99.7%)** |

**Takeaway:** reciprocity-based cooperation is not unconditionally robust — it depends on mistakes being rare enough that retaliation spirals don't spin out of control. Grim Trigger in particular is fragile: one accidental defection locks in permanent mutual punishment, which is costly enough that Grim Trigger gets driven out once noise appears.

### Experiment 2 — Does it hold in a real N-player public goods game?
The original model is a 2-player game where you can identify and reciprocate against one specific partner. Real public goods experiments use groups of 4+ anonymous contributors. I rebuilt the actual game: each player privately contributes part of an endowment to a shared pot, which is multiplied by 1.6x and split evenly among the group regardless of who contributed what.

Result: starting from an even mix of Free Rider, Full Cooperator, and Conditional Cooperator (a strategy that mimics the group's average past contribution), the population converges to **100% Free Rider — 0% cooperation**.

**Takeaway:** this matches the textbook Nash equilibrium prediction for this exact game. Once reciprocity can no longer be targeted at one specific person, Conditional Cooperator has no way to "punish" the individual free-riders hiding inside the group average, and free-riding completely dominates. This directly confirms the hypothesis from the original writeup: group size and anonymity, not the underlying psychology, are enough to flip the outcome from ~100% cooperation to 0%.

### Experiment 3 — How sensitive is the outcome to where you start?
I reran the 2-player model from five different starting population splits instead of always starting even.

| Starting mix | Final equilibrium |
|---|---|
| Even (33/33/33) | 86% Tit-for-Tat |
| Defect-heavy (10% Coop / 80% Defect / 10% TFT) | 100% Tit-for-Tat |
| **Cooperate-heavy (80% Coop / 10% Defect / 10% TFT)** | **100% Always Defect** |
| TFT-heavy (10/10/80) | 90% Tit-for-Tat |
| No Tit-for-Tat at all (50% Coop / 50% Defect) | 100% Always Defect |

**Takeaway — the most surprising result of the deeper dive:** starting with MOSTLY cooperators (80%) and few defectors actually leads to total defection winning, not cooperation. With so many naive cooperators to exploit and few Tit-for-Tat players around to police them early on, Always Defect gets a strong enough head start that Tit-for-Tat never catches up. This is a genuine **multiple-equilibria / tipping-point** result: this system doesn't have one inevitable outcome — the ending point depends on the initial mix, not just the payoff structure. It also shows Tit-for-Tat's success in the original run wasn't guaranteed by the payoffs alone; it needed a large-enough starting foothold to survive.

### Experiment 4 — Comparing against more real-world data
Two additional real benchmarks, beyond the original Zelmer (2003) figure:

- **Rand & Nowak (2011, *Nature Communications*)** modeled an N-player public goods game evolutionarily (much like Experiment 2 here) and found average cooperation of **~34% with no punishment mechanism available, rising to ~87% once players could pay to punish free-riders**. This lines up strikingly with my own two extremes: my N-player model with no punishment option converged to 0% (even lower than their 34%, likely because my Conditional Cooperator still can't target individuals at all, whereas theirs has some punishment mechanics), and my 2-player model (which has, in effect, "punishment" built in via Tit-for-Tat's ability to retaliate) converged to ~100%, even higher than their punishment-enabled 87%.
- **Herrmann, Thöni & Gächter (2008, *Science*)** ran the same public goods game in 16 cities across 16 countries and found **large, significant cross-societal variation** in both cooperation and punishment behavior — cooperation is not a fixed human universal but depends on local social norms and institutions. This is directly relevant to a Synthesis-style "cultural and consumer insights" lens: a single global "human cooperation rate" doesn't really exist: it's culturally contingent.

## Full comparison table

| Source | Cooperation rate |
|---|---|
| My 2-player IPD simulation (equilibrium) | ~100% |
| My N-player public goods simulation (equilibrium) | 0% |
| Real public goods experiments, early rounds (Zelmer, 2003) | 40–60% |
| Real public goods experiments, late rounds (Zelmer, 2003) | 10–20% |
| Rand & Nowak (2011) model, no punishment | ~34% |
| Rand & Nowak (2011) model, with punishment | ~87% |
| Herrmann et al. (2008), 16 countries | wide variation, no single figure |

## Overall interpretation
Two "pure" models — one built purely on dyadic reciprocity, one built purely on anonymous group self-interest — bracket real human behavior from opposite extremes (100% vs. 0%), while real humans consistently land in between (10–60% depending on the study and round). This strongly suggests real cooperation is driven by a *mix* of mechanisms my simple models don't individually capture: partial reciprocity even in group settings (people do form reputations over repeated play), genuine other-regarding preferences (altruism, warm-glow giving, inequity aversion), and social norms that vary by culture — exactly the kind of nuance a pure game-theoretic replicator model can't produce on its own, but which behavioral/consumer-insight research is built to investigate.

## What I'd explore next
- Add a costly-punishment option to the N-player model (as in Rand & Nowak, 2011) to see if it can close the gap between my 0% result and their 34–87% range.
- Model a "reputation" system in the N-player game (e.g., a public history of each individual's past contributions) to test whether *identifiable* reciprocity — short of full anonymity, short of full 1-on-1 pairing — is enough to sustain partial cooperation.
- Map the sensitivity result from Experiment 3 more finely: find the approximate "tipping point" starting ratio of Tit-for-Tat to Always Defect needed for cooperation to survive.
- Look for country-level data (building on Herrmann et al., 2008) to see if any single country's cooperation rate is a closer match to either of my two pure models than the global average is.

## References
- Zelmer, J. (2003). Linear Public Goods Experiments: A Meta-Analysis. *Experimental Economics*, 6(3), 299–310.
- Rand, D. G. & Nowak, M. A. (2011). The evolution of antisocial punishment in optional public goods games. *Nature Communications*, 2, 434.
- Herrmann, B., Thöni, C. & Gächter, S. (2008). Antisocial Punishment Across Societies. *Science*, 319(5868), 1362–1367.
