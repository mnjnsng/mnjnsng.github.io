---
title: "TD learning with Linear Function Approximation: Noise free analysis"
date: 2022-11-24
permalink: /posts/2022/11/TD-LFA-noisefree/
tags:
  - TD learning
  - Reinforcement Learning
  - LFA
---

From the last [post](https://mnjnsng.github.io/posts/2022/11/TD-LFA/) we obtained the algorithm for TD learning with LFA:

$$
\begin{aligned}
\theta_{k+1} = \theta_k-\epsilon_k\left(\Phi^\top D^\pi \Phi \theta_k - \Phi^\top D^\pi r-\gamma \Phi^\top D^\pi P^\pi\Phi \theta_k\right) - \epsilon_k n_k.
\end{aligned}
$$

In this post we continue to analyze the algorithm under noise-free case and prove the convergence of it.

## Noise-free analysis

Consider a noise-free case when $n_k \equiv 0$:

$$
\begin{aligned}
\theta_{k+1} = \theta_k-\epsilon_k\left(\Phi^\top D^\pi \Phi \theta_k - \Phi^\top D^\pi r-\gamma \Phi^\top D^\pi P^\pi\Phi \theta_k\right).
\end{aligned}
$$

First, observe that _if_ the algorithm would converge such that $$\theta_k \rightarrow \theta^\star$$ as $$k\rightarrow \infty$$, then the above equation simply reduces to

$$
\begin{aligned}
\Phi^\top D^\pi \underbrace{\Phi \theta^\star}_{Q^\star} =  \Phi^\top D^\pi\left(\underbrace{r+\gamma \Phi^\top D^\pi P^\pi\Phi \theta^\star}_{T^\pi Q^\star} \right) .
\end{aligned}
$$

This can be interpreted in the _projection_ view point such that

$$Q^\star = \text{Proj}_\Phi(T^\pi Q^\star).$$

To illustrate this point, recall that the _projection_ of any $$\tilde{Q}$$ onto the space spanned by $\Phi$ is $$\Phi \tilde{\theta}$$ where $$\tilde{\theta}$$ solves

$$\min_{\tilde{\theta}}\frac{1}{2}\|\Phi \tilde{\theta} - \tilde{Q}\|^2_{D^\pi}.$$

The norm can be any norm in general in defining a projection, but here I used the weighted 2-norm or $$D^\pi$$ norm so that it fits our case. In other words, the solution to the above minimization is given implicitly by

$$\Phi^\top D^\pi \Phi \tilde{\theta} - \Phi^\top D^\pi \tilde{Q} = 0.$$

Replacing the corresponding variables, it is clear that

$$Q^\star = \text{Proj}_\Phi(T^\pi Q^\star).$$

Now, we claim that $$\text{Proj}_\Phi(T^\pi Q)$$ is a contraction maping in the sense of $$\|\cdot\|_{D^\pi}$$.

(_Proof_)
First observe that $$\text{Proj}_\Phi$$ is a _non-expansive_ operator such that

$$\|\text{Proj}_\Phi(Q_1)-\text{Proj}_\Phi(Q_2)\|_{D^\pi} \leq \|Q_1-Q_2\|.$$

The proof of the nonexpansive property of a projection operator can be found [here](https://angms.science/doc/CVX/CVX_ProjectionIsNonexpansive.pdf).

Then,

$$
\begin{aligned}
\|\text{Proj}_\Phi T^\pi Q_1-\text{Proj}_\Phi T^\pi Q_2\|^2_{D^\pi} &\leq \|T^\pi Q_1 - T^\pi Q_2\|^2_{D^\pi} \quad (\because \text{non-expansive})\\
&\leq \gamma\|Q_1-Q_2\|^2_{D^\pi} \quad (\because T^\pi \text{ is a contraction mapping}).
\end{aligned}
$$

Therefore $$\text{Proj}_\Phi(T^\pi Q)$$ is a contraction maping in the sense of $$\|\cdot\|_{D^\pi}$$.

Coming back to the noise-free analysis, define $$W(\theta) := \frac{1}{2}\|\theta-\theta^\star\|^2$$ where $$Q^\star(s,a) = \theta^{\star \top}\phi(s,a).$$

Then,

$$
\begin{aligned}
W(\theta_{k+1})- W(\theta_k) &= (\theta_k-\theta^\star)^\top (\theta_{k+1}-\theta_k) + \frac{1}{2}\|\theta_{k+1}-\theta_k\|^2\\
&= -\epsilon_k(\theta_k-\theta^\star)^\top \Phi^\top D^\pi (\Phi\theta_k - T^\pi \Phi\theta_k) + \epsilon_k^2\|\Phi^\top D^\pi(\Phi \theta_k - T^\pi \Phi \theta_k)\|^2\\
&= -\epsilon_k(\theta_k-\theta^\star)^\top \Phi^\top D^\pi (\Phi\theta_k-\Phi \theta^\star +\Phi \theta^\star - T^\pi(\Phi \theta_k))\\
&\quad \quad +\epsilon_k^2\|\Phi^\top D^\pi(\Phi \theta_k - \Phi \theta^\star + T^\pi (\Phi \theta^\star) - T^\pi \Phi \theta_k)\|^2\\
&\quad \quad \quad (\because \Phi^\top D^\pi Q^\star =  \Phi^\top D^\pi\left( T^\pi Q^\star\right).
\end{aligned}
$$

Further, using that both $$T^\pi$$ and $$\text{Proj}_\Phi T^\pi$$ are a contraction mapping and rewriting it in terms of $D^\pi$ norm we obtain

$$
\begin{aligned}
&\leq -\epsilon_k\|\Phi(\theta_k-\theta^\star)\|^2_{ D^\pi} + \epsilon_k\gamma\|\Phi(\theta^\star -\theta_k)\|_{D^\pi}^2 \\
&\quad \quad + \epsilon_k^2\|\Phi^\top D^\pi(\Phi \theta_k - \Phi \theta^\star)\|^2 + \epsilon_k^2 \|\Phi^\top D^\pi (T^\pi (\Phi \theta^\star) - T^\pi \Phi \theta_k)\|^2\\
&\leq -\epsilon_k\|\Phi(\theta_k-\theta^\star)\|^2_{ D^\pi} + \epsilon_k\gamma\|\Phi(\theta^\star -\theta_k)\|_{D^\pi}^2 +\epsilon_k^2 (1+\gamma^2) \|\Phi\theta_k - \Phi \theta^\star\|^2_{D^\pi}\\
&\leq -\epsilon_k(1-\gamma)d_{\min}\|\theta_k-\theta^\star\|^2_{D^\pi} + \epsilon_k^2 (1+\gamma^2) d_{\min} \|\theta_k-\theta^\star\|^2_{D^\pi}\\
&\leq -\epsilon_k(1-\gamma)d_{\min} \lambda_{\min}\|\theta_k-\theta^\star\|^2 + \epsilon_k^2 (1+\gamma^2) d_{\min} \lambda_{\max} \|\theta_k-\theta^\star\|^2.
\end{aligned}
$$

Therefore, we have that

$$W(\theta_{k+1}) \leq \left(1-\epsilon_k(1-\gamma)d_{\min} \lambda_{\min} + \epsilon_k^2 d_{\min}\lambda_{\max}(1+\gamma^2)\right)W(\theta_k),$$

which converges for small enough $$\epsilon_k\equiv \epsilon \quad \forall k.$$

It is important to recall from the previous [post](https://mnjnsng.github.io/posts/2022/11/TD-LFA/) that we made an assumption: $$ Q(s,a) = \theta^\top \phi(s,a).$$ However, in general we do not have a precise linear representation of Q-function. So, although we proved under the assumption that $\theta$ converges to the solution of

$$\Phi \theta^\star = \text{Proj}_{\Phi}T^\pi(\Phi \theta^\star),$$

this is only half of the story. In other words, we cannot account for the model error caused by the selection of $\phi$. To be specific, use the triangular inequality to obtain

$$\|Q^\pi - Q_k\| \leq \underbrace{\|Q^\pi - \Phi\theta^\star\|}_{\text{model error}} + \underbrace{\| \Phi\theta^\star - Q_k\|}_{\text{Projection error}}.$$

Here, although the projection error can be proved to be bounded, the model error cannot be compensated with the TD learning because it is independent of $Q_k$. This error can only be made smaller by choosing better features $\Phi$.
