# Evolutionary Game Simulation: Does Cooperation Survive?

## What is this?
A simulation of the Iterated Prisoner's Dilemma (or Hawk-Dove game) using replicator dynamics to model how cooperative and selfish strategies compete and evolve over generations — then compared against real cooperation rates from public goods game experiments.

## Why does this matter?
Classic game theory predicts selfishness should dominate, yet cooperation is everywhere in nature and society. This project explores when and why cooperative strategies can survive and even thrive.

## How it works
1. Define a set of strategies (e.g., Always Cooperate, Always Defect, Tit-for-Tat)
2. Simulate repeated games between all strategy pairs
3. Update the population each "generation" based on average scores (replicator dynamics)
4. Track the population mix over time until it stabilizes

## Results
<img width="790" height="490" alt="strategy population over time" src="https://github.com/user-attachments/assets/c022c0c1-af9f-4509-80e7-51cb1a8339c5" />

Starting from an even 3-way split, the population converges to roughly 86% Tit-for-Tat, 14% Always Cooperate, and 0% Always Defect. Tit-for-Tat's ability to cooperate with cooperators while punishing defectors lets it outcompete unconditional strategies, and Always Defect is driven to extinction once Tit-for-Tat dominates the population.

## Real-world comparison
<img width="690" height="490" alt="simulated vs real-world" src="https://github.com/user-attachments/assets/cdba85a8-a97b-4142-99d9-50401d0cddd0" />

Because Tit-for-Tat still defects against defectors, strategy share alone doesn't capture the real cooperation rate — so I measured the actual % of individual moves that were "Cooperate" once the population reached equilibrium: ~100%.

Public goods game experiments report much lower cooperation: a widely-cited meta-analysis (Zelmer, 2003) finds average contributions of 40–60% of the endowment in early rounds, decaying to roughly 10–20% by later rounds of repeated play.

My simulation massively overshoots real human cooperation. Full reasoning in writeup.md, but the short version: my model lets players track and reciprocate against one specific, repeatedly-faced opponent, while real public goods experiments involve anonymous groups of 4+ people contributing to a shared pot — conditions where Tit-for-Tat-style reciprocity can't function, so cooperation erodes toward free-riding instead.

## How to run it
Open the notebook in Google Colab and run all cells in order:

https://colab.research.google.com/github/yxngwei/evolutionary-game-simulation/blob/main/simulation.ipynb

## What I'd explore next
- Extending the simulation to groups of 4+ players (a true public-goods setup) instead of 2-player matches, to see if cooperation collapses toward the real-world benchmark once reciprocity against a single opponent is no longer possible
- Adding noise (occasional accidental defections) to see if Tit-for-Tat still dominates when mistakes happen
- Testing additional strategies (e.g., Grim Trigger, Generous Tit-for-Tat)
- Running multiple random seeds to check how sensitive the equilibrium is to the starting mix
