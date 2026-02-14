---
layout: post
title: "Manifold and Riemannian Geometry (X): Connections, Parallel Transport, and Geodesics (in progress)"
date: 2026-02-13 16:36:23
description: Exploration of Connection
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

This is episode X on the Manifold and Riemannian Geometry series. Today we will be exploring more on tangent vectors, vector fields, another key concept about how to relate tangent spaces together, and many other derivative notions crucial for the development of Riemannian Geometry. 


## 0] Intuition and Introduction: Directional Derivatives

Connection: Bridging derivatives from $$\mathbb{R}^n$$ to curved manifolds. The concept of a connection is the necessary tool that allows us to perform differential calculus on curved spaces (manifolds), such as the surface of a sphere. It generalizes the familiar idea of the directional derivative from flat Euclidean space ($$\mathbb{R}^n$$).

Directional Derivatives in Euclidean Space ($$\mathbb{R}^3$$)
In $$\mathbb{R}^3$$ with Cartesian coordinates $$(x, y, z)$$, the directional derivative provides a simple way to measure change. The basis vectors $$\left\{ \mathbf{i}, \mathbf{j}, \mathbf{k} \right\}$$ (or the equivalent operators $$\left\{ \frac{\partial}{\partial x}, \frac{\partial}{\partial y}, \frac{\partial}{\partial z} \right\}$$) are constant, allowing us to define derivatives simply as component-wise partial derivatives.
Let $$p=(1, 2, 0)$$ be a point, $$X=(y, -x, 3x)$$ be the direction vector field, and $$V=(xz, y^2, -2x)$$ be a vector field.

There are two things we could implement:

### 1) Derivative of a Scalar Function ($$D_X f$$).
This is the directional derivative of a smooth scalar function $$f$$ in the direction $$X$$.

For example, for $$f(x,y,z)=xy^2+z$$ at $$p$$:
By definition, $$D_X f = \nabla f \cdot X$$, so 
$$D_X f(p) = \langle 4, 4, 1 \rangle \cdot \langle 2, -1, 3 \rangle = \mathbf{7}$$

Point Derivation (Geometry)
$$X[f] = X^i \frac{\partial f}{\partial x^i}$$
$$Xf = (y^3 - 2x^2y + 3x)$$

