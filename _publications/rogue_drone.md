---
title: "Protective Mission against a Highly Maneuverable Rogue Drone Using Defense Margin Strategy"
collection: publications
permalink: /publication/rogue_drone
excerpt: "This paper is about the number 1. The number 2 is left for future work."
date: 2022-09-01
venue: "American Control Conference 2023"
paperurl: "https://arxiv.org/pdf/2203.15872"
citation: "Sung, Minjun, et al. "Protective Mission against a Highly Maneuverable Rogue Drone Using Defense Margin Strategy." arXiv preprint arXiv:2203.15872 (2022)."
---

You can find the full paper [here](https://arxiv.org/abs/2203.15872).

## Abstract

The current paper studies a protective mission to defend a domain called the safe zone from a rogue drone invasion. We consider a one attacker and one defender drone scenario where only a noisy observation of the attacker at every time step is accessible to the defender. Directly applying strategies used in existing problems such as pursuit-evasion games are shown to be insufficient for this mission. We introduce a new concept of defense margin to complement an existing strategy and construct a control strategy that successfully solves our problem. We provide analysis to point out the limitations of the existing strategy and how our defense margin strategy can enhance the performance. Simulation results show that our strategy outperforms that of the existing strategy at least by 36.0 percentage points in terms of protective mission success.

## Results

In the presence of observational noise, naively following the widely adopted guidance law called the _Pure Pursuit (PP)_ strategy is only effective only for some limited conditions (Theorem III.2). This is primarily because the PP strategy only aims to minimize the distance to the attacker, making it a highly aggressive strategy without considering its primary mission to protect to _safe zone_. Consequently, we define a more conservative strategy via the proposed concept called the _Defense Margin (DM)_. By adaptively parameterizing between the PP and DM strategy _(ADM)_, we obtain a highly effective stratety that effectively solves the protective mission.

<p float = "left", align = "center">
<img src = /images/publication/rogue_drone/ppvsintelligent.gif width="200" height="200"/>
<img src = /images/publication/rogue_drone/ourscvsintelligent.gif width="200" height="200"/>
<img src = /images/publication/rogue_drone/oursavsintelligent.gif width="200" height="200"/>
</p>

Demonstration of PP strategy **(left)** , DM strategy **(center)**, and ADM strategy **right**. Blue indicates a defender, red an attacker, and orange a noisy observation of an attacker. The green circle is the safe zone which the defender aims to keep intact from the attacker.

<p align = "center">
<img src = /images/publication/rogue_drone/winpercentage.png width = 550/>
</p>

Winning percentage of each strategy against different attacker strategies. Each percentage result is obtained by counting successful missions out of 1,000 randomly initialized scenarios. Clearly, the PP strategy was easy to be defeated. Our strategy to complement the PP strategy with the DM was successful in a protective mission.
