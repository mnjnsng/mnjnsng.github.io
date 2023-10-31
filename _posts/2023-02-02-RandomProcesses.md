---
title: "Random Processes"
date: 2023-02-02
permalink: /posts/2023/02/Random_Processes/
tags:
  - Random Processes
---

In this post, we review some preliminaries and axiioms to understand the fundamentals of random processes.

# Probability Preliminaries

#### Axioms of Probability Theory

**Kolmogorov's axioms of probability** : [link](https://en.wikipedia.org/wiki/Probability_space)

Let $(\Omega, F, P)$

- $P(E)$ being the probability of some event $E$, and $P(\Omega) = 1$.
- sample space $\Omega$: Set of possible outcomes
  - event $\omega$ is a subset of $\Omega$. It is said to occur, or to be true when the experiment is performed _if the outcome is in the event._
    > This part can be quite unintuitive at first hand. Say $\omega = \{1,2,3\}$ of a sample space $\Omega = \{1,2,3,4,5,6\}$. $\omega$ is true if the outcome is $1$. The diparity comes from the word _event_, which seems to be a singular noun, but it can be a subset of sample space that includes multiple outcomes.
- event space $F$ is a set of subsets of $\Omega$.
- probability measure $P$ assigns probability to each event.

- **Event axioms** - $F$ is required to satisfy the following axioms:

  - E.1 $\Omega \in F$
  - E.2 If $A\in F$ then $A^c \in F$
  - E.3 If $A, B \in F$ then $A\cup B \in F$.

    - Also, if $A_1, A_2, \cdots$ is a sequence of elements in $F$ then $\bigcup^{\infty}_{i=1} A_i \in F$.
      > Note here that one choice of $F$ that always satisfies the three axioms is the set of all subsets of $\Omega$ or equivalently, a power set. In fact, when dealing with finite or countably infinite sample space $\Omega$, $F$ will be a set of all subsets of $\Omega$. When dealing with uncountably infinite $\Omega$, then $F$ will simply not allow all subsets of such $\Omega$ to be events. More will be described in [Borel's $\sigma$-algebra](https://en.wikipedia.org/wiki/Borel_set).

  - These three axioms are equivalent to saying that the event space is **$\sigma$-algebra**
  - As a consequence of Event axioms, following are true
  - E.4. $\empty $ is an event ($\empty = \Omega ^c \in F$)
  - E.5. If $A,B \in F$, then $AB = (A^c\cup B^c)^c \in F$
  - E.6. More generally, if $B_1,B_2,\cdots \in F$, then $B_1B_2\cdots \in F$.

- **Probability axioms** - The probability measure $P$ is required to satisfy the following axioms:

  - P.1.(Nonnegativity): $P(E) \geq 0 \text{  } \forall E\in F$
  - P.2.(Normalization): $P(\Omega) = 1$
  - P.3.($\sigma$-additivity): $P(A\cup B) = P(A)+P(B)$

    - Extending this to infinite set, an infinite **sequence** of disjoint sets (mutually exclusive events) satisfies: $$P(\bigcup^\infty_{i=1} E_i ) = \sum^{\infty}_{i=1}P(E_i) $$

    - What **sequence** implies is that the infinite set is countable.
    - In other words, **probability is assigned to events, not outcomes**.

  - As a consequence of these axioms, following are true.
  - P.4 $P(A) \leq 1$
  - P.5 $P(\empty) = 0$
  - P.6 $P(A)+P(A^c) = 1$
  - P.7 If $A\subset B$, then $P(A)\leq P(B)$.
  - P.8 $P(A\cup B) = P(A)+P(B)-P(A\cap B)$.
  - P.9 (Union bound) $P(A\cup B) \leq P(A)+P(B)$
  - etc.
  - Helpful [video](https://youtu.be/DqGUwoz4d4M)

---

#### Sample spaces with infinite cardinality

> Now dealing with finite or countably infinite sample space is straightforward. What is not clear, however, is a sample space that has uncountable cardinality, such as $[a,b]\in \mathbb{R}$. We start by looking at the following example

- **Example** (Standard unit-interval probability space):

  - $\Omega = [0,1]$
  - Draw $\omega$ from $\Omega$, with no preference for drawing from one interval or another. Then, for $0\leq a\leq b \leq 1$,
    $$P([a,b]) = b-a$$
  - If $a=b$, then we are to consider event $\{a\}$ as a singleton sets, which have a probability $P([a,b])=0$, since the cardinality of $\Omega$ is inifinite, while the event is a set of a countable outcome.
  - It says " In order for the event axioms to be true, open intervals $(a, b)$ must also be events and $P( (a, b) ) = b-a$" ... why?
  - $F$ contains all the closed and open subsets of $\Omega$.

  ***

#### Borel's Algebra

- Smallest $\sigma$-algebra containing all subintervals of $\Omega$

#### Lemma: Continuity of Probability

- Suppose $B_1, B_2, \cdots$ is a sequence of events. Then,

    (a) If $ B*1 \subset B_2 \subset \cdots $. Then, $\lim*{j\rightarrow \infty} P(B*j) = P(\cup*{i=1}^\infty B*i) $
    (b) If $ B_1 \supset B_2 \supset \cdots $. Then, $\lim*{j\rightarrow \infty} P(B*j) = P(\cap*{i=1}^\infty B_i) $

    > These look quite wierd that it is written as $P(\cup_{i=1}^\infty  B_i)$ instead of $P(\lim_{i\rightarrow \infty}  B_i)$. I think it is just a conventional expression to write limit expression of an event set as such.

#### Independence and Conditional probability

- Events $A_1$ and $A_2$ are defined to be **independent** if $P(A_1A_2) = P(A_1)P(A_2)$. This definition can be extended to $A_1, A_2, \cdots A_k$. Weaker condition _pairwise independent_ dan be defined for $A_i$ and $A_j$ for all $1<i<j\leq k$.

- **Conditional probability** of $A$ given $B$ is defined by: $$P(A|B) = \frac{P(AB)}{P(B)}$$ for $P(B) \neq 0$.

- Events $E_1, E_2 \cdots E_k$ are said to form a partition of $\Omega$ if the events are mutually exclusive and $\Omega = E_1 \cup E_2 \cdots \cup E_k$.
- The

**Remarks**

- If $A$ and $B$ are independent, then $A^c$ and $B$ are independent
  - But $A^c$ and $A$ are **NOT** independent.
    > We can check this with the definition, but more intuitively, if we have an event $A$, we can be certain that $A^c$ has not happened. Therefore two are highly dependent events.
- For independent $A,B$, $P(A|B) = P(A)$
- For mutually exclusive $E_1, E_2, \cdots E_k$ which are partition of $\Omega$, $P(A) = P(AE_1)+P(AE_2) \cdots +P(AE_k)$.
  - If $E_i \neq 0$, then $P(A) = P(A|E_1)P(E_1) + \cdots P(A|E_k)P(E_k)$, which leads to [_Bayes formula_](https://en.wikipedia.org/wiki/Bayes%27_theorem)
    $$P(E_i|A) = \frac{P(AE_i)}{P(A)} = \frac{P(A|E_i)P(E_i)}{P(A)} = \frac{P(A|E_i)P(E_i)}{P(A|E_i)P(E_i)+\cdots+P(A|E_k)P(E_k)} $$
- Independence is not _transitive_ , meaning that $A$, $B$ are indepdent and $B$,$C$ are independent does not necessarily imply that $A$, $C$ are independent. Consider $A=C$.
- Pairwise independence of a set of events does not imply independence. [Example](<https://en.wikipedia.org/wiki/Independence_(probability_theory)#Pairwise_and_mutual_independence>).