This confirms that in differential geometry, a tangent vector $$X$$ is rigorously defined as a point derivation: an operator that mimics the directional derivative by satisfying the Leibniz rule (please see my [previous blog](https://jasmineruixiang.github.io/blog/2025/Manifold(2)/) on the definition and interpretation of tangent vectors).

### 2) Derivative of a Vector Field ($$D_X V$$)
This derivative measures how the vector field $$V$$ changes as we move in the direction $$X$$. In $$\mathbb{R}^3$$, this is calculated by taking the directional derivative of each component of $$V$$.

Using the vector fields $$X$$ and $$V$$, we notice that the $$k$$-th component of the resulting vector $$D_X V$$ is $$(D_X V)^k = \sum_{i} X^i \frac{\partial V^k}{\partial x^i}$$.

Component 1 (i.e., $$k=1$$): $$(D_X V)^1 = yz + 3x^2$$
Component 2 (i.e., $$k=2$$): $$(D_X V)^2 = -2xy$$
Component 3 (i.e., $$k=3$$): $$(D_X V)^3 = -2y$$

Evaluating at $$p=(1, 2, 0)$$ gives:

$$D_X V(p) = \langle 3, -4, -4 \rangle$$

---

## 1] Why Do We Need a Connection?
In Euclidean space $$\mathbb{R}^{n}$$, if we have a vector field
$$Y(x) = (Y^1(x), \cdots, Y^n(x))$$, and we could easily differentiate component-wise: $$\frac{\partial Y^i}{\partial x^j}$$. This works because

- All tangent spaces are the same vector space.
- We can subtract vectors at different points.
- We can differentiate vector fields componentwise.

But on a manifold:

- A vector field satisfies  
  $$V(p) \in T_pM$$
  $$V(q) \in T_qM$$
- These live in _different_ vector spaces.
- Subtracting them directly makes no sense.

So we need a rule that tells us:

> How does a vector field $$Y$$ change as we move in a given direction given by $$X$$?

This rule is an **affine connection**. And we __choose__ such a rule. 

---

## 2] Definition of an Affine Connection

A connection is an operator operator $$\nabla: C^{\infty}(M) \times C^{\infty}(M) \to C^{\infty}(M)$$ that maps two vector fields, $$X$$ and $$Y$$, to a new vector field $$\nabla_X Y$$.

We could think of it as:

> The derivative of $$Y$$ in the direction $$X$$.

This specified choice needs to satisfy the following three rules:

* Linearity in $$X$$
$$
\nabla_{fX + gZ} Y = f\nabla_X Y + g\nabla_Z Y
$$

* Linearity in $$Y$$
$$
\nabla_X (Y + Z) = \nabla_X Y + \nabla_X Z
$$

* __Leibniz Rule__
$$
\nabla_X (fY) = X(f)Y + f\nabla_X Y
$$

where $X(f) = Xf$ is the directional derivative of $$f$$. This last property is crucial, as it mirrors ordinary differentiation (notice that we also require the definition of derivation when in tangent vector to also satisfy the Leibniz rule).

Also note that this definition does **not** require a metric.

---

## 3] Local Coordinate Formula and Christoffel Symbols

Choose coordinates $$(x^1, \dots, x^n)$$ first. 

Let  
$$
X_i = \frac{\partial}{\partial x^i}
$$

Note that for the following I might interchange these two symbols, but they are always equivalent. 

Define:

$$
\nabla_{X_i} X_j = \Gamma^k_{ij} X_k
$$

(Or, just in another format, to be crystal clear)

$$
\nabla_{\frac{\partial}{\partial x_i}}\frac{\partial}{\partial x_j} = \sum_{k = 1}^n \Gamma^k_{ij} \frac{\partial}{\partial x_k}
$$

We use above the Einstein summation notation as convention (to simplify notations). In below, whenever summation might be obvious I will drop it. Here the $\Gamma^k_{ij}$ are called the ``Christoffel symbols``, which tell us how the coordinate basis vectors change as we move.

Now use the above notation, say for two vector fields:

$$
X = X^i \frac{\partial}{\partial x^i}
$$
$$
Y = Y^j \frac{\partial}{\partial x^j}
$$

We could obtain:

$$
\boxed{
\nabla_X Y
= \left(\underbrace{
X^i \frac{\partial Y^k}{\partial x^i}
}_{\text{I. Flat-Space Derivative Term}}
+
\underbrace{X^i \Gamma^k_{ij} Y^j
}_{\text{II. Curvature Correction Term}}
\right)\frac{\partial}{\partial{x^k}}
}
$$



This will be our main formula. If we stair at it for a bit, the following should be intuitive:

- First term: change in components, what we shall expect in Euclidean space. It measures how the components of $$Y$$ change. If the manifold were flat and the basis vectors didn’t vary, this would be enough. 
- Second term: correction for changes in basis, because on a curved manifold, the basis vectors $$\frac{\partial}{\partial x^j}$$ change from point to point. So even if the components $$Y^j$$ stay constant, the vector field itself might change because the basis is rotating or stretching. This change is captured by the second term including the Christoffel symbols, which measure how the coordinate frame twists and turns.

Consequently, covariant derivative is basically the combination of component change and basis change, and it's obvious from above the Christoffel symbols entirely dictate the properties of the chosen connection.

---

## 4] A Short Reflection and Discussion

Let's do a short summary and discussion before we continue. Based on the above definition, what is an affine connection conceptually? Well, an affine connection gives us a way to compare vectors at nearby points, which later will turn out to be our abilities to define 

* 1] a notion of parallel transport
* 2] a notion of geodesics
* 3] a notion of curvature

It is the structure that tells us how to move vectors around on a manifold. Without it, differentiation makes no sense.  

At the same time, notice that apriori we __choose__ this arbitrary connection. There are infinitely many affine connections on a manifold. Why? Recall the local definition of connection in coordinates $$\nabla_{X_i} X_j = \Gamma^k_{ij} X_k$$. The Christoffel symbols $$\Gamma^k_{ij}$$ can be any smooth functions. This already tells us that there are infinitely many possible connections. Nothing forces a specific choice at this stage. In fact, If $$\nabla$$ is a connection and $$A$$ is any smooth $$(1,2)$$-tensor field, then $$\tilde{\nabla}_XY = \nabla_XY + A(X, Y)$$ is another connection. So the space of connections is affine, not linear and thus there is enormous freedom. This is why it's called "affine connection". 

It's only after we impose some natural geometric conditions that one becomes canonical (more to reveal below). And after introducing ``Riemmanian metric``, there're specific properties of connection that we demand which render only one unique option left (kind of miracle). That's why we do not drown in choices. 

