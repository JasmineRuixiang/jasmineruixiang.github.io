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
related_posts: trues
related_publications: true
citation: true
math: true
toc:
  beginning: true
  sidebar: left
---


## 1] Riemannian Curvature Tensor
The key to understanding the **Riemann curvature tensor** is to read the formula as a statement about **how covariant derivatives fail to commute** when we move in two different directions on a manifold.

The definition of Riemannian curvature is the correspondence:

$$
R(X,Y)Z = \nabla_Y\nabla_X Z - \nabla_X\nabla_Y Z + \nabla_{[X, Y]}Z .
$$

This expression measures **how much the vector field $$Z$$ changes when we transport it around an infinitesimal parallelogram spanned by $$X$$ and $$Y$$**.

Let me elaborate below. 

---

### 1.1] What Would Happen in Flat Space

In ordinary Euclidean space with the standard derivative,

$$
\nabla_X \nabla_Y Z = \nabla_Y \nabla_X Z .
$$

So the difference would be **zero**. This reflects the fact that in flat space, the order of taking directional derivatives does not matter (a simple rule we knew from basic Calculus). Consequently, curvature should measure:

> **How much the order of taking derivatives matters.**

---

### 1.2] The Naive Commutator is Not Tensorial

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

### 1.3] Subtract the Coordinate Artifact

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

Actually, to require that the operator $$R(X, Y)$$ is linear:

* $$R(X, Y)fZ = fR(X, Y)Z$$

we need to include the term $$\nabla_{[X, Y]}Z$$

---

### 1.4] Geometric Picture: Transport around a Tiny Loop

Imagine the following process:

1. Start at a point $$p$$.
2. Move a tiny distance along $$X$$.
3. Then along $$Y$$.
4. Then back along $$-X$$.
5. Then back along $$-Y$$.

This forms an **infinitesimal parallelogram**. Now parallel transport the vector $$Z$$ around this loop.

In **flat space**, the vector returns unchanged. However, on a **curved manifold**, the vector comes back slightly rotated. The curvature tensor measures exactly that change. More precisely, $$ R(X,Y)Z $$ is the **infinitesimal failure of parallel transport around the loop to return $$Z$$**.

---

### 1.5] What Kind of Object It Is

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

### 1.6] One More Intuition

Think of $$ \nabla_X Z $$ as:

> the rate of change of $$Z$$ when moving in direction $$X$$.

Then

- $$ \nabla_Y\nabla_X Z $$
  means: change $$Z$$ along $$X$$, then see how that result changes along $$Y$$.

- $$ \nabla_X\nabla_Y Z $$
  means: reverse the order.

If the space is curved, **these two operations do not give the same result**, and the difference is exactly what the curvature tensor measures.

---

### 1.7] Why This Definition is Fundamental

This definition is powerful because **all classical curvature quantities come from it**. From $$R$$ we derive

- **Sectional curvature**
- **Ricci curvature**
- **Scalar curvature**

Everything about curvature in Riemannian geometry originates from this operator.

---

### 1.8] Expression in Christoffel Symbols

---

### 1.9] Differences between Riemannian Curvature and Covariant Derivative

---

## 2] Sectional Curvature 


### 2.1] Definition

We will use the following notation:

$$
|x \wedge y|
$$

represents

$$
\sqrt{|x|^2|y|^2 - \langle x, y \rangle ^2}
$$

which is the are of a two-dimensional parallelogram spanned by $$x, y \in V$$. Here $$V$$ is a vector space. 

We define sectional curvature as the following:


What's also very important about sectional curvature is that once we know $$K(\sigma), \forall \sigma$$ then we know $$R$$ completely. 

---

### 2.2] Meaning of the Numerator

Recall the geometric meaning of $$R(x, y)z$$: it measures how much the vector $$z$$ changes when parallel transported around a tiny parallelogram spanned by $$x$$ and $$y$$. Now we specialize: 

* $$z = y$$
* Measure the component in direction $$x$$

So $$ \langle R(x,y)y,x \rangle $$

measures how much curvature bends the vector $$y$$ toward $$x$$ when moving around the $$x-y$$ parallelogram. It captures the curvature restricted to that plane.

---

