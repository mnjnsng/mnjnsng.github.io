---
title: "Spectral Theory"
date: 2023-03-29
permalink: /posts/2023/03/Spectral_theory/
tags:
- Robust control
- Functional analysis
- Spectral Theory
---

In this post, the definition of a spectrum of a function and its spectral radius is provided. Then, some useful properties are stated, and some examples problems are solved using the properties.

> **(Definition)** Let $$\mathcal{V}$$ be a Banach Space, and $$M\in \mathcal{L}(\mathcal{V}).$$ The spectrum of $M$ is defined by
$$spec(M) := \{\lambda\in \mathbb{C} : (\lambda I-M) \text{ is not invertible in } \mathcal{L}(\mathcal{V})\},$$
    and the spectral radius of $M$ by
$$\rho(M):= \max\{|\lambda|:\lambda \in spec(M)\}.$$

Spectral radius intuitively is the maximum magnitude of a gain of the operator. Following are useful properties of the spectral radius.

1. $$\rho(M)\leq \|M\|$$
2. $$\rho(M) = \lim_{k\rightarrow\infty}\|M^k\|^\frac{1}{k}$$
3. $$\rho(MQ) = \rho(QM)$$
4. $$M=M^*\Rightarrow \rho(M) = \|M\|$$
5. $$\|M\|^2 = \|M^*M\| = \rho(M^*M).$$

Note that (1) holds due to the small gain theorem.

The adjoint operator in Hilbert space is defined as follows (it is defined in Hiilbert space since it is defined by the inner product).

> **(Definition)**  Suppose $$\mathcal{V}, \mathcal{Z}$$ are Hilbert spaces, and $$F\in\mathcal{L}(\mathcal{V},\mathcal{Z}). $$ The operator $$F^*\in \mathcal{L}(\mathcal{Z},\mathcal{V})$$ is the adjoint of $F$ if
$$\left<z,Fv\right>_\mathcal{Z} = \left<F^*z,v\right>_\mathcal{V}$$
for all $$v\in \mathcal{V}$$ and $$z\in\mathcal{Z}$$.

An operator $U$ is isometric if it satisfies

$$U^*U=I.$$

Notice that

$$\left<Uv_1,Uv_2\right> = \left<v_1,v_2\right>$$

which implies that such operator $U$ preserves the norm. A consequence of this is that $$\|U\|=1$$.

More restrictive is the *unitary operator*, if it further satisfies $$U^*=U^{-1}$$. If there exists a unitary operator $$U \in \mathcal{L}(\mathcal{V},\mathcal{Z})$$, then $$\mathcal{V},\mathcal{Z}$$ are *isomorphic*.
