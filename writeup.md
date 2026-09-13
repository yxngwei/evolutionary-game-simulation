# Writeup: Simulated vs. Real-World Cooperation

## What I built
An agent-based simulation of the Iterated Prisoner's Dilemma with three strategies — Always Cooperate, Always Defect, and Tit-for-Tat — evolving  under replicator dynamics over 100 generations. Strategies that score above the population average grow their share each generation; strategies below average shrink.

## Simulation result
Starting from an even 3-way split (33% each), the population converges to:

| Strategy | Final share |
|---|---|
| Tit-for-Tat | ~86% |
| Always Cooperate | ~14% |
| Always Defect | ~0% |

This matches a well-known result from evolutionary game theory (first popularized by Robert Axelrod's tournaments): Tit-for-Tat is a highly robust strategy because it cooperates with cooperators (avoiding unnecessary conflict) while punishing defectors (avoiding being exploited). Always Defect looks attractive against naive cooperators but gets punished the moment it meets a Tit-for-Tat player, and eventually goes extinct once Tit-for-Tat dominates the population.

Measuring the actual cooperation rate — the % of individual moves that were "Cooperate," not just which strategy is most common — the equilibrium population cooperates on **~100%** of moves. This makes sense: once Always Defect is extinct, every remaining strategy in the population (Tit-for-Tat vs. Tit-for-Tat, or Tit-for-Tat vs. Always Cooperate) cooperates on essentially every round.

## Real-world benchmark
A meta-analysis of public goods game experiments (Zelmer, 2003, covering decades of studies) reports average contributions of **40–60% of the endowment in early rounds**, typically **decaying to 10–20%** by later rounds as free-riding sets in — a pattern replicated across dozens of follow-up studies.

## The gap, and why it's not a flaw in the model
My simulation predicts ~100% cooperation. Real humans cooperate at 15–50%. That's a large gap, and it's worth taking seriously rather than explaining away — but it has a clear structural cause:

1. **Group size and anonymity.** My simulation is strictly 2-player: each match is between exactly two identifiable participants who can remember each other's past moves. Public goods experiments typically involve groups of 4 or more people contributing anonymously to a shared pot. You cannot direct reciprocity at "the person who defected last round" when contributions are pooled and anonymous — which means Tit-for-Tat-style strategies, which rely entirely on tracking and punishing a specific partner, cannot function the same way in a group setting.

2. **Multiplier structure differs.** In the Prisoner's Dilemma payoff table used here, mutual cooperation is the second-best individual outcome and defection against a cooperator is strictly the best. Public goods games typically use a "multiply the pot and divide equally" structure, which produces a different, often steeper, individual incentive to free-ride as group size grows (each individual's contribution is diluted across more people).

3. **Real humans aren't playing repeated matches against a fixed partner for 10 rounds and then evolving as a population.** Many public goods experiments use one-shot or "stranger-matching" designs where participants are re-paired with new anonymous partners each round specifically to rule out reciprocity — which removes the exact mechanism (Tit-for-Tat-style tracking) that drives cooperation to ~100% in my model.

In short: my model shows that reciprocity-based strategies are a very powerful force for sustaining cooperation *when reciprocity against a specific partner is possible*. Real-world public goods behavior shows that once you remove that condition — by scaling up group size and anonymity — cooperation drops sharply, even though people are still far more cooperative than pure self-interest would predict (0% defection rather than the ~50% overshoot from Always Defect being profitable).

## What I'd explore next
- Extend the simulation to a true N-player public goods setting (pot is multiplied and split evenly) instead of 2-player Prisoner's Dilemma matches, to see whether the equilibrium cooperation rate drops toward the 15–50% real-world range once reciprocity against a single partner is no longer possible.
- Add noise (a small chance of accidental defection even from cooperative strategies) to test whether Tit-for-Tat's dominance is robust to mistakes, or whether it collapses into cycles of retaliation the way it can in noisy environments.
- Introduce additional strategies observed in real behavioral studies, such as Generous Tit-for-Tat (occasionally forgives a defection) or Grim Trigger (permanently defects after the first betrayal), to see how they reshape the equilibrium.
- Run the simulation across multiple random seeds and starting population splits to check how sensitive the final equilibrium is to initial conditions.

## Reference
Zelmer, J. (2003). Linear Public Goods Experiments: A Meta-Analysis.
*Experimental Economics*, 6(3), 299–310.