### 2.3] Why Divide by $$|x \wedge y|^2$$
Dividing by it removes dependence on the lengths of $$x,y$$, which is the scaling of the parallelogram. This ensures $$K(x,y)$$ depends only on the plane, not the particular vectors chosen.

Consequently, really it is

$$
K(\sigma) = K(x, y), \forall x, y \in \sigma
$$

---

### 2.4] The Geometric Picture
Imagine a tiny geodesic parallelogram spanned by $$x,y$$. Curvature causes the parallel transport loop to fail to close perfectly. The Riemann tensor measures this failure. Sectional curvature extracts the scalar curvature intensity of that failure __inside the chosen plane__. 

Sectional curvature answers the following question: If I walk in two infinitesimal directions $$x$$ and $$y$$, how much does the manifold curve within that plane?

Positive curvatur indicates that geodesics bend toward each other. Negative curvature indicates geodesics bend away from each other, while zero curvature says that geodesics stay parallel.

---

## 3] Ricci Curvature

### 3.1] Definition

Let $$M$$ be an $$n$$-dimensional Riemannian manifold and let  
$$\{z_1,\dots,z_n\}$$ be an **orthonormal basis** of $$T_pM$$. A standard formula for Ricci curvature in direction $$x$$ is

$$
\mathrm{Ric}_p(x) = \frac{1}{n-1}\sum_{i=2}^{n} \langle R(x,z_i)z_i , x\rangle
$$

where $$\{x,z_2,\dots,z_n\}$$ is an orthonormal basis.

Conceptually this is (note because of orthonormal the denominator of sectional curvature is just 1):

$$
\text{Ric}_p(x) = \frac{1}{n-1} \sum_{i=2}^{n} K(x,z_i)
$$

Consequently, **Ricci curvature = the average sectional curvature of all planes containing $$x$$.**

---

### 3.2] Geometric intuition

Fix a direction $$x$$. There are many 2-planes that contain $$x$$:

$$
\text{span}(x,z_2),\;
\text{span}(x,z_3),\;
\dots,\;
\text{span}(x,z_n)
$$

Each of those planes has its own sectional curvature. Ricci curvature takes the **average curvature of all those planes**.

What then does Ricci curvature describe? Well, it describes **volume distortion along geodesics**.

Imagine shooting out many geodesics from $$p$$ with initial velocity $$x$$.

- If Ricci $$>0 \rightarrow $$ nearby geodesics **tend to converge**
- If Ricci $$<0 \rightarrow $$ → they **spread apart**

(we could perhaps imange geodesics on a sphere through a fixed point converge based on the positive curvature) More precisely:

- positive Ricci $$\rightarrow$$ small geodesic balls have **smaller volume than Euclidean**
- negative Ricci $$\rightarrow$$ **larger volume**

This is why Ricci curvature appears in:

- comparison geometry
- Ricci flow
- Einstein’s equations.

---

### 3.3] Why Ricci Curvature is Important

Ricci curvature is exactly the part of curvature that affects **volume growth**.

For a small geodesic ball of radius $$r$$,

$$
\mathrm{Vol}(B_r(p))
= \omega_n r^n \left(1 - \frac{S(p)}{6(n+2)}r^2 + O(r^3) \right)
$$

So scalar curvature tells how the **volume deviates from Euclidean space**.

---

## 4] Scalar curvature: averaging Ricci curvature

Scalar curvature is defined as

$$
K(p)=\frac{1}{n}\sum_j \mathrm{Ric}_p(z_j)
$$

It is simply the **average of Ricci curvature over all directions**.

Equivalently,

$$
S(p) = \sum_{i<j} 2K(z_i,z_j)
$$

So scalar curvature averages the sectional curvature of **all 2-planes at $$p$$**.

---

## 5] The Hierarchy of Curvature Information

We can think of the curvature tensors as progressively losing directional information.

Riemann tensor $$ R(X,Y)Z $$ characterizes the full information including curvature of every plane and direction.

Sectional curvature $$K(\sigma)$$ is slicing out Riemannian curvature tensor by focusing only on a 2-dim plane.  

Ricci curvature $$ \mathrm{Ric}_p(x) $$ is a partial average of the curvature of planes containing a specific direction $$x$$. 

Scalar curvature $$ S(p) $$ is the average curvature over **all directions**. 





