# Evolutionary Game Simulation: Does Cooperation Survive?
 
## What is this?
A simulation of evolutionary game theory — starting with the Iterated Prisoner's Dilemma under replicator dynamics, then extended across eight experiments (noise robustness, a true N-player public goods game, costly punishment, a reputation system, starting-condition sensitivity, an exact tipping-point map, and real per-city comparisons) — tested throughout against real cooperation-rate data from three independent published sources.
 
## Why does this matter?
Classic game theory predicts selfishness should dominate, yet cooperation is everywhere in nature and society. This project explores when and why cooperative strategies can survive, when that logic breaks down, and which real-world mechanisms (punishment, reputation, group size) actually move the needle.
 
## How it works
1. Define a set of strategies (Always Cooperate, Always Defect, Tit-for-Tat, later Grim Trigger and Generous Tit-for-Tat)
2. Simulate repeated matches between every pair of strategies
3. Update the population each "generation" via replicator dynamics
4. Track the population mix until it stabilizes, then measure the actual move-by-move cooperation rate
5. Compare against real experimental cooperation-rate benchmarks

## Baseline result
<img width="790" height="490" alt="strategy population over time" src="https://github.com/user-attachments/assets/c022c0c1-af9f-4509-80e7-51cb1a8339c5" />

Starting from an even 3-way split, the population converges to roughly **86% Tit-for-Tat, 14% Always Cooperate, 0% Always Defect**, with an actual cooperation rate of **~100%** of moves — far above real human cooperation rates.

## Eight deeper-dive experiments
 
**1. Noise robustness** — added Grim Trigger and Generous Tit-for-Tat, tested mistake rates from 0–20%.

<img width="790" height="490" alt="strategy robustness to noise" src="https://github.com/user-attachments/assets/366bad8d-728c-4821-bf55-ee7adb67caff" />

At 20% noise, **Always Defect takes over almost entirely** — retaliation spirals become too costly once mistakes are frequent.

 
**2. Real N-player public goods game** — private contributions to a shared pot, multiplied 1.6x, split among a group of 4.

<img width="790" height="490" alt="strategy population over time" src="https://github.com/user-attachments/assets/06c806e8-94bb-4670-90ef-bfbfaa358ece" />

**Cooperation collapses to 0%** — group size and anonymity alone can flip the outcome from ~100% to 0%.


**3. Sensitivity to starting conditions** — reran the 2-player model from five different starting splits.

<img width="989" height="590" alt="equillibrium sensitivity to starting population mix" src="https://github.com/user-attachments/assets/c154af79-2c84-4228-a48f-04ecfd07c975" />

Most surprising result: starting with **80% cooperators and only 10% defectors still leads to Always Defect winning 100%** — a genuine multiple-equilibria result where the outcome depends on the starting mix, not just the payoffs.

 
**4. Costly punishment** — added a Punisher strategy to the N-player game to see if it could rescue cooperation.

<img width="790" height="490" alt="pg_punishment_convergence" src="https://github.com/user-attachments/assets/6acbc244-1e77-4d53-891f-17bdacd5ad1a" />

**Still collapses to 0% cooperation.** Cooperators (who pay no punishment cost) outcompete Punishers first; once Punishers are gone, Free Riders finish the job — reproducing the well-known **second-order free-rider problem**.

 
**5. Reputation system** — an agent-based model where each player's contribution history is a public, visible reputation.

<img width="1290" height="490" alt="reputation_model" src="https://github.com/user-attachments/assets/ec898201-8efa-466c-aa9e-447d9cc850fa" />

Cooperation stays low (~1%), but "Discriminator" agents survive at ~22% of the population instead of going fully extinct — reputation gives cooperation a fighting chance, not a winning one.

 
**6. Exact tipping point** — with Always Cooperate fixed at 10%, swept the Tit-for-Tat vs. Always Defect starting ratio.

<img width="790" height="490" alt="tipping_point" src="https://github.com/user-attachments/assets/c4139a58-1cfa-49fc-be55-8e9d7aa0975b" />

Found a sharp threshold: **6.3–6.8% starting Tit-for-Tat share** is the knife-edge between total defection and total cooperation winning.

 
**7. Real per-city comparison** — 16 real cities' cooperation rates (Herrmann, Thöni & Gächter, 2008) against my two pure models.

<img width="889" height="790" alt="country_comparison" src="https://github.com/user-attachments/assets/db0a717d-16f4-4f8b-8f32-523beb97b9d9" />

**Copenhagen (88.5%)** sits closest to my 2-player model's 100%;
**Athens (28.5%)** sits closest to my N-player model's 0%. The 16-city average, 56.1%, lands almost exactly halfway between my two extremes.
 

