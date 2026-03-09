---
layout: post
title: 'The Flow of Time: Prob/Measure/Transport/Generative Mumble(X): Entropy Convexity and Ricci Curvature (in progress)'
date: 2026-03-09 00:16:01
description: Displacement Convextiy, Ricci Curvature 
# thumbnail: /assets/img/blogs/spd.png
tags: 
    - "Probability"
    - "Optimal Transport"
categories: 
    - "Brain-Computer Interface"
featured: false
related_posts: true
related_publications: true
citation: true
math: true
toc:
  beginning: true
  sidebar: left
---

This blog post is dedicated to a specific topic that unites entropy, displacement convexity, and Ricci curvature. In my personal opinion, this is one of the deepest and aesthetically appealing results of modern optimal transport theory. I quickly alluded to it in the 4th blog post in this series, but here I will expand from there a little more.

In summary, what I will elaborate is where optimal transport becomes **shockingly close to Riemannian geometry**. The key idea is due to Felix Otto: the space of probability densities with the 2-Wasserstein metric behaves like an **infinite-dimensional Riemannian manifold**, and the **entropy Hessian in this manifold contains the Ricci tensor of the base manifold**.

In case we get confused, in this blog I will specifically add subscripts to distinguish differentiation on $$t$$ vs on $$x$$. 

---

### 1] The “manifold” of probability densities

Let

$$
(M,g)
$$

be a smooth $$d$$-dimensional Riemannian manifold with Riemannian metric $$g$$.

Let $$dx$$ denote the Riemannian volume measure on $$M$$.

We consider the space

$$
\mathcal P_2(M)
= \left\{
\rho(x) \ge 0 :
\int_M \rho(x)\,dx = 1,
\quad
\int_M |x|^2 \rho(x)\,dx < \infty
\right\}.
$$

Each element of $$\mathcal P_2(M)$$ is a **probability density function**

$$
\rho : M \rightarrow \mathbb{R}_{\ge 0}.
$$

We treat \(\mathcal P_2(M)\) as an **infinite-dimensional manifold**.

---

### 2] Curves of probability densities

Consider a time-dependent probability density

$$
\rho_t(x), \quad
t \in [0,1],\; x \in M.
$$

Thus we have a map (probability flow): 

$$
\rho : [0,1] \times M \rightarrow \mathbb{R}_{\ge 0},
\quad
(t,x) \mapsto \rho_t(x).
$$

Assume mass is conserved over time:

$$
\int_M \rho_t(x)\,dx = 1
\quad
\forall t.
$$

Mass conservation implies the **continuity equation**

$$
\partial_t \rho_t(x) + \nabla \cdot \left(\rho_t(x) v_t(x) \right) = 0.
$$

Here

- $$v_t(x) \in T_xM$$ is a velocity vector field
- $$v : [0,1] \times M \to TM$$

---

### 3] Tangent vectors in probability space

At a fixed density $$\rho_t(x)$$, the time derivative

$$
\dot{\rho}_t(x)
= \partial_t \rho_t(x)
$$

acts as a **tangent vector** in the space of probability measures.

From the continuity equation, we know that

$$
\dot{\rho}_t(x)
= - \nabla \cdot \left( \rho_t(x) v_t(x) \right).
$$

Optimal transport theory shows that along **Wasserstein geodesics**, the velocity field is always a gradient field (we covered this in this previous blog post YY):

$$
v_t(x) = \nabla \phi_t(x)
$$

for some scalar potential

$$
\phi_t : M \to \mathbb{R}.
$$

Thus tangent vectors take the form

$$
\dot{\rho}_t(x)
= - \nabla \cdot \left( \rho_t(x)\nabla \phi_t(x) \right).
$$

Therefore

- tangent vectors acquires a corresponding scalar potentials $$ \phi_t(x) $$

---

### 4] Otto’s Riemannian metric

Felix Otto introduced a __Riemannian metric__ on $$\mathcal P_2(M)$$.

Suppose we have two tangent vectors:

$$
\dot{\rho}^{(1)}(x)
= - \nabla \cdot \left( \rho(x)\nabla \phi_1(x) \right)
$$

and

$$
\dot{\rho}^{(2)}(x) 
= - \nabla \cdot \left( \rho(x)\nabla \phi_2(x) \right).
$$

The **Riemannian inner product** is defined as

$$ 
\langle
\dot{\rho}^{(1)},
\dot{\rho}^{(2)}
\rangle_{\rho}
= \int_M
g_x
\big(
\nabla \phi_1(x),
\nabla \phi_2(x)
\big)
\rho(x)\,dx.
$$

Here

- $$g_x(\cdot,\cdot)$$ is the Riemannian metric at $$x$$.

How to interpret this? Notice that the squared norm of a tangent vector becomes

$$
\| \dot{\rho} \|_\rho^2 = \int_M | \nabla \phi(x) |_g^2 \rho(x)\,dx.
$$

This equals the **kinetic energy of moving mass**.

Indeed, the Benamou–Brenier formula states

$$
W_2^2(\rho_0,\rho_1)
= \inf_{\rho_t,v_t}
\int_0^1
\int_M
|v_t(x)|^2
\rho_t(x)
\,dx\,dt.
$$

---

