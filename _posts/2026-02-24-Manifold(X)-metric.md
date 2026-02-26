---
layout: post
title: "Manifold and Riemannian Geometry (X): Riemannian metric and Riemannian manifold (in progress)"
date: 2026-02-24 23:06:01
description: Exploration of metric, and Riemannian manifold
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

## 0] Intuition and Introduction

## 1] Riemannian Metric

## 2] Riemannian Manifold

### 2.1] Definitions

### 2.2] Examples

#### 2.2.1] Symmetric Positive Definite Matrix
Recall from the blog on Lie group/Lie algebra that we talked about the set of symmetric positive definite matrices: 

$$
\mathrm{SPD}(n)
= \{ A \in \mathbb{R}^{n \times n} \mid A^\top = A,\; A > 0 \}.
$$, 

which is not actually a Lie group but a smooth manifold. In particular, this framework pops up a lot in brian signals analysis given that data (spatial) covariance matrices are exactly $$\mathrm{SPD}$$ matrices (reference ...). 

Since $$\mathrm{SPD}(n)$$ is a manifold, then we could naturally apply some Riemannian metric to it, after which we could calculate distances and geodesics. Of course, there is no unique way of specifying a metric and there are several important ones. To have a taste of how that might look like, let me present the observations in a backward order: Based on different selected metrics, we might have different notions of distances as below: 

* (A) Euclidean / Frobenius distance

$$
d(A,B) = \|A - B\|_F.
$$

Here we just treat matrices and vectors. This is simple but not geometrically natural, and it's not invariant under coordinate changes. 

* (B) Affine-Invariant Riemannian Metric (AIRM, Canonical)

$$
d(A,B)
= \left\|
\log\left(A^{-1/2} B A^{-1/2}\right)
\right\|_F.
$$

This distance has specific properties:

- Geometrically natural 
- Invariant under congruence transforms:
  $$
  A \mapsto M A M^\top
  $$
- __Nonpositive__ curvature
- Geodesic:
  $$
  \gamma(t)
  = A^{1/2}
  \left(A^{-1/2} B A^{-1/2}\right)^t
  A^{1/2}.
  $$ (which is nicely multiplicative)

Notice that this metric turns $$\mathrm{SPD(n)}$$ into a Riemannian symmetric space. The affine-invariant metric is the canonical one because it respects the natural symmetry of the space.

$$
\mathrm{SPD}(n)
= \mathrm{GL}(n) / \mathrm{O}(n).
$$

* (C) Log-Euclidean Metric

$$
d(A,B)
= \|\log A - \log B\|_F.
$$

Here log refers to matrix logarithm. This computation is very easy, and makes the space "flat" after log transform. 

* (D) Bures–Wasserstein Metric

$$
d^2(A,B)
= \mathrm{tr}(A) + \mathrm{tr}(B) - 2 \mathrm{tr}\left( (A^{1/2} B A^{1/2})^{1/2} \right).
$$

This choice is deeply related to optimal transport and quantum information theory. 

---

I want to dwell here a little longer to illustrate the computation of the affine-invariant distance. Practically, the process is as follows: 

Let $$A, B \in \mathrm{SPD}(n)$$.

The affine-invariant distance, as presented above, is

$$
d(A,B)
= \left\|
\log\left(A^{-1/2} B A^{-1/2}\right)
\right\|_F.
$$

We first do diagonalization: since $$A^{-1/2} B A^{-1/2}$$ is SPD, 

$$
A^{-1/2} B A^{-1/2}
= Q \Lambda Q^\top,
$$

where

$$
\Lambda = \mathrm{diag}(\lambda_1,\dots,\lambda_n),
\quad \lambda_i > 0.
$$

These $\lambda_i$ are eigenvalues of $$A^{-1}B$$ (they are also called the generalized eigenvalues of the pair $$(A,B)$$). Then we apply matrix logarithm and it's not hard to see that 

$$
\log(A^{-1/2} B A^{-1/2})
= Q \mathrm{diag}(\log \lambda_1,\dots,\log \lambda_n) Q^\top.
$$

On the other hand, notice that the Frobenius norm is invariant under orthogonal conjugation:

$$
\|Q D Q^\top\|_F = \|D\|_F.
$$

So

$$
d(A,B)
= \left(
\sum_{i=1}^n (\log \lambda_i)^2
\right)^{1/2}.
$$

This final formula is very clean: if $$\lambda_i$$ are eigenvalues of $$A^{-1}B$$, then:

$$
\boxed{
d(A,B)
= \left(
\sum_{i=1}^n
(\log \lambda_i)^2
\right)^{1/2}
}
$$

Furthermore, to connect back to the log-Euclidean metric, notice that one special case: if $$A$$ and $$B$$ commute (so they would share eigenvalues):

$$
d(A,B)
= \|\log A - \log B\|_F.
$$

---

We could interpret this distance as

- Diagonalize in coordinates where $$A = I$$.
- Measure how much $$B$$ stretches each principal axis.
- Take the Euclidean norm of the log-stretches.

So the distance is just Euclidean distance between the log-eigenvalues.

The affine-invariant metric makes SPD matrices behave like $$\mathbb{R}^n$$ (in log-eigenvalue coordinates), except that

- eigenvectors rotate smoothly,
- curvature is nonpositive.
