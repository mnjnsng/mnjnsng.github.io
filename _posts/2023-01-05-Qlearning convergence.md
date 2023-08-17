---
title: "Q-Learning: Convergence of the algorithm"
date: 2023-01-05
permalink: /posts/2023/01/Qlearning-convergence/
tags:
  - TD learning
  - Reinforcement Learning
  - Q learning
---

As we discussed in the previous [post](https://mnjnsng.github.io/posts/2022/12/Qlearning/), in this post, we will prove the convergence of the Q-learning algorithm using some useful norm tricks and contraction theorem.

#### Lemma (norm equivalence)

If $$\|TQ_1-TQ_2\|_\infty \leq \gamma \|Q_1-Q_2\|_\infty$$, then the following inequality holds:

$$\|TQ_1-TQ_2\|_{w,p} \leq \gamma_{w,p} \|Q_1-Q_2\|_{w,p},$$

where $$\|\cdot\|_{w,p} \triangleq (\sum_{i}w_i|\cdot|^p)^{\frac{1}{p}}$$.

Such property holds true when the vector space is finite-dimensional real or complex one. In our case, we are considering fininte cardinality of $\mathcal{S}\times \mathcal{A}$, and therefore such lemma holds.

#### Lemma (p-1 smoothness)

Let $W(Q_k) = \|Q_k-Q\|_p^2$, where $Q=TQ$. Then, the following inequality holds:
$$W(Q_{k+1}) \leq W(Q_k) + \nabla^\top W(Q_k)(Q_{k+1}-Q_k) + \frac{1}{2}(p-1)\|Q_{k+1}-Q_k\|_p^2.$$

Proof of the lemma can be done by direct calculation and using Holder's inequality. Details can be found in this [lecture note](https://www.google.com/url?q=https%3A%2F%2Fuofi.box.com%2Fs%2Fapz6455iv2y6r378nm58mc6cue4fplmt&sa=D&sntz=1&usg=AOvVaw3CYmdzejzEZj_kwji4kgQN).

Now, given the two lemmas, we are ready to prove the convergence of Q-learning algorithm.

## Convergence of Q-learning algorithm

The Taylor series expansion on the Lyapunov function $W(Q_{k+1})$ yields

$$
\begin{aligned}
W(Q_{k+1}) &= W(Q_{k}) + \nabla^\top W(Q_k)(Q_{k+1}-Q_k) + \frac{1}{2}(Q_{k+1}-Q_k)^\top \nabla^2 W(\tilde{Q}_k)(Q_{k+1}-Q_k)\\
&\leq W(Q_{k}) + \underbrace{\nabla^\top W(Q_k)(Q_{k+1}-Q_k)}_{(1)} +\underbrace{\frac{1}{2}(p-1) \|Q_{k+1}-Q_k\|_p^2}_{(2)}~~~(\text{p-1 smoothness})
\end{aligned}
$$

We will prove that (1) $\leq -C_1\epsilon_k W(Q_k)$ and $(2) \leq C_2 \epsilon_k^2 W(Q_k)$, and thus obtain $W(Q_{k+1}) \leq (1-C_1\epsilon_k+C_2\epsilon_k^2)W(Q_k),$ which implies that with the wise choice of $\epsilon$, the Lyapunov function will converge; proving the convergence of the Q-learning algorithm.

The first part can be proved as follows:

$$
\begin{aligned}
\nabla^\top W(Q_k)(Q_{k+1}-Q_k) &= -\epsilon_k \nabla^\top W(Q_k)D^\pi (Q_k-TQ_k) ~~(\because \text{ Q-learning algorithm})\\
&= -\epsilon_k \sum_{(s,a)}\|Q_k-Q\|_p~~ \frac{1}{p}(\sum_{(s,a)}|Q_k(s,a)-Q(s,a)|^p)^{\frac{1}{p}-1}\\
&~~~~\cdot p|Q_k(s,a)-Q(s,a)|^{p-1}sgn(Q_k(s,a)-Q(s,a))\\
&~~~~\cdot d^\pi(s,a)(Q_k-TQ_k)(s,a)\\
&= \frac{\epsilon_k}{\|Q_k-Q\|^{p-2}_p} \sum_{(s,a)}(Q_k(s,a)-Q(s,a))^{p-1}\underbrace{sgn(Q_k(s,a)-Q(s,a))}_{q}d^\pi(s,a)(Q_k(s,a)-TQ_k(s,a))\\
&\leq \frac{\epsilon_k}{\|Q_k-Q\|^{p-2}_p} \|Q_k-Q\|^{p-1}_{D^\pi,p} (\|Q_k-Q\|_{D^\pi,q}\underbrace{\|TQ_k-TQ\|_{D^\pi,q}}_{\leq \gamma\|Q_k-Q\|_{D^\pi,q}})\\
&\leq -\epsilon_k  C_1 \|Q_k-Q\|^2_{p} \end{aligned}$$
for some $C_1>0$.

Likewise,

$$
\begin{aligned}
\frac{p-1}{2}\|Q_{k+1}-Q_k\|^2_p &= \frac{(p-1)\epsilon_k^2}{2}\|D^\pi(Q_k-TQ_k)\|_p^2\\
&\leq \frac{(p-1)\epsilon_k^2}{2} 2\left(\|D^\pi(Q_k-Q)\|^2_p + \|D^\pi(Q-TQ_k)\|^2_p\right)~~~(\because (a+b)^2\leq 2(a^2+b^2)).
\end{aligned}
$$

Whereas, we have

$$
\begin{aligned}
\|D^\pi(Q_k-Q)\|_p^p &= \sum_{(s,a)}(d^\pi(s,a))^p|Q_k(s,a)-Q(s,a)|^p\\
&\leq d_{\max}^p \sum_{(s,a)}|Q_k(s,a)-Q(s,a)|^p.
\end{aligned}
$$

Consequently,

$$\|D^\pi(Q_k-Q)\|_p^2\|\leq d_{\max}^2 \|Q_k-Q\|^2$$, and

$$
\begin{aligned}
\|D^\pi(Q-TQ_k)\|_p^p &= \sum_{(s,a)}(d^\pi(s,a))^p| Q(s,a)-TQ_k(s,a)|^p\\
&\leq \sum_{(s,a)}d^\pi(s,a)|Q-TQ_k|^p\\
&= \|Q-TQ_k\|^p_{D^\pi,p}\\
&\leq \tilde{\gamma}^p\|Q-Q_k\|^p_{D^\pi,p}.
\end{aligned}
$$

Putting it together,

$$
\begin{aligned}
\frac{p-1}{2}\|Q_{k+1}-Q_k\|^2_p &\leq (p-1)\epsilon_k^2(d_{\max}^2 + d_{\max}\tilde{\gamma}^2)\|Q_k-Q\|_p^2\\
&\leq (p-1)\epsilon_k^2 d_{\max}(1+\tilde{\gamma}^2)\|Q_k-Q\|^2_p .
\end{aligned}
$$

Therefore, as we stated before, we have

$$W(Q_{k+1})\leq (1-C_1\epsilon_k +C_2\epsilon_k^2)W(Q_k),$$

indicating the convergence of the lyapunov function, and hence, the Q-function.
