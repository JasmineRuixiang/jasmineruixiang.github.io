---
layout: post
title: "Manifold and Riemannian Geometry (X): Riemannian metric and Riemannian manifold"
date: 2026-02-24 23:06:01
description: Exploration of metric, and Riemannian manifold
series: Manifold and Riemannian Geometry
# thumbnail: /assets/img/blogs/rmx.png
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

Note: There's a fundamental difference and perhaps surprising connection between Riemannian metric and the usual term of metric used in metric space:

### 1.1] Metric space: global distance as a primitive

In a metric space, we start with a function  
$$
d : X \times X \to \mathbb{R}
$$  
satisfying positivity, symmetry, and the triangle inequality. That’s it. There's no smoothness, no coordinates, and no tangent spaces: a metric space is **purely topological/analytic**: it tells us how far apart two points are, globally, with no notion of infinitesimal structure.

---

### 1.2] Riemannian metric: infinitesimal inner product

On a smooth manifold, a Riemannian metric assigns to each point $$p \in M$$ an inner product  
$$
g_p : T_p M \times T_p M \to \mathbb{R}
$$  
that varies smoothly with $$p$$.

So instead of distances between points, we get:
- lengths of tangent vectors  
- angles  
- infinitesimal geometry  

This is fundamentally **local** (infinitesimal) data.

---

### 1.3] The key bridge: Riemannian metric ⇒ metric space

A Riemannian metric *induces* a metric space structure.

Given $$g$$, define the length of a curve 
$$
L(\gamma) = \int \sqrt{g_{\gamma(t)}(\dot{\gamma}(t), \dot{\gamma}(t))} \, dt
$$

Then define distance:
$$
d(p,q) = \inf_{\gamma: p \to q} L(\gamma)
$$

This turns the manifold into a metric space: consequently, we know that **Riemannian metric always induces a metric space**

However, it'snot the other way around: A general metric space **does not come from a Riemannian metric**. Why not?

Because a metric space:
- has no notion of tangent space  
- no smooth structure  
- no way to define inner products locally  

Even worse:
- many metric spaces are highly singular (fractals, graphs, etc.)
- even smooth manifolds can carry metrics that are not induced by any Riemannian structure (e.g. Finsler metrics)

Here's a nice conceptual hierarchy:
$$
\text{Riemannian geometry}
\;\subset\;
\text{length spaces}
\;\subset\;
\text{metric spaces}
$$

While Riemannian metric is **infinitesimal inner product**, metric space is **global distance function**. A Riemannian metric is like specifying a **quadratic form at each point**, and the metric space distance is what we get after **integrating that local data globally**. In a word, a Riemannian metric is a *differential (local) object*,  while a metric space metric is an *integrated (global) object*.

---

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

Here we just treat matrices and vectors. This is simple but not geometrically natural, and it's not invariant under coordinate changes. Note that to make this Euclidean, we actually applied the Frobenius inner product $$<A, B>_X = Tr(A^\top B)$$. 

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