But let me also reinforce the idea that at this stage, no Riemannian metric at all is assumed. The definition of connection has nothing to do with any metric (we will see shortly that in some sense we do need to marry these two notions in a nice way; if this is your intuition, keep immersing yourself in it).

Another crucial point is that, you might object that after all why can’t we just subtract tangent vectors using the ambient Euclidean structure? If you visualize a manifold as a curved surface floating in 3D, there's clearly a nice inner product structure already defined in $$\mathbb{R}^3$$ which we effortlessly borrow. 

Well, this is the deeper part, echoeing all the way back to why we even want a manifold. Recall that a general manifold has no built-in shape. A smooth manifold is just a set of points with coordinate charts which are glued together smoothly. That’s it. There's no concept of length, angle, curvature, or embedding. Just smooth structure. We should __NOT__ think of a general manifold as a curved surface floating in space. That is only a visualization aid.

Shape appears only after adding extra structure. For a smooth manifold, only differentiability exists. We can talk about vector fields, Lie brackets, and differential forms. But there's __no metric__ yet. Now if we add an affine connection, we could then ifferentiate vector fields (and also define parallel transport and talk about geodesics, see below). However, there's still __no notion of length or angle__. It's only after we introduce a Riemannian metric that we get a notion of lengths, angles, volumes, and curvature. Only here does something resembling "shape" emerge. And remarkably, all of this can be defined without embedding.

But before Riemannian metric, for a generic smooth manifold, embeddings are misleading. When we picture a sphere in $$\mathbb{R}^3$$ or torus in space, we are seeing extrinsic geometry. But intrinsic geometry ignores how the object sits in space. A famous example is imagine a flat sheet of paper vs a cylinder made by rolling it. They look totaly different in $$\mathbb{R}^3$$, but __intrinsically they are the same geometry__, because distances measured along the surface are unchanged. Consequently, intrinsic geometry does not care about bending in ambient space.

Recall that Gauss proved in his Theorema Egregium that

> Curvature of a surface can be computed entirely from the __metric__.

(Here Curvature specifically refers to Gaussian Curvature). It does not depend on how the surface sits in space. This was revolutionary, only to be pushed further by Riemann later that:

> Manifolds need not be embedded at all.

They can exist abstractly.

[TODO]: That means to talk about intrinsic geometry we need to define a metric first? Also, the definition of Riemannian metric has nothing to do with the embedding. 

Consequently, if you are still thinking that a manifold is a curved surface in space, it might be helpful if you instead interpret manifold as an abstract smooth space and geometry is an additional structure placed on it. And more importantly, 

> The __metric__ determines what “curved” even means.

The manifold is just an abstract collection of points with smooth structure, not an object with a fixed geometric shape. Shape emerges only after adding a metric.

Now, with the above discussions, suppose we do have an embedding: $$M \in \mathbb{R}^n$$. We embed it. Vectors lie in tangent planes inside $$\mathbb{R}^n$$. Then yes, we can subtract them as vectors in $$\mathbb{R}^n$$. So what’s the problem? Well, it lies in again the insight Gauss offered back in 1827: __intrinsic vs extrinsic__ geometry. 

> Let $$V(t) \in T_{\alpha(t)}M$$. If we compute the ordinary derivative $$\frac{dV}{dt}$$ (pretend that $$M$$ never has existed), it sure lives in in $$\mathbb{R}^n$$ , but generally $$\frac{dV}{dt} \notin T_{\alpha(t)}M$$. It usually has a normal component. That normal component measures how the surface bends in space. But that bending __depends on the specific embedding__ we chose.

For example, on the sphere, if we carry a tangent vector along a great circle, the ambient derivative will point slightly inward (the normal component). But that normal part has nothing to do with intrinsic geometry, and it only reflects how the sphere sits in space. To get intrinsic change, we would take ambient derivative and project back to the tangent plane. (Spoiler alert: this projection then defines the Levi-Civita connection of the induced metric)

However, projecting back to tangent space gives intrinsic derivative.

- Different embeddings give different projections.
- Intrinsic geometry should not depend on embedding.

Connections defined intrinsically avoid embedding dependence.


To answer the question why we cannot always use this trick?

> Not all manifolds come with a preferred embedding. Manifolds might be embedded but the embedding is not unique. Different embeddings give different ambient derivatives and the induced extrinsic correction indeed depends on embedding. 

> On the other hand, intrinsic geometry should not depend on how we sit in space. If we rely on ambient subtraction, we're measuring extrinsic curvature. But Riemannian geometry is centered around exploring intrinsic properties. The connection should not depend on embedding.

