---
title: 'Temporal Difference (TD) Learning: Understanding the idea'
date: 2022-09-29
permalink: /posts/2022/09/tdlearning/
tags:
  - TD Learning
  - Reinforcement Learning
---

Value iteration and policy iteration methods as discussed in the previous [post](https://mnjnsng.github.io/posts/2022/09/vipi/), requires the knowledge of the transition matrix. To be precise, the calculation of the Bellman operator requires the calculation of the expected value of the value function (or a Q-function) of the next possible state (and action) given current state (and action). In other words, the model of the Markov Decision Process (MDP) is assumed to be known in advance. This was a feasible assumption when we have an access to the system or a simulator used to collect the data. However, it is not always the case, and therefore we need some clever method to find an optimal policy under such circumstances.

## Monte-Carlo Methods

One simple idea is to leverage the Law of Large Numbers (LLN). That is, one starts at state $s$, follow policy $\pi$ to generate sample trajectories, and then calculate the total discounted reward and then average it out. Simply speaking, run as many sample trajectories starting from the state $s$ and the average reward of those trajectories will be the value of that state. Compactly,

$$ V(s) = \lim_{n\rightarrow \infty}\frac{\sum_{i=1}^n G_i(s)}{n}$$

where $G_i(s)$ represents the total reward in trajectory $i$ starting from $s$.

Although this method is very intuitive to comprehend, there are couple problems.

1. We don't know when we should stop sampling for each trajectory and start the next one. If the environment is episodic, it is not an issue, but what if it is not (i.e. an infinite horizon problem)? One might argue that we can simply choose large enough number as a threshold at least for the discounted reward problem, where the reward will become geometrically small. This is a reasonable point to make, however, we have bigger problem.

2. If we have to run several (possibly infinitely many) trajectories for each state, then how are we going to complete collecting samples for all $s$? Imagine state space to have a finite but very large cardinality (i.e. state space of a chess).

## TD Learning

TD learning method is a stochastic approximation based method that updates the estimates in an online-manner, unlike Monte-Carlo method that could make an update only after each trajectory. In addition, TD learning method updates the estimates based on the estimates learned so far, which we call it *bootstrapping*. This is done by cleverly exploiting the Bellman equation.

Consider a Q-function where we follow the fixed policy $\pi$. Then the Bellman equation becomes the following.

$$Q^\pi (s,a) = \mathbb{E}[r(s,a) + \gamma Q^\pi(s',a')] = \bar{r}(s,a) + \gamma \sum_{s',a'} P^\pi(s',a'\vert s,a)Q^\pi(s',a').$$

As mentioned above, we do not know $P^\pi$ in advance, therefore we cannot directly calculate the expected value. What we do have is the samples $s_0, a_0, r_0, s_1, a_1, r_1, \cdots$ from which we can make a noisy estimate of $Q^\pi(s,a)$ for each state and action pair of the trajectory. Simply speaking, the noisy estimate of $Q^\pi(s,a)$ can be calculated as:

$$Q^\pi (s,a) = \mathbb{E}[r(s,a) + \gamma Q^\pi(s',a')] \simeq r(s,a) + \gamma Q^\pi(s',a').$$

However, imagine a case when you have observed $Q(s,a)=1$ 9 times, but at the 10th time you visited the same state and action pair you obtained $0$. Then your $Q(s,a)$ will immediately be updated as $0$ depite your 9 times of making an observation of $1$. This seems like an unfair update law. Then how can we make it fair? We can simply use the mean estimation method.

$$\begin{aligned}
\mu_{n+1} &= \frac{X_1 + \cdots + X_{n+1}}{n+1}  \\
&=  \frac{n}{n+1} \frac{X_1 + \cdots + X_n}{n} + \frac{X_{n+1}}{n+1}\\
&=  \frac{n\mu_n}{n+1} + \frac{X_{n+1}}{n+1} =: (1-\epsilon_n) \mu_n+\epsilon_n X_{n+1}.
\end{aligned}$$

Here $X_k$ is a random variable sampled at time step $k$. Inspired byt this observation, we may update $Q$ value not directly with the noisy observation but with the mean estimation of the $Q$ value:

$$Q_k^\pi (s,a) = (1-\epsilon_k)Q_k^\pi(s,a) + \epsilon_k(r(s,a) + \gamma Q_k^\pi(s',a')).$$

This is the TD learning algorithm. Note that in the mean estimation example we particularly used $\epsilon_n = 1/(n+1)$, but in the TD learning we may choose any $\epsilon_k $ that  $\epsilon_k \rightarrow 0$ as $k\rightarrow \infty$ in general.

Another thing to note here is that this is an *asynchronous* update algorithm such that you only update $Q^\pi$ for a single $(s,a)$ at a time, and Q values for all the other states remains unchanged. As a result of this, the convergence of $Q$ learning (the proof of convergence will be discussed in later posts) will become very slow as the cardinality of $\mathcal{S} \times \mathcal{A}$ becomes large.

------
