---
title: "Normed and Inner product spaces"
date: 2023-03-15
permalink: /posts/2023/03/Linear_analysis/
tags:
- Linear Analysis
---

In this post, some definitions of the normed and inner product space is given with illustrative examples. Then, the small gain theorem is stated.

> **(Definition)** A norm $$\|\cdot\|_\mathcal{V}$$ on a vector space $$\mathcal{V}$$ is a function mapping $$\mathcal{V}\rightarrow [0,\infty)$$ which, for each $$v\in \mathcal{V}$$, satisfies
    1. $$\|v\|_\mathcal{V}=0$$ if and only if $$v=0$$;
    2. $$|\alpha| \cdot \|v\|_\mathcal{V} = \|\alpha v\|_\mathcal{V}$$ for all scalars $$\alpha$$;
    3. $$\|u+v\|_\mathcal{V}\leq \|u\|_\mathcal{V}+\|v\|_\mathcal{V}$$.

**(Example)** For $$p\geq 1$$, $$L_p(-\infty,\infty)$$ denote the vector space of functions mapping $$\mathbb{R}$$ to $$\mathbb{C}^n$$ that satisfy   $$\int_{-\infty}^\infty \vert u(t)\vert _p^pdt < \infty.$$

We will typically deal with $$L_2$$ and $$L_\infty$$ space.
Such definition of norm can be extended to matrices, or more generally, operators by using the induced norm given by

$$
\|F\|_{\mathcal{V}\rightarrow \mathcal{Z}} = \sup_{v\in \mathcal{V}, v\neq 0} \frac{\|Fv\|_\mathcal{Z}}{\|v\|_\mathcal{V}} = \sup_{v\in \mathcal{V},\|v\|_\mathcal{V} = 1}\|Fv\|_\mathcal{Z}.
$$

> **(Definition)** An inner product $$<\cdot,\cdot>_{\mathcal{V}}$$ on a vector space $$\mathcal{V}$$ is a function mapping $$\mathcal{V}\times\mathcal{V} \rightarrow \mathbb{F}$$:
    1. the inner product $$<v,v>_\mathcal{V}$$ is non-negative for all $$v\in\mathcal{V}$$;
    2. $$<v,v>_\mathcal{V} = 0$$ iff $$v=0$$;
    3. the mapping $$u\mapsto <u,v>_\mathcal{V}$$ is linear on $$\mathcal{V}$$;
    4. $$<u,v>_\mathcal{V}$$ is the complex conjugate of $$<v,u>_\mathcal{V}$$ for all $$u,v\in \mathcal{V}$$.

Inner product defined with the inner product is called an inner product space:

$$\|v\| = \sqrt{<v,v>}.$$

When the inner product space is $$\mathbb{R}^n$$ or $$\mathbb{C}^n$$, the innerproduct is often given by

$$<x,y> = x^*y.$$

When $$\mathcal{V} = \mathbb{C}^n$$,

$$ \|F\|_\mathcal{V} = \sup_{v \in \mathcal{V}, \|v\|=1} \|Fv\| = \sup_{v \in \mathcal{V}, \|v\|=1} \|v^* F^*Fv\| =:\bar{\sigma}(F).$$

> **(Cauchy-Schwartz inequality)**

$$|<u,v>| \leq \|u\|\|v\|.$$

Matrix extension of the concept of inner product is

$$<A,B> = TrA^*B$$

for $$A,B\in \mathbb{C}^{m\times n}.$$ Note that this induces the Frobenius norm.  

> Frobenius norm is not an induced norm and therefore cannot be used as an operator norm which has fundamental connection to the eigenvalue of an operator.

Likewise, the inner product for $$L_2$$ is given by

$$<x,y> = \int_{-\infty}^{\infty} x^*(t)y(t)dt.$$

> **(Completeness)** A normed space is complete if every Cauchy sequence in it converges.

> **(Hilbert Space)** Hilbert space is a complete inner product space with the norm induced by its inner product.

> $$L_2$$ is the only Hilbert space among $$L_p$$ spaces.

The set of operators $$\mathcal{L}$$ on a Banach space (not necessarily Hilbert) endows an algebraic structure. One of the main property is that

$$\|FG\|\leq \|F\|\cdot\|G\|$$

which is known as the submultiplicative property.

> **(Small gain theorem)**  $Q$ is a member of a Banach algebra. If $$\|Q\|<1$$ then, $$(I-Q)^{-1}$$ exists, and satisfies
  $$(I-Q)^{-1}=\sum_{k=0}^\infty Q^k.$$
