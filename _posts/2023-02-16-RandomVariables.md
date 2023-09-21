---
title: "Random Variables"
date: 2023-02-16
permalink: /posts/2023/02/Random_Variables/
tags:
  - Random variables
---


# Random Variables

#### Random Variables

- Let a probability space $(\Omega, F, P) $ be given. **Random variable** is a function $X$ from $\Omega$ to $\mathbb{R}$ that is _$F$-measurable_, meaning that for any number $c$, $$\{w:X(w) \leq c \in F \}$$

> Many people say that the name _Random variables_ is a terrible name, since it seems like it is a randomly generated number, instead of a function. The name was developed in comparison to the traditional variable (i.e. $x= 2x^2-1$), for which the value of the variable can be either assigned or be solved for. In this context, we can think of a random variable as a generation of a variable as a result of an operation of some arbitrary function.

> Another thing to mention is the necessity of the existence of such concept. It is essentially a mapping from a randomly processed outcome to $\mathbb{R}$, which intuitively is quantifying the outcome. For example, we may assign 1 to head and 0 to tails for flipping a coin. If we do not assign a number, then one would have to calculate $\frac{H}{H+T}$, for which defining the operation would be even harder.

- **Cumulative Distribution Function (CDF)** of a random variable $X$ is denoted by $F_X$. $$F_X(c) = P\{w: X(w)\leq c\} \triangleq P\{X\leq c\}$$

  - Note that the set $\{w: X(w)\leq c \}$ must be well-defined in $F$, according to the $F$-measurable condition of the definition. If not, CDF cannot be well-defined as well.
  - A function $F$ is the CDF of some random variable _if and only if_ it has the following three properties
    1. $F$ is nondecreasing
    2. $\lim_{x\rightarrow +\infty} F(x) = 1 $ and $\lim_{x\rightarrow -\infty} F(x) = 0$
    3. $F$ is right continuous, meaning that $\lim_{x\rightarrow c^+} F(x) = F(c)$
       > Note that the last property does not include left continuity, because there can be some jump discontinuities.

- Discrete random variable is for if it takes in a discrete set as a value (whether it's finite or infinitely countable) $X_1, X_2, \cdots$.

- **Probability mass function (pmf)** is defined for a discrete set : $$p_X(x) = P\{X = x\}$$
- CDF can be expressed with pmf: $$F_X(x) = \sum_{y:y\leq x}p_X(y) $$

  > pmf is more intuitively comprehensible since it directly assigns a probability of certain event. However pmf has its limits in generalizing into continuous domains.

- A random vaiable $X$ is a continuous random variable if the CDF is the integral of a function: $$F_X(x) = \int_{-\infty}^x f_X(y)dy $$ Or equivalently,$$f_X(x) = \frac{dF_X(x)}{dx} $$
  - This is analogous to CDF correlation with pmf. However, in this case, we call $f_X$ as a **probability density function (pdf)**.
  - $$f_X(x) = \frac{dF_X}{dx} = \lim_{\epsilon\rightarrow 0} \frac{1}{\epsilon}\int_{x}^{x+\epsilon}F_X(u)du $$

#### Expectation

- For a simple ($\exists$ finite set $\{x_1,\cdot,x_n\}$ such that $X(w)\in \{x_1,\cdot,x_n\} \forall w \in \Omega$ ) RV, $$E[X] = \sum_{i=1}^{m} x_iP(X=x_i)  $$
- For continuous RV, $$E[X] = \int_{-\infty}^{\infty} xf_X(x) dx $$

> Simple RV is not necessarily a discrete RV. Consider $X= 0$ if $x\in \mathbb{R}/\mathbb{N}$, and $1$ if $x\in\mathbb{N}$. It has piecewise continuous domain, but it is simple.

- Linearity features:
  - $E[cX] = cE[X]$
  - $E[X+Y] = E[X]+E[Y]$
    > This is intuitively obvious, and moreover, if we think of the definition of an expectation, we can superpose $Z= X+Y$ then prove the feature using the indepencence and joint RV distribution.

#### Theorem

- Say $X\geq 0$, and $0\leq E[X] < \infty$
  $$E[X] = \int_{0}^{\infty} P(X>x) dx = \int_{0}^{\infty} F_X^c(x)dx$$

- This can be generalized to $$E[X] = \int_{0}^{\infty} P(X>t) dt - \int_{-\infty}^{0} P(X<t) dt$$

#### Theorem (LOTUS)

- If $Y$ is a function of a real variable, such that $Y= g(X)$, $$E[Y] = \int_{\infty}^{\infty} g(x)f)X(x) dx$$
  > So, simply speaking, when we have another mapping that translates original RV to another domain, we can just multiply that function when calculating for the expected value.
