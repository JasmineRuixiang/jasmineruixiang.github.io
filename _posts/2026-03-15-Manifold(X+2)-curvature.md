---
layout: post
title: "Manifold and Riemannian Geometry (X+2): Riemannian Curvature, Sectional Curvature, and Ricci Curvature"
date: 2026-03-15 23:09:29
description: Exploration of curvature
series: Manifold and Riemannian Geometry
# thumbnail: /assets/img/blogs/spd.png
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


### 1] Riemannian Curvature Tensor
The key to understanding the **Riemann curvature tensor** is to read the formula as a statement about **how covariant derivatives fail to commute** when we move in two different directions on a manifold.

The definition of Riemannian curvature is the correspondence:

$$
R(X,Y)Z = \nabla_Y\nabla_X Z - \nabla_X\nabla_Y Z + \nabla_{[X, Y]}Z .
$$

This expression measures **how much the vector field $$Z$$ changes when we transport it around an infinitesimal parallelogram spanned by $$X$$ and $$Y$$**.

Let me elaborate below. 

---

#### 1.1] What Would Happen in Flat Space

In ordinary Euclidean space with the standard derivative,

$$
\nabla_X \nabla_Y Z = \nabla_Y \nabla_X Z .
$$

So the difference would be **zero**. This reflects the fact that in flat space, the order of taking directional derivatives does not matter (a simple rule we knew from basic Calculus). Consequently, curvature should measure:

> **How much the order of taking derivatives matters.**

---

#### 1.2] The Naive Commutator is Not Tensorial

A natural first guess might be

$$
\nabla_X\nabla_Y Z - \nabla_Y\nabla_X Z
$$

But this expression alone **does not behave tensorially**.  
The issue is that the vector fields $$X$$ and $$Y$$ themselves may not commute.

Recall their Lie bracket

$$
[X,Y]
$$

which measures the difference between flowing along $$X$$ then $$Y$$ versus $$Y$$ then $$X$$.

So part of the difference above is simply caused by **the coordinate grid twisting**, not curvature.

---

#### 1.3] Subtract the Coordinate Artifact

To isolate the **true geometric effect**, we subtract the derivative along the Lie bracket direction:

$$
\nabla_{[X,Y]}Z
$$

This removes the artifact caused by the noncommutativity of the flows.

So the curvature tensor becomes

$$
R(X,Y)Z
= \nabla_Y\nabla_X Z - \nabla_X\nabla_Y Z + \nabla_{[X,Y]}Z .
$$

Now the result depends **only on the values of \(X,Y,Z\) at the point**, making it a tensor.

---

#### 1.4] Geometric Picture: Transport around a Tiny Loop

Imagine the following process:

1. Start at a point $$p$$.
2. Move a tiny distance along $$X$$.
3. Then along $$Y$$.
4. Then back along $$-X$$.
5. Then back along $$-Y$$.

This forms an **infinitesimal parallelogram**. Now parallel transport the vector $$Z$$ around this loop.

In **flat space**, the vector returns unchanged. However, on a **curved manifold**, the vector comes back slightly rotated. The curvature tensor measures exactly that change. More precisely, $$ R(X,Y)Z $$ is the **infinitesimal failure of parallel transport around the loop to return $$Z$$**.

---

#### 1.5] What Kind of Object It Is

At each point $$p$$:

$$
R_p : T_pM \times T_pM \times T_pM \to T_pM
$$

So it takes

- two directions defining a **2-plane** ($$X,Y$$)
- a vector $$Z$$

and returns **how $$Z$$ twists when transported around that plane**. Thus curvature is fundamentally a **property of planes**, not just directions. Spoiler alert: This is why sectional curvature depends on a **2-dimensional plane** (see below).

We could summarize the meaning of curvature as:

$$
\textbf{Curvature = failure of covariant derivatives to commute}
$$

after correcting for the fact that vector fields themselves might not commute.

---

#### 1.6] One More Intuition

Think of $$ \nabla_X Z $$ as:

> the rate of change of $$Z$$ when moving in direction $$X$$.

Then

- $$ \nabla_Y\nabla_X Z $$
  means: change $$Z$$ along $$X$$, then see how that result changes along $$Y$$.

- $$ \nabla_X\nabla_Y Z $$
  means: reverse the order.

If the space is curved, **these two operations do not give the same result**, and the difference is exactly what the curvature tensor measures.

---

#### 1.7] Why This Definition is Fundamental

This definition is powerful because **all classical curvature quantities come from it**. From $$R$$ we derive

- **Sectional curvature**
- **Ricci curvature**
- **Scalar curvature**

Everything about curvature in Riemannian geometry originates from this operator.

---

