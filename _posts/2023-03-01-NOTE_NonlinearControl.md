---
title: "Some preliminaries on Nonlinear Control"
date: 2023-03-01
permalink: /posts/2023/03/Preliminaries/
tags:
- Nonlinear Control
---

In this post, we review some mathematical preliminaries that are important in understanding the fundamentals of Nonlinear Control theory.

# Mathematical Preliminaries

> Disclaimer: This is a personal note on a lecture that the author took. This document is not intended to be distributed, and it may include some (many) errors.

#### Dynamical Systems

**Dynamical systems** are systems described via system of differential equations:
$$\dot x (t) = f(t,x(t)), x(t_0) = x_0 $$

- $x\in \mathbb{R}^n$: state vector
- $t \in [t_0,\infty]$: time variable
- $x_0$ : initial condition of the state vector
- $f: \mathbb{R}\times \mathbb{R}^n \rightarrow \mathbb{R}^n$: function that characterizes the dynamical system.

> When analyzing a dynamical system, we would like to specify(predict) how the system will evolve. For example, when we have a pendulum, we would like to know the angle and height of the pendulum at certain time. To have such an analysis to be valid, we need to guarantee that the state will be defined in the future time. Moreover, we expect that state to be repeatedly achievable. In other words, it shall be unique. Therefore, we need to discuss about the existence and uniqueness of the solution.

---

#### Lemma 1

If $f(t,x)$ is piece-wise continuous in $t$ and locally Lipschitz in $x$, such that
$$||f(t,x_1) - f(t,x_2)|| \leq L_r||x_1-x_2||, \\\forall x_1,x_2 \in \mathcal{B_r} = \{x\in \mathbb{R}^n: ||x-x_0||\leq r\},\\ \forall t\in [t_0,t_1], L_r>0$$
Then, there exists $\delta>0$ such that the system has a unique solution for $t\in[t_0,t_0+\delta]$.

- $\mathcal{B}\_r $ is a compact set
- $L_r$ is Lipschitz in $x$ over $\mathcal{B}_r$ in $t$ over $t \in [t_0,t_1]$
- Piecewise continuity implies that the solution is valid _almost everywhere_, except for the discontinuous points.

---

