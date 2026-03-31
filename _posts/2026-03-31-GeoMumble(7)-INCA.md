---
layout: post
title: 'The Dance of Space: Geom/Topo/Dynam Mumble(7): Independent Nonlinear Component Analysis (in progress)' 
date: 2026-03-31 11:53:49
description: Summary and Reflection on INCA 
series: The Dance of Space
tags:
    - "Geometry Concepts/Tools"
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

Today we will talk about a nonlinear dimensionality reduction method, which is a *Geometric and Information-Theoretic Generalization of PCA*. I recently read this paper xxx and really appreciate the authors' way of thought. 


## 0] Big Picture

This paper introduces a **nonlinear generalization of Principal Component Analysis (PCA)** that:

* Produces **statistically independent components** (not just uncorrelated)
* Works for **nonlinear, non-Gaussian data**
* **Reduces exactly to PCA** in the linear Gaussian case
* Requires **no user-specified regularization parameters**

The method is called **Independent Nonlinear Component Analysis (INCA)**, and the core idea, in one sentence, is 

> to transform data into a Gaussian using **optimal transport**, and then find the most informative directions using **entropy instead of variance**, and map back to obtain nonlinear components.

In alignment with the paper, I will use their notation system. 

---

## 1] Specific Pipeline

### 1.1] Gaussianization via Optimal Transport

We start with:
$$
y \sim f(y)
$$

Construct a map:
$$
x = T(y)
$$

such that:

* $$x \sim \mathcal{N}(0, I)$$
* $$T$$ is the **Brenier map** (gradient of a convex function)

This satisfies the change-of-variables formula:
$$
f(y) = \phi(T(y)) \left|\det \frac{\partial T(y)}{\partial y}\right|
$$

Something nice of using the Brenier map is the following key properties:

* **Unique** (under mild conditions)
* **Monotone** (gradient of convex function)
* Minimizes transport cost:
  $$
  \int |y - T(y)|^2 f(y) dy
  $$

In some sense, this is the *most natural* way to transform our data into a Gaussian.

---

### 1.2] Independence is Built In

The standard Gaussian factorizes:
$$
\phi(x) = \prod_i \phi(x_i)
$$

So:

* The components $$x_i$$ are **independent**

Here comes the key idea:

> Instead of *finding* independent components, we **construct a representation where independence holds automatically**. 

Then potentially using the inverse map, which position us starting from these independent Gaussian variables, we are theoretically guaranteed that we ensure independence. I will cover this in the discussion sections. 

---

### 1.3] Rotation in Latent Space

We then define components:

$$
z_j = u_j^\top x
$$

where $${u_j}$$ are orthonormal directions in the $$x$$ space. By the way, you could also contend that it's the same space since both $$y, x$$ live in $$\mathbb{R}^d$$.

Here you might wonder, since $$x \sim \mathcal{N}(0, I)$$ is isotropic, aren’t all directions equivalent?

The answer is Yes, in $$x$$-space.

However, even though the Gaussian is rotationally invariant, we do **not stay in $$x$$-space**. We interpret directions via:

$$
y = T^{-1}(x)
$$

So each direction $$u$$ corresponds to:
$$
y(t) = T^{-1}(t u)
$$

which is a **curve in data space**.

Does this remind you the coordinate charts in topological/smooth manifolds? 

Consequently, we could have the following interpretation:

* $$x$$-space: flat, isotropic Gaussian
* $$y$$-space: nonlinear, curved data manifold
* $$T^{-1}$$: **nonlinear deformation**

Even though directions are identical in $$x$$, after mapping back, different directions produce **different nonlinear structures in $$y$$**

> The Gaussian is rotationally invariant, but the inverse transport map is not.

So:

* Rotation in latent space = selecting meaningful nonlinear directions
* These directions correspond to:

  > **independent, information-rich features of the data**

---

### 1.4] What Breaks the Symmetry?

An immediately following question is that then the data structure might clearly not be isotropic, so how are we sure we could handle that situation? Well, 

it is the Jacobian

$$
J(y) = \frac{\partial T(y)}{\partial y}
$$

which captures how space is distorted.

Some directions expand a lot, while others compress a lot. And this already hints at the fundamental reason why we employ rotation:

> Rotation selects directions that correspond to **meaningful nonlinear features in the original data**

---

### 1.5] Replace Variance with Entropy

In PCA, we 

$$
\text{maximize } \mathrm{Var}(u^\top y)
$$

But here in INCA:

The authors claim that variance is not meaningful under nonlinear transformations.

