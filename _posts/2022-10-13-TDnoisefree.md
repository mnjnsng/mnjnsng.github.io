---
title: 'TD learning: deeper analysis (1)'
date: 2022-10-13
permalink: /posts/2022/10/tdlearning-noisefree/
tags:
  - TD learning
  - Noise-free analysis
---
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
In the previous [post](https://mnjnsng.github.io/posts/2022/09/tdlearning/), we have discussed the basic idea about the TD learning. In this post, we will go deeper into its analysis and prove the convergence of the TD learning under *noise-free* case, which we will describe shortly.

## Revisiting TD learning

Let's begin by rewriting the TD learning algorithm.

$$Q_k(s,a) = (1-\epsilon_k)Q_k(s,a) + \epsilon_k(r(s,a) + \gamma Q_k(s',a')).$$

I have dropped superscript $\pi$ from the notation from the previous [post](https://mnjnsng.github.io/posts/2022/09/tdlearning/) for brevity.

We can write this compactly in a vector form as

$$Q_{k+1} = Q_k - \epsilon_k(D_k Q_k - D_k r_k- \gamma E_k Q_k)$$

where $D_k := \text{diag}(I_{(s_k,a_k) = (s,a)})$ and $E_k := \text{diag}(I_{(s_k,a_k) = (s,a)\: \&\:(s_{k+1},a_{k+1}) = (s',a')})$ are indicator functions that vectorizes the asynchronous update law.

Now assume that the transition dynamics $P^\pi$ that follows the given policy $\pi$, is irreducible such that there exists a stationary distribution $d^\pi$. Further suppose that $(s_k,a_k) \sim d^\pi$ such that each state and action pair for an asynchronous update is sampled from the stationary distribution. Then,

$$\begin{aligned}
\mathbb{E}_{(s_k,a_k) \sim d^\pi}[D_k] &= \text{diag}(d^\pi) =: D^\pi\\
\mathbb{E}_{(s_k,a_k) \sim d^\pi}[E_k] &= D^\pi P^\pi
\end{aligned}$$

The first equality is obviously true from the definition of stationary distribution, and the second is a one-step propagation of state and action pair following the transition dynamics $P^\pi$. Rewriting the TD-learning algorithm with respect to $D^\pi$, we obtain:

$$\begin{aligned}
Q_{k+1} &= Q_k - \epsilon_k(D_k Q_k - D_k r_k- \gamma E_k Q_k)\\
&=Q_k - \epsilon_k(D^\pi Q_k - D^\pi \bar{r} - \gamma D^\pi P^\pi Q_k) + \epsilon_k (D^\pi Q_k-D^\pi\bar{r}-\gamma D^\pi P^\pi Q_k)\\
&\quad \quad \:-\epsilon_k(D_kQ_k-D_kr_k-\gamma E_kQ_k)\\
&=: Q_k -\epsilon_k D^\pi(Q_k-\bar{r}-\gamma P^\pi Q_k) + \epsilon_k n_k.
\end{aligned}$$

What we are trying to do here is to separate the expected value and *noise* $n_k$ from the noisy estimate that we initially had.

Now we will prove that the $Q$ function will converge provided that $n_k\equiv 0$. One might consider this to be a meaningless analysis because it is assuming that all the samples are directly from the stationary distribution such that our *noisy estimate* is not actually noisy. However, this analysis will be the basic building block for later analysis that relaxes the noise-free assumption, and therefore it is worth going through.

### Noise-free analysis

In the noise free case, we have

$$Q_{k+1} =  Q_k -\epsilon_k D^\pi(Q_k-\bar{r}-\gamma P^\pi Q_k) = Q_k - \epsilon_k D^\pi(Q_k- T^\pi Q_k)$$

where the second equality is from the definition of the Bellman operator $T^\pi$.

Now define $W(Q_k) := \frac{1}{2} \|Q_k-Q^\pi\|^2_2$. We will show that $W(Q_k)\rightarrow 0$ as $k\rightarrow \infty $ such that $Q_k \rightarrow Q^\pi $. This indicates convergence of the $Q$ function under TD learning with the absence of noise. Defining $W(Q_k)$ and proving its convergence to $0$ is resembles a technique using a *Lyapunov function* to prove the stability of an ODE.

*Proof*.

$$\begin{aligned}
W(Q_{k+1}) - W(Q_k) &= \nabla^\top W(Q_k)(Q_{k+1}-Q_k) + \frac{1}{2}\|Q_{k+1}-Q_k\|^2\\
&= (Q_k-Q^\pi)^\top (-\epsilon_kD^\pi(Q_k-T^\pi Q_k))+ \frac{1}{2}\|\epsilon_k D^\pi(Q_k-T^\pi Q_k)\|^2\\
&= -\epsilon_k (Q_k-Q^\pi)^\top D^\pi (Q_k- T^\pi Q^\pi + T^\pi Q^\pi -T^\pi Q_k)\\
& \quad \quad \: +\frac{1}{2} \epsilon_k^2\|Q_k- T^\pi Q^\pi + T^\pi Q^\pi -T^\pi Q_k\|^2_{(D^\pi)^2}
\end{aligned}$$

where the last norm is a weighted 2-norm with weight matrix being $(D^\pi)^2$. Elsewhere is simply a 2-norm. Note that $T^\pi Q^\pi \equiv Q^\pi$ by the definition of the Bellman equation. We also have that $ \| x+y \| ^2 \leq 2( \| x \| ^2+ \| y \| ^2)$, and $ \| \cdot \|_{(D^\pi)^2} \leq \| \cdot \|_{D^\pi}$ since each element in $D^\pi$ is less than or equal to 1. Using these ideas, we can further reduce the above equality as follows.

$$\begin{aligned}
&= -\epsilon_k (Q_k-Q^\pi)^\top D^\pi (Q_k- T^\pi Q^\pi + T^\pi Q^\pi -T^\pi Q_k)\\
& \quad \quad \: +\frac{1}{2} \epsilon_k^2\|Q_k- T^\pi Q^\pi + T^\pi Q^\pi -T^\pi Q_k\|^2_{(D^\pi)^2}\\
&\leq -\epsilon_k(Q_k-Q^\pi)^\top D^\pi (Q_k-Q^\pi) + \epsilon_k(Q_k-Q^\pi)^\top D^\pi(T^\pi Q_k-T^\pi Q^\pi)\\
& \quad \quad \: +\epsilon_k^2\left(\|Q_k-Q^\pi\|^2_{D^\pi} + \|T^\pi Q_k-T^\pi Q^\pi\|^2_{D^\pi}\right)\\
&\leq -\epsilon_k \|Q_k-Q^\pi\|^2_{D^\pi} + \epsilon_k \|Q_k-Q^\pi\|_{D^\pi} \|T^\pi Q^\pi -T^\pi Q_k\|_{D^\pi} \\
&\quad \quad \: + \epsilon_k^2\left(\|Q_k-Q^\pi\|^2_{D^\pi} + \|T^\pi Q_k-T^\pi Q^\pi\|^2_{D^\pi}\right)
\end{aligned}$$

It turns out that $T^\pi$ is a contraction mapping with respect to $\|\cdot\|_{D^\pi}$ (we will prove it shortly). Consequently the inequality becomes

$$\begin{aligned}
&\leq -\epsilon_k \|Q_k-Q^\pi\|^2_{D^\pi} + \epsilon_k \gamma \|Q_k-Q^\pi\|^2_{D^\pi} + \epsilon_k^2\left(\|Q_k-Q^\pi\|^2_{D^\pi} + \gamma^2\|Q_k-Q^\pi\|^2_{D^\pi}\right)\\
&= -\epsilon_k \left((1-\gamma) - \epsilon_k(1+\gamma^2)\right)\|Q_k-Q^\pi\|^2_{D^\pi}\\
&\leq -\epsilon_k \left( (1-\gamma)-\epsilon_k (1+\gamma^2) \right)\|Q_k-Q^\pi\|^2_2\cdot d_{\min}\\
&= -2\epsilon_k d_{\min} \left( (1-\gamma)-\epsilon_k (1+\gamma^2) \right) W(Q_k)
\end{aligned}$$

where we simply used the fact that there must exist a constant $d_{\min}$ that relates the two finite and non-negative values. As a result,

$$ W(Q_{k+1})\leq \left(1-2\epsilon_k d_{\min} \left( (1-\gamma)-\epsilon_k (1+\gamma^2) \right)\right)W(Q_k) $$

and for sufficiently small $\epsilon_k$

$$W(Q_{k+1})\leq \beta W(Q_k) \leq \cdots \leq \beta^kW(Q_0)$$

where $\beta \in [0,1)$. As such, $W(Q_k)\rightarrow 0$ as $k\rightarrow \infty$, indicating the convergence of the $Q$-function.

As we have mentioned above we still have to prove that $T^\pi$ is a $\|\cdot\|_{D^\pi}$ contraction to complete the proof. Recall that $T^\pi Q := \bar{r}+\gamma P^\pi Q$. Then,

$$\begin{aligned}
\|T^\pi Q_1 - T^\pi Q_2\|^2_{D^\pi} &= \|\gamma P^\pi (Q_1-Q_2)\|^2_{D^\pi}\\
&= \gamma^2\ \sum_{s,a} d^\pi(s,a)\left(\sum_{s',a'}P(s',a'\vert s,a) \left(Q_1(s',a')-Q_2(s',a')\right)\right)^2\\
&= \gamma^2\ \sum_{s,a} d^\pi(s,a)\left(\mathbb{E}_{(s',a')\sim P^\pi}[Q_1(s',a')-Q_2(s',a')]\right)^2\\
&\leq \gamma^2 \sum_{s,a} d^\pi(s,a) \mathbb{E}_{(s',a')\sim P^\pi} [\left(Q_1(s',a')-Q_2(s',a')\right)^2] (\because \text{Jensen's inequality})\\
&= \gamma^2 \sum_{s,a} d^\pi(s,a) \sum_{s',a'} P(s',a'\vert s,a) \left(Q_1(s',a')-Q_2(s',a')\right)^2\\
&= \gamma^2 \sum_{s',a'}\left(Q_1(s',a')-Q_2(s',a')\right)^2 \sum_{s,a} d^\pi(s,a)P(s',a'\vert s,a)\\
&= \gamma^2 \sum_{s',a'}\left(Q_1(s',a')-Q_2(s',a')\right)^2 d^\pi(s',a')\\
&= \gamma^2\|Q_1-Q_2\|^2_{D^\pi}.
\end{aligned}$$

Hence, the convergence of $Q$ under noise-free TD learning algorithm is complete.

-----
