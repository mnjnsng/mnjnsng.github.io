---
title: "Gradient Descent"
date: 2023-01-19
permalink: /posts/2023/01/Gradient-Descent/
tags:
  - Gradient Descent
  - Reinforcement Learning
  - Optimization
---

In this post, we will briefly explain what Gradient Descent (GD) is, how it works, why it is useful, and where it is used.

Imagine you're on a hilly terrain, blindfolded, and your goal is to reach the lowest valley. The best strategy you can think of is to take small steps in the steepest downward direction to gradually descend and eventually find that valley. When you don't have anywhere else to descend, congratulations, you have reached the valley! This process of moving in the steepest descent direction is analogous to what gradient descent does in mathematical optimization.

Now let's repeat what I just said in mathematical terms. Mathematically, gradient descent involves calculating the gradient of a cost function with respect to the model's parameters. The gradient points in the direction of the steepest ascent of the function. By taking steps in the opposite direction of the gradient, we move closer to the minimum. The size of each step is controlled by a parameter called the learning rate. A larger learning rate might lead to overshooting the minimum, while a smaller rate might slow down convergence. The gradient descent algorithm can be written by the following equation:

$$x_{t+1} = x_k-\epsilon_k\nabla f(x_k),$$

where $\epsilon_k$ is the learning rate at timestep $k$.

To mathematically prove the convergence of such optimization strategy, some assumptions need to be posed and some conditions will have to be satisfied. But first, let's look at the following lemma.  

### Descent Lemma

Suppose $$f\in \mathcal{C}^1_L: \|\nabla f(y)-\nabla f(x) \|\leq L\|y-x\|.$$ Then,

$$f(y)\leq f(x)+ \left<\nabla f(x),y-x\right> + \frac{L}{2}\|y-x\|^2_2.$$

Before we prove this lemma, let's first try to understand what this inequality is saying. Clearly, the first two terms of the RHS correspond to the first order Taylor series approximation of $$f(y)$$ at $x$. Whereas, the last term, stated by the lipschitz constant and the squared norm, must be related to the boundedness of the gradient of $f$. This sort of makes sense, because the gradient of $f$ is bounded, and therefore, an approximation of a function can also be bounded by the lipschits constant.

**Proof**
By the Taylor's theorem,

$$
\begin{aligned}
f(y) &= f(x)+\int_0^1\langle\nabla f(x+t(y-x)),y-x\rangle dt.
\end{aligned}
$$

Subtract $$f(x) + \langle\nabla f(x),y-x\rangle$$ and apply absolute value on both sides to obtain

$$
\begin{aligned}
|f(y) - f(x) - \langle \nabla f(x), y-x\rangle| &= | \int_0^1 \langle \nabla f(x+t(y-x))-\nabla f(x),y-x\rangle dt |\\
&\leq \int_0^1 | \langle \nabla f(x+t(y-x))-\nabla f(x),y-x\rangle|  dt\\
&\leq \int_0^1 \| \nabla f(x+t(y-x))-\nabla f(x)\|_2\|y-x\|_2 dt\\
&\leq L\int_0^1 t\|x-y\|_2^2 = \frac{L}{2}\|y-x\|^2_2 ~~_\blacksquare
\end{aligned}
$$

Now we are ready to prove the Convergence of GD.

## Convergence of GD

By the descent lemma we just proved, we have

$$
\begin{aligned}
f(x_{k+1})-f(x_k) &\leq \langle \nabla f(x_k),x_{k+1}-x_k \rangle + \frac{L}{2}\|x_{k+1}-x_k\|^2_2\\
&= \langle \nabla f(x_k),-\epsilon_k\nabla f(x_k) \rangle + \frac{L}{2}\|\epsilon_k\nabla f(x_k)\|^2_2\\
&= -\epsilon_k\|\nabla f(x_k)\|^2_2+ \frac{L\epsilon_k^2}{2}\|\nabla f(x_k)\|^2_2\\
&= -\epsilon_k(1-\frac{L\epsilon_k}{2})\|\nabla f(x_k)\|^2_2.
\end{aligned}
$$

Now let $$f^_\triangleq \inf_x f(x)$$, and $x^_$ be its argument. Also, let $\epsilon_k \equiv \epsilon \leq 1/L$. Lastly, assume $f$ to be a convex. Subsequently, we have that $$-\epsilon(1-\frac{L\epsilon}{2})\leq -\frac{1}{2}.$$ Plugging this back into the above inequality yields

$$
\begin{aligned}
f(x_{k+1})-f(x_k) \leq -\frac{1}{2}\epsilon \|\nabla f(x_k)\|^2_2.
\end{aligned}
$$

Notice that the RHS is always negative, implying that $$f(x_{k+1})\leq f(x_k)$$. Therefore, we can intuitively think that the GD algorithm will reach the (global) minimum for a (convex) $f$.

Now, apply telescoping summation:

$$
\begin{aligned}
\sum_{t=1}^k \left(f(x_{t+1})-f(x_t)\right) \leq -\frac{1}{2}\epsilon \sum_{t=1}^k\|\nabla f(x_t)\|^2_2.
\end{aligned}
$$

Rearranging this yields

$$ \frac{1}{2}\epsilon \sum_{t=1}^k\|\nabla f(x_t)\|^2_2 \leq f(x_1)-f(x_{k+1})\leq f(x_1)-f(x^*).$$

At this point, we have some idea (lower bound) about how much the function has been decreased. However, since we do not have access to $x^*$, we do not know how close we are to the optimality. To evaluate this criterion, we need to use the convexity assumption.

$$
\begin{aligned}
\|x_{k+1}-x^_\| &= \|x_k-\epsilon \nabla f(x_k) -x^_\|^2_2\\
&= \|x_k-x^_\|^2 +\epsilon^2\|\nabla f(x_k)\|^2 -2\epsilon \langle \nabla f(x_k),x_k-x^_\rangle\\
&\leq \|x_k-x^_\|^2 + \epsilon^2\|\nabla f(x_k)\|^2 +2\epsilon(f(x^_)-f(x_k))~~~(\because \text{convexity}).
\end{aligned}
$$

Applying telescoping summation, we obtain

$$
\|x_{k+1}-x^_\| \leq \epsilon^2 \sum_{t=1}^k\|\nabla f(x_t)\|^2_2 + 2\epsilon \sum_{t=1}^k(f(x^_)-f(x_t)) + \|x_1-x^*\|.
$$

Rearranging this yields

$$
\begin{aligned}
\sum_{t=1}^k (f(x_t)-f(x^_)) &\leq \frac{\epsilon}{2}\sum_{t=1}^k\|\nabla f(x_t)\|^2 + \|x_1-x^_\| - \|x_{k+1}-x^_\| \\
&\leq \frac{\epsilon}{2}\sum_{t=1}^k\|\nabla f(x_t)\|^2 + \|x_1-x^_\| \leq \epsilon M+ \|x_1-x^*\|,
\end{aligned}
$$

where finite $M$ exists because $$f\in \mathcal{C}^1_L.$$

In addition, recalling that $f(x_k)$ is a decreasing sequence. Consequently,

$$f(x_{k+1})-f(x^_) \leq \frac{1}{k}\sum_{t=1}^k (f(x_k)-f(x^_)) \leq \frac{1}{k+1}(\epsilon M + \|x_1-x^*\|).$$

As such, for convex $f$, the convergence is achieved with $$O(\frac{1}{k})$$.

Similarly, one can prove that with for strong convexity assumption, an expoential convergence can be achieved. Proof of this is left as an exercise.  