> Here, three questions arise before speaking of [proof](https://www.cs.ucr.edu/~craigs/ucla-courses/135.2.16s/proof-local.pdf). 1. What does locally Lipschitz mean? 2. Why is a _compact_ here? Following are answers to that.

#### Lipschitz condition

[wikipedia](https://en.wikipedia.org/wiki/Lipschitz_continuity#Examples)
Consider a real valued function.
A function $f:R\rightarrow R$ is Lipschitz continuous if there exists a $K>0$ such that for all real $x_1$ and $x_2$, $$|f(x_1)-f(x_2)| \leq K|x_1-x_2|$$

> Intuitively this means that the derivative of a function is bounded by a number.

Locally Lipschitz continuous function is a function every $x$ in $X$ there exists a [neighborhood](<https://en.wikipedia.org/wiki/Neighbourhood_(mathematics)>) $U$ of $x$ such that $f$ restricted to $U$ is Lipschitz continuous.

Some things to notice here is that when $f:R\rightarrow R$, the Lipschitz condition can be written as $$ \frac{|f(y)-f(x)|}{|y-x|} \leq L $$

- Any function $f(x)$ that has infinite slope at some point cannnot be locally Lipschitz at that point.
  - Any continuously differentiable function (derivative is continuous, smooth) is locally Lipschitz
- Continuity does not guarantee Lipschitz continuity
  - Not even uniformity (uniformly continuous) does not guarantee Lipschitz continuity
- If $|f'(x)|$ is bounded by $k$ over the interval of interest, then $f(x)$ is Lipschitz on the same interval with $L_r = k$

  - In general, continuity of the derivative is not required for the function to be Lipschitz

- Continuously differentiable $\subset$ Lipschitz continuous $\subset$ continuous

---

#### Lemma 2 Jacobian and Lipschitz Constant

Let $f:[a,b]\times D \rightarrow R^m$ be continuous for some domain $D\subset R^n$. Suppose that $\frac{\partial f}{\partial x}$ exixts and is continuous on $[a,b] \times D$. If, for a convex subset $W\subset D$ there is a constant $L\geq 0$ such that
$$||\frac{\partial f}{\partial x}(t,x)|| \leq L$$
on $[a,b] \times W$ then, $$||f(t,x)-f(t,y)|| \leq L||x-y||$$
for all $t\in [a,b]$, $x\in W$ and $y \in W$.

- This lemma points out how a Lipschitz constant can be calculated using the knowledge of $\frac{\partial f}{\partial x} (t,x)$ [Link](https://www.eee.hku.hk/~msang/Numerical_Lipschitz.pdf)

---

#### Lemma 3

If $f(t,x)$ and $\frac{\partial f}{\partial x} (t,x)$ are continuous on $[t_0,t_1]\times R^n$, then $f$ is globally Lipschitz in $x$ iff $\frac{\partial f}{\partial x} (t,x)$ is uniformly bounded on $[t_0,t_1]\times R^n$.

- uniformly bounded means that the function is bounded by a real number:
  $$|f_i(x)|\leq M, \text{  }\forall i \in I,\text{  } \forall x \in X$$

---

#### Lemma 4

If $f(t,x)$ and $\frac{\partial f}{\partial x} (t,x)$ are continuous on $[t_0,t_1]\times D$ for some open domain $D \subset R^n$, then $f$ is locally Lipschitz in $x$ on $[t_0,t_1]\times D$.

- This simply means that a continuously differentiable function is locally Lipschitz.

---

**Finite escape time** is used to describe the phenomenon that a trajectory escapes to infinity at a finite time.

For example, $\dot{x}=-x^2 $ with $x(0)= -1$ is locally Lipschitz for all $x\in R$ since $f'(x) = -2x$ is bounded on any compact subset of $R$. Therefore we can conclude that the solution is unique.

However, the unique solution is $x(t) = \frac{1}{t-1}$. Since $t_0 = 0$, $T>0$. At this point, we can see that $T$ cannot be greater than 1. Thus, the solution exists only over $[0,1)$. And we call $T$ the finite escape time.

> This terminology of finite escape time points out that the interval existence of the solution cannot be extended indefinitely, such that there is a maximum interval $[t_0,T)$ where the unique solution starting at $(t_0,x_0)$ exists. Then what is the condition for the solution be extended indefinitely?

#### Theorem 1 Global Existence and Uniqueness

SUppose that $f(t,x)$ is piece-wise coontinuous in $t$ and satisfies
$$||f(t,x)-f(t,y)||\leq L||x-y||\text{  } \forall x,y\in R^n, \forall t\in [t_0,t_1]$$ Then, the state equation $\dot{x} = f(t,x)$ with $x(t_0) = x_0$ has a unique solution over $[t_0,t_1]$, where $t_1$ can be arbitrarily large.

- Note the difference that $x,y$ are no longer an element of a compact subset of $R$, but whole $R$.

> This I guess is related to Lemma 3. If the function is globally Lipschitz, then the solution exists and is unique for all $t$ with indefinite upper bound. But this condition is rather not likely in practice. What we need to care most about is the system that is only locally Lipschitz.

---

#### Theorem 2

Let $f(t,x)$ be piecewise continuous in $t$ and locally Lipschitz in $x$ for all $t\geq t_0$ and all $x$ in a domain $D\subset R^n$. Let $W$ be a compact subset of $D$, $x_0\in W$ and suppose it is known that every solution of
$$\dot{x} = f(t,x), x(t_0) = x_0$$ lies entirely in $W$. Then, there is a unique solution that is defined for all $t\geq t_0$.

> When dealing with locally Lipschitz function, we do not have to find $x(t)$ explicitly. Instead, we can simply check if all the solution lies inside the compact set to conclude that the solution is unique.

---

#### Gronwall-Bellman Lemma

Let $\phi:[t_0,t_1]\rightarrow R$ be continuous, $\psi: [t_0,t_1] \rightarrow R$ be continuous and nonnegative. If $y:[t_0,t_1]\rightarrow R$ satisfies
$$y(t)\leq \phi(t)+ \int_{t_0}^{t}\psi(s)y(s)ds $$
for all $[t_0,t_1]$, then,
$$y(t)\leq \phi(t)+ \int*{t_0}^{t}\psi(s)y(s)\exp\left[\int*{s}^{t}\psi(\tau)d\tau \right]ds $$.
if $\phi(t) = const$ and $\psi(t) = const >0$, then,
$$y(t) \leq \phi\exp(\psi(t-t_0)) $$

- [video](https://youtu.be/MPOfJbOoNtM) explanation/proof of the lemma
- This lemma can be used in proving the uniqueness of a solution function.
- It can also be used to prove hte continuous dependence of the unique solution upon $t_0$.
  > In situations where a physical process is described (modelled) by an initial value problem for a system of ODEs, then it is desirable that any errors made in the measurement of either initial data or the vector field, do not influence the solution very much. In mathematical terms, this is known as continuous dependence of solution of an IVP. [reference](http://www.math.iitb.ac.in/~siva/ma41707/ode91.pdf)

---

#### Theorem 3 Bounded perturbations of the state equation

Let $f(t,x) $ be piecewise continuous in $t$ and Lipschitz in $x$ on $[t_0,t_1]\times D$ with Lipschitz constant $L$, where $D\in R^n$ is an [open connected set](https://en.wikipedia.org/wiki/Connected_space#Path_connectedness). Let $y(t)$ and $z(t)$ be solutions of
$$\dot{y} = f(t,y), y(t_0) = y_0$$
and
$$\dot{z} = f(t,z)+g(t,z), z(t_0) = z_0$$,
which are contained in $D$ over the entire time interval $[t_0,t_1]$. Suppose that
$$||g(t,x)||\leq \mu, \forall(t,x) \in [t_0,t_1]\times D$$
for some $\mu>0$. Then,
$$||y(t)-z(t)|| \leq ||y_0-z_0|| \exp (L(t-t_0)) + \frac{\mu}{L}(\exp (L(t-t_0))-1), \forall t \in [t_0,t_1]$$

> What this tells is that if the perturbation term $g$ is bounded, then the trajectories have at worst an exponentially diverging difference. In other words, over a finite time interval, the two solutions are within a finite (and quantifiable) distance of one another.

- Connected set was brought in as an extension of compact sets into a higher dimensional domain.

---

#### Theorem 4 Continuous dependence on Initial conditions and parameters

[Theorem link](http://www.math.iitb.ac.in/~siva/ma41707/ode91.pdf)

- Extension of boundedness of a solution - under given assumptions for any acceptable range around the nominal trajectory, there is an appropriately small range of initial states and parameters which give rise to trajectories that lie entirely inside the tube.

---

#### Comparison lemma

$$\dot{x} = f(x,t), x(t_0) = x_0$$
$f$ is locally Lipschitz in $x$ and continuous in $t$ for all $t\geq 0$. Let $[t_0,T)$ be the maximal interrval of existence of the solution $x(t)$, where $T$ could be infinity). Let $v(t)$ be a $C^1$ function which satisfies the differential inequality $$\dot{v} \leq f(v,t), v(t_0) \leq x_0$$.
Then, $v(t) \leq x(t)$ for all $t\in [t_0,T)$.

> This is quite obvious, if we think of a function $f$ and $v$ which have the same initial value, but one has smaller gradient values at all times.
> So expressing $x$ with $v$ whih is differentiable, and satisfies a differential inequality, then expressing that boundary again with $x$ and $x_0$ is what this lemma actually does.