### 5] Wasserstein geodesic equations

Under Otto’s metric, geodesics satisfy the continuity equation:

$$
\partial_t \rho_t(x) + \nabla \cdot \left( \rho_t(x)\nabla \phi_t(x) \right) = 0
$$

By the Hamilton–Jacobi equation

$$
\partial_t \phi_t(x) + \frac12 | \nabla \phi_t(x) |^2 = 0.
$$

Solving these equations produces the **optimal transport interpolation** (optimal transport flow) between

$$
\rho_0(x)
\quad \text{and} \quad
\rho_1(x).
$$

Just as a sanity check, we should now be comfortable for connecting the static and dynamic view of optimal transport, and easily write out the optimal flow $$\rho_t(x)$$ as the law of the straight flow once we know the optimal map:

$$
\rho_t(x) = \text{Law}((1-t)x + t\Phi^{opt}(x))
$$

or, in other words

$$
\rho_t = ((1-t) + t\Phi^{opt})_{\#}
$$

---

### 6] Entropy Functional and Variations

Define the **entropy functional** as (not whether to take negative sign is just a choice)

$$
\mathrm{H}(\rho) = \int_M \rho(x)\log\rho(x) \,dx.
$$

This is a functional

$$
\mathrm{H} :
\mathcal P_2(M)
\rightarrow
\mathbb{R}.
$$

Then let's compute the first variation:

$$
\frac{\delta \mathrm{H}}{\delta \rho}(x) = 1+\log\rho(x).
$$

The **Wasserstein gradient flow** equation for entropy then becomes

$$
\partial_t \rho_t(x)
= \nabla \cdot
\left(
\rho_t(x)
\nabla
\frac{\delta \mathrm{H}}{\delta\rho}(\rho_t(x))
\right).
$$

Substitute

$$
\frac{\delta \mathrm{H}}{\delta \rho}(x)= 1+\log\rho_t(x).
$$

Then

$$
\nabla (1+\log\rho_t(x))= \frac{\nabla\rho_t(x)}{\rho_t(x)}.
$$

Thus

$$
\partial_t\rho_t(x)
= \nabla \cdot \left( \nabla \rho_t(x) \right).
$$

Therefore

$$
\partial_t\rho_t(x)= \Delta_x \rho_t(x).
$$

Very surprisingly:

**The heat equation is the gradient flow of entropy in Wasserstein space.**

This result is known as the **Jordan–Kinderlehrer–Otto (JKO) scheme**.

---

### 7] Second Derivative of Entropy along a Geodesic

Let

$$
\{\rho_t(x)\}_{t \, \in \, [0, 1]}
$$

be a Wasserstein geodesic generated by the potential

$$
\phi_t(x).
$$

Compute

$$
\frac{d^2}{dt^2}
\mathrm{H}(\rho_t).
$$

After a long calculation using the continuity equation and the **Bochner identity**, we would obtain

$$
\frac{d^2}{dt^2} \mathrm{H}(\rho_t) =
\int_M
\Big(
|\nabla^2 \phi_t(x)|^2
+
\mathrm{Ric}_x
(
\nabla \phi_t(x),
\nabla \phi_t(x)
)
\Big)
\rho_t(x)
\,dx.
$$

Here

- \( \nabla^2\phi_t(x) \) is the **Hessian** of \( \phi_t \)
- \( \mathrm{Ric}_x(\cdot,\cdot) \) is the **Ricci curvature tensor** at \(x\).

How to interpret this formula then? Well, notice that the Hessian splits into two terms:

* Positive Hessian term $$ |\nabla^2\phi_t(x)|^2 \ge 0$$
* Curvature term $$ \mathrm{Ric}_x ( \nabla\phi_t(x), \nabla\phi_t(x))$$, which depends on the geometry of the manifold.

---

### 8] Convexity from Ricci Curvature

Suppose the manifold satisfies

$$
\mathrm{Ric}_x(v,v)
\ge
K|v|^2
\quad
\text{for all } v\in T_xM.
$$

Then

$$
\mathrm{Ric}_x
(
\nabla\phi_t(x),
\nabla\phi_t(x)
)
\ge
K
|
\nabla\phi_t(x)
|^2.
$$

Thus

$$
\frac{d^2}{dt^2}
\mathrm{H}(\rho_t)
\ge
K
\int_M
|
\nabla\phi_t(x)
|^2
\rho_t(x)
\,dx.
$$

But along a Wasserstein geodesic,

$$
\int_M | \nabla\phi_t(x) |^2 \rho_t(x) \,dx = W_2^2(\rho_0,\rho_1)
$$

Therefore

$$
\mathrm{H}(\rho_t)
\text{ is } K\text{-convex along Wasserstein geodesics}.
$$

---

### 9] Geometric Meaning

This shows that

$$
\text{Hessian}_{W_2}
\mathrm{H}
\sim
\mathrm{Ric}.
$$

So the **Ricci curvature of the manifold controls the convexity of entropy in Wasserstein space**. This insight leads to the **Lott–Sturm–Villani theory**, which defines curvature bounds using optimal transport.

Or put it differently, the Ricci curvature ≥ $$K$$ if and only if

> **The entropy functional is \(K\)-convex along Wasserstein geodesics in the space of probability measures.**