Using Euclidean subtraction measures the intrinsic change plus the extrinsic bending, whereas the covariant derivative isolates and includes intrinsic change only. 

[TODO]: This leads to yet another interesting observation: is covariant derivative just a projection of the Euclidean derivative back onto the manifold?

Enough deliberation for now, let's continue our story. 

---

## 5] Covariant Derivative Along a Curve

In many scenarios, we are interested in how a vector field "changes" on a specific trajectory. With an affine connection defined as above, how could we calculate such change?

Given a curve $$\alpha(t)$$ and vector field along the curve: $$V(t) \in T_{\alpha(t)}M$$. Again, the ordinary derivative $\frac{dV}{dt}$ makes no sense because:

$$
\frac{dV}{dt} = \lim_{h \rightarrow 0}\frac{V(t + h) - V(t)}{h}
$$

but $$V(t+v) \in T_{\alpha(t+h)}M$$, $$V(t) \in T_{\alpha(t)}M$$, and subtraction requires vectors to live in the same space.

To resolve this conflict and apply the defined connection, we define:

$$
\frac{DV}{dt} = \nabla_{\alpha'(t)} V
$$

where $$\alpha'(t) = \frac{d\alpha}{dt} \in T_{\alpha(t)}M$$.

which is called the `covariant derivative`. In other words, the covariant derivative along a curve is just the affine connection applied in the direction of the velocity vector. It’s not new machinery — it’s just the connection specialized to a curve.

In coordinates, if we write $$V(t) = V^k(t)\frac{\partial}{\partial x^k}$$:

$$
\frac{DV^k}{dt}
= (\frac{dV^k}{dt}
+
\Gamma^k_{ij}
\frac{dx^i}{dt}
V^j)\frac{\partial}{\partial x^k}
$$

This is just the previous formula about coordinate representation of connections if we replace $$X = \alpha'(t)$$. Consequently, we could carry the same interpretation: Covariant derivative = change in components+change in basis. 

We could interpret it this way: Imagine walking on a sphere. We carry an arrow that we try to keep “pointing in the same direction.” Even if the arrow looks constant to you, from an external viewpoint it may be rotating because:

> The tangent plane itself rotates as you move.

> The coordinate basis changes.

So $$\frac{D V}{dt}$$ actually measures the true rate of change of the vector relative to/correcting for the manifold’s geometry. There are two kinds of change: 1)Intrinsic change — the vector itself changes' 2) Fake change — the coordinate frame changes. But $$\frac{D V}{dt}$$ removes the fake change. It measures intrinsic change.

---

## 6] Parallel Vector Field and Parallel Transport

Then the natural question to ask is what does it mean to have $$\frac{DV}{dt} = 0$$? 

Well, there's a specific name for it --- the vector field $$V$$ such $$\frac{DV}{dt} = 0$$ for a specific curve $$c$$ is called a `parallel vector field` along the $$c$$. Geometrically, this means that the vector is being transported without turning according to the manifold’s geometry.

On the other hand, the vector $$V(t)$$ on $$V$$ is said to `parallel transport` along $$c$$. 

Parallel transport is obtained by solving the following system of ODE:

$$
\frac{dV^k}{dt}
+
\Gamma^k_{ij}
\frac{dx^i}{dt}V^j
= 0
$$

Notice that this is just $$n$$ first order equations. Pointwise Christoffel data determines global comparison via integration.

This offers us another way to think of $\frac{D V}{dt}$:

> Take $$V(t+h)$$, parallel transport it back to $$T_{\alpha(t)}M$$, subtract $$V(t)$$, divide by $h$. Then take the limit. 

This is the hidden geometric definition.

---

## 7] Geodesics

Closely building upon the above idea of parallelism, we define a curve to be a ``geodesic`` if:

$$
\frac{D\alpha'}{dt} = 0
$$

Meaning:

> The velocity vector transports itself parallelly.

Geometric, this means that there is no intrinsic acceleration and the curve does not “turn” inside the manifold. This generalizes straight lines.

Another physical way to interpret this is that a geodesic has only acceleration perpendicular to the surface. 

A quick sanity check: In Euclidean space $$\mathbb{R}$$, since the basis vectors then Christoffel symbols vanish.

Consequently, $$\frac{DV}{dt} = \frac{dV}{dt}$$ --- Covariant derivative reduces to ordinary derivative, and this demonstrates that the definition of connection is truly a generalization.

And if we solve 

$$
\frac{d^2\alpha^k}{dt^2}
+
\Gamma^k_{ij}
\frac{dx^i}{dt}\alpha^j
= 0
$$

where we plug in $$V^j = \alpha^j$$,

we will arrive at a straight line, which is __the straight line__ in Euclidean space. 

---

## 8] Metric Compatibility

There're several ways to characterize metric compatibility. Let's start with the following: a connection $$\nabla$$ is compatible with a given metric $$<,>$$ if 

$$
X \langle Y,Z \rangle
= \langle \nabla_X Y, Z \rangle
+
\langle Y, \nabla_X Z \rangle
$$

This must hold for all vector fields $$X,Y,Z,\in \mathfrak{X}(M) $$. Intuitively, this represents that the derivative of the inner product equals the inner product of the derivatives. In other words, the connection differentiates vectors in a way that respects the metric. Note that here's where we start to marry connection with metric!

Another way to look at metric compatibility is through the lens of geodesic, which I think more closely epitomizes the essence of this concept: If a vector field $$V(t)$$ along a curve satisfies $$\frac{dV}{dt} = 0$$ (i.e., it's parallelly transported), then metric compatibility implies:

$$\frac{d}{dt}<V(t), V(t)> = 0$$

which means that the length of the geodesic $$V(t)$$ stays constant. 

More generally, if both $$V$$ and $$W$$ are parallel, then 

$$\frac{d}{dt}<V(t), W(t)> = 0$$

then the angles are also preserved. 

Consequently, this is equivalent to saying that, in coordinates, 

$$
\nabla_k g_{ij} = 0
$$

or just 

$$
\nabla g = 0
$$

This means that the metric tensor has zero covariant derivative. Consequently, the metric looks “constant” under parallel transport. Notice that this generalizes the idea that in Euclidean space $$\partial_k​ \delta_{ij} = 0$$.

A physical analogy will be that imagine each tangent space is a tiny rigid measuring device. Metric compatibility means that when we transport a measuring stick along a curve, its length does not change. If it did change, our notion of length would depend on the path we used to move it. That would be geometrically inconsistent.

Intuitively, metric compatibility suggests that when we move vectors around using the connection, we are not distorting the metric structure. There's no stretching, shrinking, nor skewing of angles. Metric compatibility ensures that parallel transport acts like an orthogonal transformation between tangent spaces. If we move a basis along a curve via parallel transport, it stays orthonormal. Or in other words, parallel transport behaves like rigid motion inside each tangent space. This is perhaps the cleanest geometric picture. 

And this comes not for free. We demand it by imposing such constraint. In general, connections are arbitrary. Most connections will change the length of a vector during parallel transport or distort angles. That would mean that the connection and the metric are fighting each other. However, metric compatibility forces harmony.

---

## 9] Torsion-Free (Symmetry)
We now understand that a connection is a rule for differentiating, and metric compatibility is to ensure that we preserve lengths and angles. There's only last remaning piece before we narrow down our infinite choices of connection. 

The formal definition of torsion-free, or symmetric, starts with the torsion tensor:

$$
T(X,Y) = \nabla_X Y - \nabla_Y X - [X,Y]
$$

If $$T = 0$$, then

$$
\nabla_X Y - \nabla_Y X = [X,Y]
$$

We call such connection torsion-free. In coordinate, we would have

$$
\Gamma^k_{ij} = \Gamma^k_{ji}
$$

that's why many people also call it symmetric. Notice that the above equation is straightforward since coordinate vector fields (coming from a coordinate chart) always commute ($$[\frac{\partial}{\partial x^i}, \frac{\partial}{\partial x^j}] = 0$$; Lie bracket has nothing to do with a connection apriori). Then just plug in $$\nabla_{X_i} X_j - \nabla_{X_j} X_i = 0$$ and the above resulst should surface naturally. In short, the symmetry of Christoffel symbols comes from both torsion-free condition and the fact that coordinate vector fields commute. 

Let me state it clearly: Coordinate vector fields always commute, independent of connection. Torsion-free means the connection respects that commutation structure. The connection does not determine the Lie bracket since the Lie bracket exists before any connection is chosen. Likewise, torsion-free does not make the Lie bracket zero — it only ensures that the connection respects whatever the Lie bracket already is.

This formula appears very bizarre, what does it mean? Recall that the Lie bracket between two vector fields $$[X, Y]$$ measures failure of flows to commute. If we first flow along $$X$$ and then $$Y$$, v.s. first $$Y$$ and then $$X$$, the difference is the Lie bracket: it measures intrinsic non-commutativity of directions. Now we look at $$\nabla_X Y - \nabla_Y X$$. This is what the connection says about the difference between directions.

Consequently, Torsion compares between 

- What the connection thinks the commutator is, and 
- What the actual geometric commutator (Lie bracket) is

and that's why it measures how much the connection _artificially twists_ the geometry beyond the natural commutator.

An intuitive picture is that imagine walking on a surface. Take two small steps, first in direction $$X$$, and then in direction $$Y$$, and compare with reversing the order. There are two possible reasons the final positions differ:

- The surface itself curves (intrinsic geometry), or 
- The connection is twisting things an extra amount.

Torsion measures the second. If torsion = 0, the connection __introduces no artificial twisting__. All the remaning non-commutativity comes purely from the underlying geometry itself, which is exactly what we want.

On the other way around, if we choose a connection with torsion. Then even though $$[X_i, X_j] = 0$$, we would have $$\nabla_{X_i} X_j - \nabla_{X_j} X_i \neq 0$$. That “extra part” would be torsion. Consequently, we could say that torsion measures deviation from symmetry beyond the intrinsic commutation.

Why does Riemannian geometry prefer torsion-free? Since Riemannian geometry models distance and angles, there is no natural “twisting” built into length geometry: the most natural connection is metric compatible and torsion-free, which ensures that geodesics behave like natural straightest paths.

In summary, a connection that is torsion-free does not introduce any artificial twisting beyond the natural commutation of vector fields.

---

## 10] Levi-Civita Connection

From the above, we know that metric compatibility controls stretching, whereas torsion-free controls twisting. Together they mean that the connection is the most natural differentiation compatible with smooth geometry and measurement.

Indeed, now we will manually impose 

1. Metric compatibility
2. Torsion-free

And, remarkably, there only exists exactly one such connection, which is called the ``Levi-Civita connection``.

As demanded by its defining property it removes stretching and twisting. Only intrinsic curvature remains.


It could be shown without too much effort that the Levi-Civita connection entirely depends on the selected metric. The Christoffel symbols of the Levi-Civita Connection are thus entirely determined by the components of the metric $g_{ij}$ and their first derivatives:

$$\boxed{\Gamma^k_{ij} = \frac{1}{2} g^{k\ell} \left( \frac{\partial g_{j\ell}}{\partial x^i} + \frac{\partial g_{i\ell}}{\partial x^j} - \frac{\partial g_{ij}}{\partial x^\ell} \right)}$$

(or if we just use the other notation: 
$$
\Gamma^k_{ij}
= \frac{1}{2}g^{k\ell}
\left(
X_i(g_{j\ell}) +
X_j(g_{i\ell}) -
X_\ell(g_{ij})
\right)
$$)

How do we obtain the above equation? We just directly calculate the Christoffel symbols using the metric compatibility and torsion-free conditions. It turns out to be not as messy as you might expect:

By definition of the connection coefficients:

$$
\nabla_{X_i} X_j
= \Gamma^k_{ij} X_k.
$$

By torsion-free condition we will have

$$
T(X_i,X_j) = \nabla_{X_i} X_j - \nabla_{X_j} X_i - [X_i,X_j] = 0.
$$

Since this is a coordinate frame, $$[X_i,X_j] = 0$$. Torsion-free implies: $$\nabla_{X_i} X_j = \nabla_{X_j} X_i$$, so $$\Gamma^k_{ij} = \Gamma^k_{ji}$$. Thus the lower indices are symmetric (remember I stated this property above!)

By metric compatibility, we would have $$\nabla g = 0$$. This means that 

$$
X_i(g_{jk}) = g(\nabla_{X_i}X_j, X_k) + g(X_j,\nabla_{X_i}X_k).
$$

Now if we substitute the definition of $$\Gamma_{ij}^l$$, since $$\nabla_{X_i} X_j = \Gamma^l_{ij} X_l$$, we get:

$$
g(\nabla_{X_i}X_j, X_k) = \Gamma^\ell_{ij} g_{\ell k}
$$

and

$$
g(X_j,\nabla_{X_i}X_k) = \Gamma^\ell_{ik} g_{j\ell}
$$

Thus:

$$
X_i(g_{jk})
= \Gamma^\ell_{ij} g_{\ell k} + \Gamma^\ell_{ik} g_{j\ell}
$$

Now comes a little tricky step: we write out the above formula but permute the indices (since they are choosen arbitrarily). We obtain:

(1)
$$
X_i(g_{jk})
= \Gamma^\ell_{ij} g_{\ell k} + \Gamma^\ell_{ik} g_{j\ell}
$$

(2)
$$
X_j(g_{ik}) = \Gamma^\ell_{ji} g_{\ell k} + \Gamma^\ell_{jk} g_{i\ell}
$$

(3)
$$
X_k(g_{ij}) = \Gamma^\ell_{ki} g_{\ell j} + \Gamma^\ell_{kj} g_{i\ell}
$$

Then, using the symmetry we obtained above from torsion-free condition, since

$$
\Gamma^\ell_{ij} = \Gamma^\ell_{ji},
$$

compute

$$
(1) + (2) - (3).
$$

The feft-hand side becomes

$$
X_i(g_{jk}) + X_j(g_{ik}) - X_k(g_{ij})
$$

Right-hand side simplifies to:

$$
2\,\Gamma^\ell_{ij} g_{\ell k}.
$$

Consequently, 

$$
X_i(g_{jk}) + X_j(g_{ik}) - X_k(g_{ij}) = 2\,\Gamma^\ell_{ij} g_{\ell k}.
$$

By rearranging terms and multiplying by the inverse metric $$g^{km}$$:

$$
g^{km} g_{\ell k} = \delta^m_\ell.
$$

We obtain:

$$
\Gamma^m_{ij} = \frac{1}{2} g^{mk} \left(X_i(g_{jk})+X_j(g_{ik})-X_k(g_{ij})\right).
$$

Renaming dummy indices:

$$
\boxed{
\Gamma^k_{ij}=\frac{1}{2}g^{k\ell}\left(X_i(g_{j\ell})+X_j(g_{i\ell})-X_\ell(g_{ij})\right)}
$$

Remark: This formula relies crucially on the fact that the frame $\{X_i\}$ is a **coordinate frame**, meaning:

$$
[X_i, X_j] = 0.
$$

If instead we used a general (non-coordinate) frame where

$$
[X_i, X_j] \neq 0,
$$

then additional terms involving the structure constants of the frame would appear in the formula for the connection. Thus the classical Christoffel formula holds precisely because the coordinate vector fields commute.



---


## 11] Quick Summary

* Connection: Infinitesimal rule for comparing tangent spaces.
* Christoffel symbols: Local coefficients describing how frames twist.
* Metric compatibility: No stretching under transport.
* Torsion-free: No artificial twisting.
* Levi-Civita connection: Unique natural connection for a Riemannian metric.
* Intrinsic geometry: Independent of embedding.

---

## 12] Discussions
When I first learned about connection/parallel transport/geodesics, I have tons of questions and different topics seem to mingle with one another, each defying the others' validity. After months of delibration and study, I finally figured out the inner workings of these concepts, and I have to admit that I'm still deepening my understanding. 

I'll not regurgitate statements and clarifications made in the previous "short summary" section 4], but instead think back on a few other critical questions to ponder. I highly recommen that readers go through 4] before reading this section. 

### 1) Why do connections exist even without metrics?

I had this thought which I deemed natural and hard to wrap my head aroud: If connections are about measuring changes, and metrics measure geometry, why can a connection exist without a metric?

Well, let's stay back a ste and think towards what a connection does and does _NOT_ do. Notice that a connection gives us a way to differentiate vector fields, to compare nearby tangent spaces. From there the notion of parallel transport and geodesics. However, notice that there's nowhere we mentioned lengths or angles. A connection is a rule about how vectors move, not about how long they are. It only needs to satisfies linearity in $$X$$ and Leibniz rule in $$Y$$. That’s purely algebraic and smooth structure, and there is no inner product in the definition. So logically:

> Connections exist on any smooth manifold; but metrics are optional.

Furthermore, _differentiation itself does not require a metric_. Think about perhaps ordinary calculus. When we compute $$\frac{df}{dx}$$, we do not need an inner product. Differentiation only needs smooth structure.

Similarly, on a manifold, to differentiate vector fields, we only need a mooth structure, a rule for comparing tangent spaces which we call/define as the connection. A metric is not logically required for this.

A simple analogy is roads vs ruler: Imagine that the manifold is a landscape. A connection tells us how to "carry arrows" while walking, and a metric tells us how long arrows are. We can describe how an arrow rotates along a path without knowing its length.

A concrete example here: Lie group. Take a Lie group $$G$$, we can define a connection using left translation:

$$\nabla_XY = 0$$

for left-invariant vector fields. This defines parallel transport algebraically, but there's no metric involved. This works because the group structure already lets use compare tangent spaces.

In Riemannian geometry, we care about length-minimizing curves and curvature measured from metric, and that's why we impose metric compatibility and torsion-free which produces the unique Levi-Civita connection. Howver, that’s a _specialization_.

In some sense, connections are more primitive than metrics, because 

> we can define curvature and geodesics from a connection alone.

Gauge theory in physics uses connections without metrics since metrics are additional structure, whereas connections are about differentiation — a more basic concept. A mental picture I have is that 

- Smooth structure $$\rightarrow$$ allows calculus
- Connection $$\rightarrow$$ allows differentiation of vector fields
- Metric $$\rightarrow$$ allows measurement

Each layer adds new capability and they are independent choices.


### 2) Point-wise definition of Christoffel symbols?
Another concern that haunted me for quite a while is the following: Now hopefully we are clear that connection is entirely dependent upon the Christoffel symbols, and connection is meant to differentiate vector fields at different points. Yet based on the definition of Christoffel symbols, it seems that it's just using the connection upon pairwise basis vectors at the same exact point. So how can something defined pointwise encode comparison between different tangent spaces?

Well, there're several persectives to look at this. 

Firstly, the hidden “different points” are actually there already: when we compute $$\nabla_XY$$, we are not subtracting $$Y(p)$$ and $$Y(q)$$ directly. Instead, we are asking: If I move an infinitesimal step from $$p$$ in direction $$X$$, how does $$Y$$ change? This “infinitesimal move” already involves neighboring points. So even though the formula is evaluated at $$p$$, it contains directional information about nearby points. Moreover, Christoffel symbols tell us how the frame twists and turns as we move. Once we know how the frame moves, we can differentiate any vector field. The “comparison across points” is encoded in how the frame changes.

Secondly, notice that a connection does not directly subtract vectors at different points. Instead, it gives an "infinitesimal rule" for how vector fields change. Indeed, a connection describes infinitesimal change. The comparison between different points comes from integrating this infinitesimal rule.

A simple analogy in $$\mathbb{R}^n$$. The derivative of a function is defined by $$f'(x)$$, which is computed at a single point. Yet from this local rule, we can compare values at different points by integrating:

$$
f(b) - f(a) = \int_a^b f'(t)\,dt
$$

so local derivative $$\rightarrow$$ global comparison. The same story here: Christoffel symbols encode infinitesimal twisting of the frame, as the definition shows:

$$
\nabla_{X_i} X_j = \Gamma^k_{ij} X_k
$$


This is indeed pointwise. But what does it mean? It tells us how the coordinate basis vector $$\frac{\partial}{\partial x^j}$$ changes when we move infinitesimally in direction $$\frac{\partial}{\partial x^i}$$. Consequently, even though the expression is evaluated at a single point, it describes __variation in a direction__. That direction information encodes how things change from one point to nearby points. Then that means we are integrating these infinitesimal changes? 

Exactly! Parallel transport along a curve is obtained by solving 

$$\frac{DV}{dt} = 0$$

To be more concrete, if we think in coordinates, we are solving 

$$
\boxed{
\frac{dV^k}{dt}
+
\Gamma^k_{ij}
\frac{dx^i}{dt}
V^j = 0
}
$$

for all $$k$$. This is an ODE! Given initial vector $$V(0)$$, the Christoffel symbols determine $$V(t)$$ uniquely. Consequently, they determine how vectors are transported from one point to another.

In summary we could think of Christoffel symbols as the “connection coefficients” describing how the local coordinate grid bends and twists. They encode infinitesimal change, from which we build parallel transport, geodesics, and later curvature. Consequently, yes indeed that Christoffel symbols are defined pointwise because connections describe infinitesimal variation, and infinitesimal variation is all we need to compare vectors at different points via integration.

---

[TODO]
Now our earlier question becomes clearer:

If we define derivatives using an embedding, we are using extra structure.

But intrinsic geometry should depend only on data defined on the manifold itself.

That’s why the Levi-Civita connection is defined purely from the metric.

No embedding required.


Theoretical (sanity check) questions to ponder:
1] Why curvature is intrinsic? Does intrinsic meaning only in need of a metric? Is metric required for intrinsic geometry or curvature?
2] Why do we say that we can define curvature and geodesics from a connection alone 
3] Or how geodesics make sense without embedding
4] How curvature arises from metric compatibility