Instead, they chose to use **entropy**:

$$
H[f] = -\int f(y) \ln f(y) dy
$$

---

### 1.6] Key Result: Entropy Decomposition

Crucially, the authors prove that

$$
H[f] = \sum_j H_{u_j}
$$

where:
$$
H_u = \frac{1}{2}\ln(2\pi e) + u^\top \bar J u
$$

and:
$$
\bar J = -\mathbb{E}[\ln J(y)]
$$

Consequently, similar to PCA, this amounts to solving via eigenvectors:

$$
\max_{u_1,\dots,u_k} \sum_{j=1}^k H_{u_j}
$$

which means that the optimal directions are the **top eigenvectors of $\bar J$**.

Interested readers please feel free to refer to the original paper for more materials on this entropy decomposition. I shall not reproduce here an already detailed proof. 

---

### 1.7] Analogy to PCA

| PCA                        | INCA                          |
| -------------------------- | ----------------------------- |
| Covariance matrix $$\Sigma$$ | $$\bar J = -\mathbb{E}[\ln J]$$ |
| Variance                   | Entropy                       |
| Linear projection          | Nonlinear mapping             |
| Uncorrelated components    | Independent components        |
| Euclidean geometry         | Transport-induced geometry    |

---

### 1.8] Nonlinear Representation

With the Brenier map calculated and eigendirections found, we could locate the following easily:

* 1) Low-dimensional nonlinear/curved reconstruction:

$$
y^* = T^{-1}\left(\sum_{j=1}^k u_j x_j \right)
$$

* 2) Low-dimensional projection/coordinates:

$$
x_j = u_j^\top T(y)
$$

---

## 2] Discussions and Reflections

### 2.1] Intuition

1. Warp data → Gaussian
2. Rotate → choose informative directions
3. Warp back → get curved manifold

---

### 2.2] Why Variance Fails

The authors gave a simple example:

* “L-shaped” distribution → large variance
* “C-shaped” distribution → lower variance but richer structure

PCA prefers straight directions, while INCA prefers directions with **larger entropy (information content)**

---

### 2.3] Interpretation of $$\bar J$$

They show:

* $$\ln J$$ behaves like a **logarithmic strain tensor**
* $$u^\top \bar J u$$ measures:

  > average **log-volume expansion along direction $$u$$**

Conseuqently, INCA finds directions where the data undergoes the **largest geometric deformation**

---

### 2.4] Relationship to ICA and PCA

| ICA                             | INCA               |
| ------------------------------- | ------------------ |
| Linear mixing assumption        | Fully nonlinear    |
| Approximate independence        | Exact independence |
| Uses kurtosis / non-Gaussianity | Uses entropy       |
| No natural ranking              | Ranked by entropy  |
| Fails for Gaussian              | Reduces to PCA     |

and relationship to PCA:

When data is Gaussian:

* $$T$$ is linear
* $$J$$ is constant
* $$\bar J$$ reduces to covariance structure

So INCA = PCA

---

### 2.5] Computational Procedure

Practically, there are a sequence of steps to fulfil, and many numerical problems might occur. 

1. Estimate density $$f(y)$$
2. Compute Brenier map $$T$$
3. Compute Jacobian $$J(y)$$
4. Compute:
   $$
   \bar J = -\mathbb{E}[\ln J(y)]
   $$
5. Eigen-decompose $$\bar J$$
6. Extract top components

As you might know, computing $$T$$ is expensive in high dimensions. Consequently, the authors propose a **hybrid approach**: apply PCA for small components and INCA for the dominant ones. 

---

### 2.6] Why This Matters (Especially for Neural Data)

We've been constantly saying that PCA captures covariance structure. To be more precise, this only covers the second-order statistcs. 

Here, INCA captures **full statistical dependence**. In other words,

* PCA manifold = second-order structure
* INCA manifold = **true latent structure**

---

### 2.7] One-Line Summary

> INCA = PCA performed in a geometrically warped space where independence replaces uncorrelatedness and entropy replaces variance.

---

## 3] Potential Connections

* 1) **Riemannian geometry**: $$T^{-1}$$ induces a pullback metric
* 2) **Information geometry**: entropy decomposition
* 3) **Optimal transport**: provides canonical nonlinear coordinates
* 4) **Latent variable models**: gives identifiable nonlinear factors

We might want to go further to explain: 

1. How to understanding the **Brenier map as a geometric object**
2. Interpreting $$\bar J$$ as a **curvature / strain tensor**
3. How to potentially connect/apply this to **neural manifolds and latent dynamics**

