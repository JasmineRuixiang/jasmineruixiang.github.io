---
layout: post
title: "Manifold and Riemannian Geometry (4): Lie Group and Lie Algebra (in progress)"
date: 2026-02-23 21:39:24
description: Lie group, Lie algebra, and their computation
tags: 
    - "Geometry Concepts/Tools"
    - "Topology Concepts/Tools"
categories: 
    - "Differential Geometry"
featured: false
related_posts: true
related_publications: true
citation: true
math: true
toc:
  beginning: true
  sidebar: left
---



## 0] Introduction and Intuition

---

## 1] Lie Group

---

## 2] Lie Algebra

---

## 3] Computation and Examples
Matrix groups and computations. 

### 3.1] Symmetric Matrix
Visualization/reason of why it is not a Lie group? 

---

### 3.2] Symmetric Positive Definite (SPD) Matrix

#### 3.2.1] Definition and Geometric Intuition
Let's start with the real case. A real matrix $$A \in \mathbb{R}^{n \times n}$$ is called **positive definite** if

$$
x^\top A x > 0 \quad \text{for all } x \neq 0.
$$

Perhaps a necessary clarification here: a positive definite matrix is __NOT__ necessarily symmetric. What actually happens is, say we write $$A$$ as

$$
A = S + K
$$

where

$$
S = \frac{A + A^\top}{2}, \qquad
K = \frac{A - A^\top}{2}.
$$

Here:
- $$S$$ is symmetric
- $K$ is skew-symmetric

Now compute, simply by linearity:

$$
x^\top A x = x^\top S x + x^\top K x.
$$

Notice that for any skew-symmetric matrix $$K$$,

$$
x^\top K x = 0.
$$

Therefore,

$$
x^\top A x = x^\top S x.
$$

This means that the quadratic form only “sees” the symmetric part. Consequently, if $$x^\top A x > 0$$ for all $$x \neq 0$$, then the symmetric part $$S$$ is positive definite. Hence, positive definiteness does NOT force $$A$$ to be symmetric. It only forces the symmetric part of $$A$$ to be positive definite. On the other hand, we can add any skew-symmetric matrix to a _symmetric_ positive definite matrix and the quadratic form will not change at all. $$A$$ can definitely be non-symmetric. 

It's also true that because the value of the "test" depends entirely on the symmetric part, many mathematicians find it cleaner to simply define positive definiteness for symmetric matrices only.

For many textbooks it's typically required that a PD matrix to be also symmetric. That's because it has very strong structure, like real positive eigenvalues, orthogonal diagonalization, and it thus defines an inner product and a Riemannian metric (we will come back to this later on), etc. Without symmetry, eigenvalues can be complex, matrix may not be diagonalizable, and the spectral theory breaks. So in analysis and geometry, “positive definite matrix” is usually defined to mean __symmetric__ positive definite. That’s sometimes also a convention.

The geometric intuition for the fact of skew-symmetric matrix adding no effects on the quadratic form is that skew-symmetric part represents an __infinitesimal rotation__. And rotations do not change $$x^\top Ax$$ because quadratic forms measure "energy" or "stretching", and rotations don't stretch — they just spin. $$S$$ controls stretching, and $$K$$ controls rotation --- the quadratic form only detects stretching. 

Another definition of positive definite is to require 
* 1. $$A$$ symmetric,
* 2. All eigenvalues are positive

where symmetry is explicitly added as part of the definition (just by condition 2 we do NOT necessarily obtain $$x^\top Ax \geq 0$$).

So in practice, people define:

$$
\mathrm{Sym}(n) = \{ A \mid A^\top = A, A \in \mathbb{R}^{n \times n} \}
$$

and then set up the following

$$
\mathrm{SPD}(n)
= \{ A \in \mathrm{Sym}(n) \mid A > 0 \}.
$$

Thus SPD means Symmetric Positive Definite, where the “S” is mostly for clarity and convention.

Finally, to complicate/complete the story a little bit more, for complex matrices, for $$z^* M z$$ to be real and positive, $$M$$ must be Hermitian (the complex analogue of symmetry, I'll not expand here).

---

#### 3.2.2] Geometry of SPD Matrices

Let

$$
\mathrm{SPD}(n)
= \{ A \in \mathbb{R}^{n \times n} \mid A^\top = A,\; A > 0 \}.
$$

First of all, is $$\mathrm{SPD}$$ a group? 

The answer is no. Why? Because 

- Not closed under addition (no additive inverse inside)
- Not closed under multiplication (product not symmetric: if $$A, B \in SPD(n)$$, then in general $$AB \notin SPD(n) $$). Consequently, it's obvious that the space of symmetric positive definite ($$\mathrm{SPD}$$) matrices is __not__ a Lie group under multiplication.

However, notice that $$GL(n)$$ is a Lie group, and 

$$
\mathrm{SPD}(n)
= \mathrm{GL}(n) / \mathrm{O}(n).
$$

Consequently, although it's not a Lie group, it is indeed 

- A smooth manifold of dimension $$\frac{n(n+1)}{2}$$
- A Riemannian symmetric space

---

#### 3.2.3] What topology?
Given that we claim it's a manifold, you may wonder what topology we are using here. Specifically, we identify symmetric matrices with vectors $$\mathbb{R}^{n(n+1)/2}$$ in the Euclidean space. The topology is simply the ``subspace topology`` inherited from Euclidean space.

Why then is it a manifold? Because symmetric matrices form a vector space, positive definiteness is an open condition, and eigenvalues depend continuously on entries.

In effect, $$\mathrm{SPD}$$ matrices form an open cone. 

Therefore:

$$
\mathrm{SPD}(n)
$$

is an **open convex cone** in $$\mathrm{Sym(n)}$$, the vector space of symmetric matrices.

---


