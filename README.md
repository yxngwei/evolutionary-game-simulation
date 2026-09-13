# Evolutionary Game Simulation: Does Cooperation Survive?

## What is this?
A simulation of evolutionary game theory — starting with the Iterated Prisoner's Dilemma under replicator dynamics, then extended into a real N-player public goods game, noise/mistake robustness testing, and starting-condition sensitivity analysis — compared throughout against real cooperation-rate data from behavioral economics experiments.

## Why does this matter?
Classic game theory predicts selfishness should dominate, yet cooperation is everywhere in nature and society. This project explores when and why cooperative strategies can survive, and — just as importantly — when and why that logic breaks down.

## How it works
1. Define a set of strategies (Always Cooperate, Always Defect, Tit-for-Tat, and later Grim Trigger and Generous Tit-for-Tat)
2. Simulate repeated matches between every pair of strategies
3. Update the population each "generation" based on average scores (replicator dynamics)
4. Track the population mix until it stabilizes, then measure the actual move-by-move cooperation rate
5. Compare against real experimental cooperation-rate benchmarks

## Results
<img width="790" height="490" alt="strategy population over time" src="https://github.com/user-attachments/assets/c022c0c1-af9f-4509-80e7-51cb1a8339c5" />

Starting from an even 3-way split, the population converges to roughly 86% Tit-for-Tat, 14% Always Cooperate, and 0% Always Defect. Tit-for-Tat's ability to cooperate with cooperators while punishing defectors lets it outcompete unconditional strategies, and Always Defect is driven to extinction once Tit-for-Tat dominates the population.

<img width="690" height="490" alt="simulated vs real-world" src="https://github.com/user-attachments/assets/cdba85a8-a97b-4142-99d9-50401d0cddd0" />

Public goods experiments report much lower real-world cooperation (Zelmer, 2003: 40–60% early rounds, decaying to 10–20% later). Full reasoning for the gap is in [writeup.md](writeup.md).

## Deeper dive: four follow-up experiments
 
**1. Noise robustness** — added Grim Trigger and Generous Tit-for-Tat, then tested mistake rates from 0% to 20%.

<img width="790" height="490" alt="strategy robustness to noise" src="https://github.com/user-attachments/assets/32f7191a-9c36-4708-9b51-538f279401b3" />

At low noise, reciprocity-based strategies mostly hold up. At 20% noise, **Always Defect takes over almost entirely** — retaliation spirals become too costly once mistakes are frequent.
 
**2. Real N-player public goods game** — rebuilt the actual multi-person game (not just 2-player matches): private contributions to a shared pot, multiplied 1.6x and split evenly among a group of 4.

<img width="790" height="490" alt="N-player public goods game" src="https://github.com/user-attachments/assets/37710249-d4cf-4c98-a2d0-e87d2281d0a4" />

Result: **cooperation collapses to 0%** (Free Rider takes over completely) — matching the textbook Nash prediction, and confirming that group size/anonymity alone (not just psychology) can flip the outcome from ~100% cooperation to 0%.
 
**3. Sensitivity to starting conditions** — reran the 2-player model from five different starting population splits.

<img width="989" height="590" alt="equillibrium sensitivity to starting population mix" src="https://github.com/user-attachments/assets/9e07c20d-19a8-412a-863a-8b367b37cd37" />

Most surprising result: starting with **80% cooperators and only 10% defectors** leads to **Always Defect winning 100%** — with so many naive cooperators to exploit, defectors get a strong enough head start that Tit-for-Tat never catches up. This is a genuine multiple-equilibria result: the outcome depends on where you start, not just the payoffs.

**4. More real-world benchmarks:**
 
| Source | Cooperation rate |
|---|---|
| My 2-player IPD simulation | ~100% |
| My N-player public goods simulation | 0% |
| Zelmer (2003), early rounds | 40–60% |
| Zelmer (2003), late rounds | 10–20% |
| Rand & Nowak (2011), no punishment | ~34% |
| Rand & Nowak (2011), with punishment | ~87% |
| Herrmann et al. (2008), 16 countries | wide cross-cultural variation |
 
Real human cooperation consistently lands *between* my two pure models (100% dyadic reciprocity vs. 0% anonymous self-interest) — suggesting real cooperation runs on a mix of mechanisms (partial reputation effects, genuine other-regarding preferences, culturally-specific social norms) that neither pure model captures alone. Full interpretation in [writeup.md](writeup.md).

## How to run it
Open the notebook in Google Colab and run all cells in order:

https://colab.research.google.com/github/yxngwei/evolutionary-game-simulation/blob/main/simulation.ipynb

## What I'd explore next
- Add a costly-punishment option to the N-player model to see if it closes the gap with Rand & Nowak's 34–87% range
- Model a public "reputation" system in the N-player game to test whether partial (not full 1-on-1) reciprocity can sustain cooperation
- Map the exact tipping-point ratio of Tit-for-Tat to Always Defect needed for cooperation to survive
- Compare individual countries' data (Herrmann et al., 2008) against my two pure models to see which real societies sit closer to which extreme
