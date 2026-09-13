# Writeup: Simulated vs. Real-World Cooperation

## What I built
An agent-based simulation of evolutionary game theory, starting from a simple 2-player Iterated Prisoner's Dilemma with replicator dynamics, then extended across eight experiments to stress-test the result and compare it against real human behavior from multiple independent sources.

## Part 1: The baseline result (2-player Iterated Prisoner's Dilemma)
Three strategies — Always Cooperate, Always Defect, Tit-for-Tat — competing under replicator dynamics from an even 3-way starting split converge to roughly **86% Tit-for-Tat, 14% Always Cooperate, 0% Always Defect**, with an actual move-by-move cooperation rate of **~100%**.

This is the classic Axelrod-style result: Tit-for-Tat's ability to cooperate with cooperators while punishing defectors makes it extremely hard to invade, and once it dominates, mutual cooperation is nearly universal.

## Part 2: Eight deeper-dive experiments

### 1. Noise robustness
Added Grim Trigger (unforgiving) and Generous Tit-for-Tat (forgives ~10% of defections) and tested mistake rates from 0% to 20%.

| Noise level | Outcome |
|---|---|
| 0–10% | Reciprocity-based strategies mostly hold up; Grim Trigger is the most fragile |
| 20% | **Always Defect takes over almost entirely (~99.7%)** |

**Takeaway:** reciprocity-based cooperation depends on mistakes being rare enough that retaliation spirals don't spin out of control.

### 2. Real N-player public goods game
Rebuilt the actual multi-person game (private contributions to a shared pot, multiplied 1.6x, split evenly among a group of 4) instead of 2-player matches.

**Result: cooperation collapses to 0%** (Free Rider takes over completely) — matching the textbook Nash prediction. Confirms that group size and anonymity alone, not just psychology, can flip the outcome from ~100% cooperation to 0%.

### 3. Sensitivity to starting conditions
Reran the 2-player model from five different starting population splits.

| Starting mix | Final equilibrium |
|---|---|
| Even (33/33/33) | 86% Tit-for-Tat |
| Defect-heavy (10/80/10) | 100% Tit-for-Tat |
| **Cooperate-heavy (80/10/10)** | **100% Always Defect** |
| TFT-heavy (10/10/80) | 90% Tit-for-Tat |
| No Tit-for-Tat at all (50/50/0) | 100% Always Defect |

**Takeaway — most surprising result of the whole project:** starting with MOSTLY cooperators (80%) and few defectors leads to total defection winning, not cooperation. With so many naive cooperators to exploit and few Tit-for-Tat players to police them early on, Always Defect gets a strong enough head start that Tit-for-Tat never catches up. The final outcome is a genuine multiple-equilibria result — it depends on where you start, not just the payoffs.

### 4. Costly punishment in the N-player game
Added a Punisher strategy (contributes fully AND pays a cost to fine below-average contributors) to see if it could rescue cooperation in the N-player game, following Rand & Nowak (2011).

**Result: still 100% Free Rider, 0% cooperation.** Tracing the generation-by-generation path explains why: Punishers pay an extra cost that plain Cooperators don't, so Cooperators quietly outcompete Punishers first (generation 5: Free Rider 79%, Cooperator 15%, Punisher 6%). Once Punishers are gone, nothing protects the remaining Cooperators, and Free Riders finish the job. This reproduces a well-known theoretical result called the **second-order free-rider problem**: punishment is itself a public good (everyone benefits from a well-behaved group, but only punishers pay the cost of enforcing it), so punishment alone, without extra structure, doesn't survive either.

### 5. Reputation system
Built an individual agent-based model (60 agents, randomly re-grouped every round) where each agent's history of past contributions is tracked as a public, visible reputation score. A "Discriminator" strategy contributes only when its current groupmates' average reputation is high enough.

**Result: cooperation stays low (~1%), but Discriminators survive at ~22% of the population** instead of going fully extinct like plain Cooperators do. Reputation gives cooperation a fighting chance, not a winning one, in this design — and counterintuitively, giving agents *more* rounds per generation to build reliable reputations made things worse, not better, since once Cooperators vanish, Discriminators can't out-compete Free Riders from a neutral starting reputation each generation.

### 6. Mapping the exact tipping point
With Always Cooperate fixed at a 10% starting share, swept the starting ratio of Tit-for-Tat to Always Defect to find the precise threshold where the population flips from total defection to total cooperation.

**Result: the tipping point sits between 6.3% and 6.8% starting Tit-for-Tat share.** Below that, Always Defect wins 100%; above it, Tit-for-Tat wins 90–100%. There is essentially no middle ground — it's a knife-edge threshold, not a gradual transition.

