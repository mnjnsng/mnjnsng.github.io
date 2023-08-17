---
title: "Q-Learning: Understanding the idea"
date: 2022-12-22
permalink: /posts/2022/12/Qlearning/
tags:
  - TD learning
  - Reinforcement Learning
  - Q learning
---

As we have previously discussed in [post](https://mnjnsng.github.io/posts/2022/09/tdlearning/) that TD learning uses the mean estimate method to update the mean estimation of the $Q$ value. We proved in this [post](https://mnjnsng.github.io/posts/2022/10/tdlearning-noisefree/) the convergence of $Q$ values following the algorithm:

$$Q^\pi_k(s,a) = (1-\epsilon_k)Q^\pi_k(s,a)+\epsilon_k(r(s,a)+\gamma Q^\pi_k(s',a')).$$

Note in this algorithm that we are updating $Q$ values according to stationary policy $\pi$, which is not being updated. In other words, we are estimating $Q$ values estimated under the policy $\pi$. However, the main goal of reinforcement learning is to find the optimal policy, and as such, we will need to somehow integrate policy optimization with this TD learning algorithm. Such type of TD algorithm is called $Q$-learning algorithm. Roughly, the optimal policy will be the action argument that maximizes the $Q$ value, and $Q$ values will be updated correspondingly. Formally,

$$Q_{k+1}(s,a) = (1-\epsilon_k)Q_k(s,a)+\epsilon_k(\underbrace{r(s,a)+\gamma \max_{a'}Q(s',a')}_{TQ_k(s,a)})$$

when $(s_k,a_k) = (s,a)$  and  $s_{k+1} = s'$, and

$$Q_{k+1}(s,a) = Q_k(s,a) $$

otherwise.

Again, this is a *very* asynchronous algorithm, meaning that you'll need a lot of data of good quality to make this algorithm converge (we will prove the convergence of this algorithm in the next post).

As we have done previously, we rewrite this equation in a vector form:

$$Q_{k+1} = Q_k-\epsilon_k D^\pi (Q_k-TQ_k) - \epsilon_k n_k,$$

where $$D^\pi := diag(d^\pi)$, $d^\pi$$ being the stationary distribution for $P^\pi$. Since the actual samples are not sampled from such distribution, resulting discrepancy are lumped in the noise term $n_k$.

Now, the big question is whether this algorithm converges. This problem is more difficult to analyze, even in the noise free ($n_k=0$) case, due to the maximum operator being part of the algorithm. To be specific, as detailed in this [post](https://mnjnsng.github.io/posts/2022/10/tdlearning-noisefree/), we used a Lyapunov analysis using the contractive property of $T^\pi$ to prove the convergence of the algorithm. However, having the maximum operator in the algorithm, the contractive property is in $$\|\cdot\|_\infty$$ norm, or formally,

$$\|TQ_1-TQ_2\|_ {\infty} \leq \gamma \|Q_1-Q_2\|_ {\infty}.$$

This raises a problem in the Lyapunov analysis because this norm is not differentiable. In the next post, we will discuss how to get around this problem simply by first proving that if an inequality holds for infinity-norm, its corresponding weighted $p$-norm also holds.
