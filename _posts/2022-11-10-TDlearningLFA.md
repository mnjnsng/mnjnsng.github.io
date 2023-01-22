---
title: "Understanding TD learning with Linear Function Approximation"
date: 2022-11-10
permalink: /posts/2022/11/TD-LFA/
tags:
  - TD learning
  - Reinforcement Learning
  - LFA
---

In the two previous posts( [post1](https://mnjnsng.github.io/posts/2022/10/tdlearning-noisefree/) and [post2](https://mnjnsng.github.io/posts/2022/10/tdlearning-iid/)), we proved the convergence of the TD learning algorithm under noise-free and i.i.d sampling assumption, respectively. This guaranteed convergence of the algorithm makes the TD learning very powerful in solving reinforcement learning problems. However, one drawback of this method is in its asynchronisity, which was briefly mentioned in the conclusion of this [post](https://mnjnsng.github.io/posts/2022/09/tdlearning/). When we update for $$Q_{k+1}(s,a)$$, $$\{(s',a')\in \mathcal{S}\times\mathcal{A} \vert (s',a') \neq (s,a)\}$$ are not updated such that $$Q_{k+1}(s',a') = Q_{k}(s',a') $$.

The problem of tabular approach is that the cardinality of $$\mathcal{S}\times\mathcal{A}$$ can be extremely large such that the rate of convergence can become extremely slow (and therefore sample inefficient). To tackle such issue, in this post, the concept of Linear Function Approximation (LFA) is introduced. Briefly, TD learning with LFA makes a low dimensional approximation of the Q-function and updates the parameters that characterize the approximation.

## TD learning with LFA

Let's first rewrite the TD learning algorithm.

$$Q_{k+1} (s,a) = (1-\epsilon_k)Q_k + \epsilon_k(r(s,a)+\gamma Q_k(s',a')).$$

Notice that this is equivalent to an _one-step gradient_ to minimize

$$
\begin{aligned}
\min_Q \underbrace{\frac{1}{2}\left(Q_k(s,a)-r(s,a)-\gamma Q_k(s',a')\right)^2.}_{=:{g(Q_k(s,a))}}
\end{aligned}
$$

To be specific,

$$
\begin{aligned}
Q_{k+1}(s,a) &= Q_k(s,a)-\epsilon_k \nabla g(Q_k(s,a))\quad {\text{(one-step gradient)}}\\
&= Q_k(s,a) - \epsilon_k(Q_k(s,a)-r(s,a)-\gamma Q_k(s',a'))\\
&= (1-\epsilon_k) Q_k+\epsilon_k(r(s,a) + \gamma Q_k(s',a'))
\end{aligned}
$$

retrieving the TD learning algorithm.

Now, the idea of the LFA is to apply this idea to the parameters of the linear approximation of the Q-function instead of Q-function itself. Formally, assume that there exists a feature vector $$\phi(s,a)$$ such that the Q-function can be represented as $$ Q(s,a) = \theta^\top \phi(s,a)$$ for $$\theta \in \mathbb{R}^d, \quad d \ll \vert\mathcal{S}\vert\vert\mathcal{A}\vert$$, where $$\theta$$ is the parameter vector.

Having the linear representation, we update the parameter vector according to the one-step gradient rule. In particular, plug replace $$Q_k(s,a)$$ in the minimization above with $$\theta_k^\top \phi(s,a)$$ and apply the one-step gradient descent. This yields

$$
\begin{aligned}
\theta_{k+1} = \theta_k-\epsilon_k\left(\theta^\top_k \phi(s,a) - r(s,a) - \gamma \theta_k^\top \phi(s,a)\right)\phi(s,a).
\end{aligned}
$$

Writing the asynchronous update in the vectorized form, we obtain

$$
\begin{aligned}
\theta_{k+1} &= \theta_k-\epsilon_k\left(\Phi^\top D_k \Phi \theta_k - \Phi^\top D_k r-\gamma \Phi^\top E_k\Phi \theta_k\right)\\
&= \theta_k-\epsilon_k\left(\Phi^\top D^\pi \Phi \theta_k - \Phi^\top D^\pi r-\gamma \Phi^\top D^\pi P^\pi\Phi \theta_k\right) - \epsilon_k n_k.
\end{aligned}
$$

The rows of $$\Phi$$ are the feature vectors $$\phi_i^\top \quad i\in \{1,\ldots,\vert\mathcal{S}\vert\vert\mathcal{A}\vert\}.$$ Rest is similar to what we did with tabular TD learning in this [post](https://mnjnsng.github.io/posts/2022/10/tdlearning-noisefree/).

One might notice that this representation is somewhat similar to the tabular TD learning, but also is different in that it has the additional $\Phi$ term. This complicates the analysis that we will do in the following posts because we cannot directly use the fact that $$T^\pi$$ is a contraction mapping as we did with the tabular case.