### 7. Comparing real per-city data against both pure models
Using real period-1 (pre-punishment) contribution data from 16 cities worldwide (Herrmann, Thöni & Gächter, 2008), compared each city's cooperation rate against my two pure-model extremes.

| City | Contribution rate |
|---|---|
| Copenhagen (highest) | 88.5% |
| Nottingham | 75.0% |
| Seoul | 73.5% |
| Bonn | 72.5% |
| Melbourne | 70.5% |
| Chengdu | 69.5% |
| Dnipropetrovsk | 54.5% |
| Minsk | 52.5% |
| St. Gallen | 50.5% |
| Muscat | 50.0% |
| Samara | 48.5% |
| Zurich | 46.5% |
| Boston | 46.5% |
| Istanbul | 35.5% |
| Riyadh | 34.5% |
| Athens (lowest) | 28.5% |

**16-city average: 56.1%** — almost exactly the midpoint between my 2-player model (100%) and N-player model (0%). Copenhagen sits closest to the dyadic-reciprocity extreme; Athens sits closest to the pure free-riding extreme. No single city matches either pure model exactly — real societies land somewhere on the spectrum between them, and where they land varies by a factor of 3x city-to-city.

### 8. Additional published benchmarks
- **Zelmer (2003)** meta-analysis: 40–60% contribution in early rounds of repeated public goods games, decaying to 10–20% in later rounds.
- **Rand & Nowak (2011)**: an evolutionary public-goods model found ~34% cooperation with no punishment option, rising to ~87% with punishment — a bigger effect from punishment than my own model produced, likely because their model includes additional strategy types and mutation dynamics that make the second-order free-rider problem less severe.

## Full comparison table

| Source | Cooperation rate |
|---|---|
| My 2-player IPD simulation (equilibrium) | ~100% |
| My N-player public goods simulation (no punishment) | 0% |
| My N-player public goods simulation (with punishment) | 0% (still collapses) |
| My reputation-based N-player model | ~1% (but Discriminators persist) |
| Copenhagen (highest city, Herrmann et al., 2008) | 88.5% |
| Athens (lowest city, Herrmann et al., 2008) | 28.5% |
| 16-city average (Herrmann et al., 2008) | 56.1% |
| Zelmer (2003), early rounds | 40–60% |
| Zelmer (2003), late rounds | 10–20% |
| Rand & Nowak (2011), no punishment | ~34% |
| Rand & Nowak (2011), with punishment | ~87% |

## Overall interpretation
Two "pure" models bracket real human behavior from opposite extremes: dyadic reciprocity pushes cooperation to ~100%, while anonymous self-interest pushes it to 0% — and neither costly punishment nor a basic reputation system was enough, on their own, to move the N-player result off of 0% in my implementation. Real humans across 16 cities average 56%, almost exactly between the two extremes, but with genuine cross-cultural variation spanning a 3x range (Athens to Copenhagen).

This suggests real-world cooperation isn't explained by any single mechanism in isolation — not pure reciprocity, not punishment alone, not reputation alone — but likely by some combination of these mechanisms working together, plus genuine other-regarding preferences (altruism, inequity aversion, warm-glow giving) and culturally-specific social norms that vary meaningfully by society. That combination is exactly the kind of nuance a simple game-theoretic replicator model can't produce from any one mechanism alone, but which cross-cultural behavioral/consumer-insight research is built to investigate.

## What I'd explore next
- Combine punishment AND reputation in the same N-player model — the second-order free-rider problem might be easier to overcome when Punishers can also identify each other via reputation and preferentially group together (assortment).
- Let reputation persist across generations rather than resetting for each new agent, to see if longer-run reputations change the outcome.
- Test whether the sharp 6.3–6.8% tipping point shifts if the fixed Always-Cooperate share is changed, to map out the full tipping-point surface rather than a single slice of it.
- Pull in country-level cultural or institutional variables (e.g. rule of law indices) to see if they predict which cities land closer to which of my two pure-model extremes.

## References
- Zelmer, J. (2003). Linear Public Goods Experiments: A Meta-Analysis. *Experimental Economics*, 6(3), 299–310.
- Rand, D. G. & Nowak, M. A. (2011). The evolution of antisocial punishment in optional public goods games. *Nature Communications*, 2, 434.
- Herrmann, B., Thöni, C. & Gächter, S. (2008). Antisocial Punishment Across Societies. *Science*, 319(5868), 1362–1367.
