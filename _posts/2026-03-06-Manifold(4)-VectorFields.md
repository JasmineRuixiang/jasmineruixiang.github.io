---
layout: post
title: "Manifold and Riemannian Geometry (4): Vector Fields"
date: 2026-02-23 21:39:24
description: Vector fields as ODEs, flows, Lie brackets, and the engines of probability flows
series: Manifold and Riemannian Geometry
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

In the previous posts of this series (Manifold and Riemannian Geometry (2) and (3)) we already built the tangent spaces $$T_pM$$ at a single point $$p$$. Remember that a single tangent space is a lonely object, though. It tells us which directions we could move in from $$p$$, and nothing about anywhere else. In fact, many visualizations of the "locally Euclidean" property of a manifold employs this tangent space structure (although this is again not rigorously correct since then we would already assume there exists some embedding...). Most of the geometry, physics and data analysis we care about happens when we choose a direction at every point at once, and choose it smoothly. This demand of a very intuitive tool/perspective is called a **vector field**.

Perhaps you've already seen vector fields from a (nonlinear) dynamical systems class. However, here we will make it more abstract and no longer assume the underlying flow-happening space to be a flat Euclidean one. Maybe think about the wind flow upon/on the surface of Earth?

Now, at a hight level view, this post is organized as the following ideas/progression:

1. A vector field is a smooth choice of tangent vector at every point (§0).
2. A vector field is secretly an ordinary differential equation, so the whole toolbox of dynamical systems moves onto manifolds (§1).
3. Solving that ODE for every starting point at once produces a **flow**: a one-parameter family of diffeomorphisms. Vector fields and flows are two descriptions of the same thing, one infinitesimal and one global (§2).
4. Once a vector field can move points around, it can also move *other* vector fields around, which lets us differentiate one field along another. That is the Lie derivative, and it comes out as the Lie bracket (§3). Note that this is subtly different from covariant derivative.
5. Finally, a flow moves probability mass as well as points. The bookkeeping is the continuity equation, which is an integral component to optimal transport and flow-based generative models (§4).

I'll assume the definitions of a smooth manifold and of $$T_pM$$ from parts (1) and (2), and use $$\partial_i$$ as shorthand for $$\frac{\partial}{\partial x^i}$$.

Let's roll our stone into the vector field.

---

## 0] Definitions

### 0.1] The tangent bundle: where vector fields live

To choose "one tangent vector at each point" we need a single space that contains all the tangent spaces at once. That space is the **tangent bundle**

$$
TM = \bigsqcup_{p \in M} T_pM = \{(p, v) : p \in M,\ v \in T_pM\},
$$

together with the projection $$\pi: TM \to M$$, $$\pi(p, v) = p$$. You can picture $$TM$$ as the manifold with a copy of $$\mathbb{R}^n$$ glued on at every point (Easiest case: a stack of papers but gradually rotating the orientation). These copies are the fibers $$\pi^{-1}(p) = T_pM$$.

$$TM$$ is itself a smooth manifold, of dimension $$2n$$. A chart $$(U, \varphi)$$ with coordinates $$(x^1, \dots, x^n)$$ on $$M$$ gives coordinates on $$\pi^{-1}(U)$$: send

$$
\Big(p,\ \sum_i v^i \,\partial_i\big|_p\Big) \ \longmapsto\ (x^1(p), \dots, x^n(p),\ v^1, \dots, v^n).
$$

The first $$n$$ numbers say *where* you are (that's the usual coordinates from our given charts), the last $$n$$ say *which arrow* you hold. When you switch to another chart $$(y^1, \dots, y^n)$$, the arrow coordinates change by the Jacobian, $$\tilde v^j = \sum_i \frac{\partial y^j}{\partial x^i}(p)\, v^i$$, which is smooth (this should be very obvious and I will not rederive or further elaborate here). Consequently, the transition maps are smooth and $$TM$$ is a smooth manifold (Lee, Ch. 3).

Two examples are worth keeping in mind:

- For the circle, $$TS^1$$ is a cylinder $$S^1 \times \mathbb{R}$$: at each point of the circle the tangent line is a copy of $$\mathbb{R}$$, and these lines can be lined up consistently all the way around.
- For the sphere, $$TS^2$$ is **not** $$S^2 \times \mathbb{R}^2$$, even though every fiber is a plane. The reason is the hairy ball theorem below. An interesting result is that locally the bundle is always a product, but globally it can twist (I will explain later why. Stay tuned!s).

### 0.2] Vector fields

> **Definition.** A **vector field** on $$M$$ is a map $$X: M \to TM$$ with $$\pi \circ X = \mathrm{id}_M$$. In words, $$X$$ assigns to each $$p$$ a vector $$X_p \in T_pM$$. It is **smooth** if it is smooth as a map between the manifolds $$M$$ and $$TM$$.

The condition $$\pi \circ X = \mathrm{id}$$ says that the arrow assigned to $$p$$ is attached at $$p$$ and not somewhere else. A map with this property is called a **section** of the bundle.

In a chart, any vector field can be written as

$$
X = \sum_{i=1}^n X^i \,\partial_i, \qquad X^i : U \to \mathbb{R},
$$

and $$X$$ is smooth exactly when all the component functions $$X^i$$ are smooth in every chart. This is the criterion we actually use. It also makes clear what a vector field is in practice: locally, it is just $$n$$ smooth functions, which are reinterpreted as an arrow at each point.

We write $$\mathfrak{X}(M)$$ for the set of smooth vector fields. We can add them pointwise, and we can multiply them by smooth *functions*, not just constants: $$(fX)_p = f(p) X_p$$. So $$\mathfrak{X}(M)$$ is a module over $$C^\infty(M)$$. As a real vector space it is infinite-dimensional, which is a useful reminder that a vector field is much more information than one vector.

### 0.3] A few examples

**In the plane.** Two fields will follow us through the whole post:

$$
W = -y\,\partial_x + x\,\partial_y \quad\text{(rotation)}, \qquad E = x\,\partial_x + y\,\partial_y \quad\text{(Euler / radial field)}.
$$

At the point $$(x, y)$$, $$W$$ points in the direction $$(-y, x)$$, which is perpendicular to the position vector, so $$W$$ swirls counterclockwise around the origin. $$E$$ points straight outward with length equal to the distance from the origin.

**On the circle.** The angle $$\theta$$ is *not* a global coordinate on $$S^1$$, since it jumps by $$2\pi$$ somewhere. Even so, the field $$\partial_\theta$$ is perfectly well defined everywhere. Any two angle charts differ locally by a constant, $$\tilde\theta = \theta + 2\pi k$$, so $$\partial_{\tilde\theta} = \partial_\theta$$ wherever both are defined. This is the yet another small example of a recurring theme: a global object can be built out of local coordinates that themselves fail to be global. In fact $$\partial_\theta$$ is just the rotation field $$W$$ restricted to the unit circle. At $$(\cos\theta, \sin\theta)$$, $$W$$ equals $$(-\sin\theta, \cos\theta)$$, which is tangent to the circle.

**On the sphere.** Rotating $$S^2 \subset \mathbb{R}^3$$ about the $$z$$-axis gives the field $$X(x, y, z) = (-y, x, 0)$$. It is tangent to the sphere because $$X \cdot (x, y, z) = 0$$. It vanishes at the north and south poles. And that is unavoidable by a famous theorem:

> **Hairy ball theorem.** Every continuous tangent vector field on $$S^2$$ vanishes at some point.

You cannot comb a hairy ball flat without leaving a cowlick (3B1B reecently published a [wonderful video](https://www.youtube.com/watch?v=BHdbsHFs2P0&t=11s) on this theorem). Equivalently, at any instant there is a point on Earth where the horizontal wind is zero (assuming the wind is continuous). The same holds on every even-dimensional sphere $$S^{2k}$$. On odd-dimensional spheres the obstruction disappears. On $$S^{2k-1} \subset \mathbb{R}^{2k}$$, the field

$$
X(x_1, x_2, \dots, x_{2k-1}, x_{2k}) = (-x_2, x_1, \dots, -x_{2k}, x_{2k-1})
$$

is tangent ($$X \cdot x = 0$$) and has unit length everywhere. The torus also carries a nowhere-vanishing field, namely "go around the long way".

Why such difference? What separates the sphere from the torus is topology. The **Poincaré–Hopf theorem** says that for a vector field with isolated zeros on a compact manifold, the indices of the zeros add up to the Euler characteristic $$\chi(M)$$. Since $$\chi(S^2) = 2 \neq 0$$ there must be at least one zero, while $$\chi(T^2) = 0$$ allows none (Milnor, *Topology from the Differentiable Viewpoint*). I find this one of the intuitive examples where we could see that __vector fields know about global shape__. We'll come back to that.

### 0.4] Vector fields are derivations

In Manifold and Riemannian Geoemtry(2), a tangent vector at $$p$$ was a *derivation at $$p$$*: a linear map $$v: C^\infty(M) \to \mathbb{R}$$ satisfying the Leibniz rule $$v(fg) = f(p)\,v(g) + g(p)\,v(f)$$. A vector field does this at every point at once, so it eats a function and returns a function:

$$
(Xf)(p) = X_p f, \qquad Xf = \sum_i X^i\, \frac{\partial f}{\partial x^i} \ \text{ in coordinates}.
$$

Then $$X: C^\infty(M) \to C^\infty(M)$$ is linear and satisfies

$$
X(fg) = f\, X(g) + g\, X(f).
$$

The converse is also true, and it is the cleanest way to think about vector fields algebraically: **every** linear map $$D: C^\infty(M) \to C^\infty(M)$$ satisfying the Leibniz rule is of the form $$Df = Xf$$ for a unique smooth vector field $$X$$ (Lee, Ch. 8). So "vector field" and "derivation of the algebra of smooth functions" are the same notion.

Applying our two planar fields to $$r^2 = x^2 + y^2$$ is instructive:

$$
W(x^2 + y^2) = -y \cdot 2x + x \cdot 2y = 0, \qquad E(x^2 + y^2) = 2x^2 + 2y^2 = 2r^2.
$$

What $$Xf$$ represents is the rate of change of $$f$$ as we move along $$X$$. The rotation field doesn't change the distance from the origin, and the radial field increases it.

### 0.5] A warning: vector fields do not push forward along arbitrary maps

A smooth map $$F: M \to N$$ pushes individual tangent vectors forward with its differential, $$dF_p: T_pM \to T_{F(p)}N$$. It is tempting to push a whole vector field forward the same way. However, in general this fails, for two reasons.

- **$$F$$ not injective.** Take $$F: \mathbb{R} \to \mathbb{R}$$, $$F(t) = t^2$$, and $$X = \partial_t$$. Both $$t = 1$$ and $$t = -1$$ land on $$1$$, but $$dF(\partial_t) = 2t\,\partial_s$$ gives $$+2\,\partial_s$$ from one and $$-2\,\partial_s$$ from the other. There is no consistent answer at $$s = 1$$.
- **$$F$$ not surjective.** Points of $$N$$ outside the image receive no vector at all.

When $$F$$ is a **diffeomorphism**, both problems disappear, and the pushforward

$$
(F_*X)_{F(p)} = dF_p(X_p)
$$

is a well-defined smooth vector field on $$N$$. More generally we say $$X$$ on $$M$$ and $$Y$$ on $$N$$ are **$$F$$-related** if $$dF_p(X_p) = Y_{F(p)}$$ for all $$p$$. Keep diffeomorphism pushforwards in mind. In §3 we will push a vector field along the flow of another one.

---

## 1] Dynamical systems scaffolding

### 1.1] Integral curves: a vector field is an ODE

A curve $$\gamma: J \to M$$ is an **integral curve** of $$X$$ if its velocity always equals the field at its current position:

$$
\gamma'(t) = X_{\gamma(t)} \quad \text{for all } t \in J.
$$

In coordinates, writing $$\gamma(t) = (x^1(t), \dots, x^n(t))$$, this is the system of ODEs

$$
\dot x^i(t) = X^i\big(x^1(t), \dots, x^n(t)\big), \qquad i = 1, \dots, n.
$$

So **a smooth vector field on $$M$$ is exactly an autonomous ODE on $$M$$**, and an integral curve is a solution. Picture dropping a leaf into a stream: the vector field is the current, and the integral curve is the path of the leaf. For our planar examples:

- $W$: $$\dot x = -y,\ \dot y = x$$. Solutions are circles traced at unit angular speed, $$(r\cos(t + \phi),\ r\sin(t + \phi))$$.
- $E$: $$\dot x = x,\ \dot y = y$$. Solutions are rays moving outward exponentially, $$e^t(x_0, y_0)$$.

The payoff is that everything in dynamical systems (fixed points, stability, limit cycles, chaos) can now be stated on a manifold without reference to coordinates. The dynamics only cares about the field.

### 1.2] Existence and uniqueness

Locally, in a chart, the Picard–Lindelöf theorem gives a unique solution through each point for a short time (recall the first/second class of your nonlinear dynamical systems!). Uniqueness lets us glue these local pieces together, which yields:

> For each $$p \in M$$ there is a unique **maximal** integral curve $$\gamma_p: J_p \to M$$ with $$\gamma_p(0) = p$$, defined on an open interval $$J_p \ni 0$$. Every other integral curve starting at $$p$$ is a restriction of it.

Uniqueness has a geometric meaning worth stating plainly: **integral curves never cross.** If two of them passed through the same point, they would both solve the same initial value problem there, so they would coincide. This is why phase portraits look like smooth combed hair rather than tangled string (if indeed tangled, that might suggest we are looking at some low dimensional projection of the real flow...).

### 1.3] Completeness: can the leaf escape?

A vector field is **complete** if every maximal integral curve is defined for all time, $$J_p = \mathbb{R}$$. Smoothness alone does not give you this. On $$\mathbb{R}$$, consider $$X = x^2\,\partial_x$$. The ODE $$\dot x = x^2$$ with $$x(0) = x_0 > 0$$ has solution

$$
x(t) = \frac{x_0}{1 - x_0 t},
$$

which reaches infinity at $$t = 1/x_0$$. The farther out the leaf gets, the faster the current flows, and it escapes to infinity in finite time. So $$J_{x_0} = (-\infty, 1/x_0)$$.

Incompleteness can also come from the manifold rather than the field. On $$M = \mathbb{R}^2 \setminus \{0\}$$, the constant field $$\partial_x$$ carries the point $$(-1, 0)$$ into the missing origin at $$t = 1$$, and the curve simply stops existing.

Both failures need somewhere to escape to, and a compact manifold has nowhere. Indeed, **every smooth vector field on a compact manifold is complete**, and more generally so is every compactly supported field (Lee, Ch. 9).

### 1.4] Zeros and linearization

If $$X_p = 0$$, the constant curve $$\gamma(t) \equiv p$$ is an integral curve. In dynamical-systems language, $$p$$ is an **equilibrium** or **fixed point**. Near an equilibrium, the natural move is to linearize. In coordinates centered at $$p$$, set

$$
A^i{}_j = \frac{\partial X^i}{\partial x^j}(p).
$$

There is a subtlety I have to point out. For a general vector field, the Jacobian matrix of its components is **not** coordinate-independent. At a zero, however, it is. If we change coordinates $$x \mapsto y$$, the components transform as $$\tilde X^k(y) = \sum_i \frac{\partial y^k}{\partial x^i} X^i(x)$$. Differentiating with the product rule,

$$
\frac{\partial \tilde X^k}{\partial y^l}
= \sum_{i,m} \frac{\partial^2 y^k}{\partial x^m \partial x^i}\,\frac{\partial x^m}{\partial y^l}\, X^i
\;+\; \sum_{i,m} \frac{\partial y^k}{\partial x^i}\,\frac{\partial X^i}{\partial x^m}\,\frac{\partial x^m}{\partial y^l}.
$$

At a zero of $$X$$ the first sum vanishes, leaving $$\tilde A = J A J^{-1}$$ with $$J = \partial y / \partial x$$. So at an equilibrium the linearization is a well-defined linear map $$T_pM \to T_pM$$, and its eigenvalues are intrinsic. Away from zeros, the first sum survives and __"the derivative of $$X$$"__ depends on the coordinates. That is exactly the problem a *connection* solves, and it is also why §3 has to work harder.

For linear fields $$X(x) = Ax$$ on $$\mathbb{R}^2$$, the eigenvalues of $$A$$ determine the whole phase portrait:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/blogs/manifold4/phase_portraits.png" class="img-fluid rounded z-depth-1" zoomable=true width="100%" %}
    </div>
</div>
<div class="caption">
    Integral curves of three linear fields $$X(x) = Ax$$. Left: eigenvalues $$\pm i$$, closed orbits around a center. Middle: eigenvalues $$\pm 1$$, a saddle. Right: eigenvalues $$-0.3 \pm i$$, a spiral sink.
</div>

The left and right panels differ only by a little damping, $$-0.3$$ on the diagonal, and yet one has closed orbits and the other spirals into the origin. Centers are fragile in this sense. The **Hartman–Grobman theorem** says that near a *hyperbolic* equilibrium (no eigenvalue with zero real part), the nonlinear flow is topologically the same as its linearization. Saddles and sinks survive small perturbations. A center has purely imaginary eigenvalues, so it is not hyperbolic, and the linearization can't decide what the nonlinear system does (Strogatz, *Nonlinear Dynamics and Chaos*).

Here is a connection to neuroscience that I find satisfying. jPCA models motor-cortex population activity as $$\dot x = Mx$$ with $$M$$ constrained to be **skew-symmetric**, $$M^\top = -M$$. The eigenvalues of a skew-symmetric matrix are purely imaginary, so the fitted dynamics is pure rotation, exactly the left panel. In the language of this post, jPCA finds the best-fitting *rotational vector field* in the neural state space. (I wrote about jPCA in more detail in the Research (1) post.)

### 1.5] Time-dependent fields

At times, we'll also need vector fields that change in time, $$X_t$$, with ODE $$\dot\gamma(t) = X_t(\gamma(t))$$. Generative models (§4) are built on these. A standard trick reduces them to the autonomous case: on $$M \times \mathbb{R}$$, define the field $$\tilde X = X_t + \partial_t$$, so that time becomes one more coordinate that ticks forward at unit speed. Everything in §1 and §2 then applies, with the caveat that the solution maps compose as a two-time family, $$\theta_{t,s}\circ\theta_{s,r} = \theta_{t,r}$$, rather than as a one-parameter group.

---

## 2] Flows: vector fields are infinitesimal diffeomorphisms

### 2.1] From a field to a flow

§1 solved the ODE for one initial point. Now we solve it for **all** initial points at once and collect the answers into a single map:

$$
\theta(t, p) = \theta_t(p) = \gamma_p(t),
$$

meaning "where the leaf dropped at $$p$$ is after time $$t$$". This is the **flow** of $$X$$. Instead of following one leaf, picture the whole surface of the stream moving at once (you might immediately think about the transformation of an entire space or even a distribution...).

> **Fundamental theorem on flows** (Lee, Ch. 9). Let $$X$$ be a smooth vector field. Then $$\theta$$ is defined on an open set $$\mathcal{D} \subseteq \mathbb{R} \times M$$ containing $$\{0\} \times M$$, $$\theta$$ is smooth, and
> $$
> \theta_0 = \mathrm{id}_M, \qquad \theta_t \circ \theta_s = \theta_{t+s} \quad\text{wherever defined.}
> $$
> If $$X$$ is complete, each $$\theta_t$$ is a diffeomorphism of $$M$$ with inverse $$\theta_{-t}$$.

The group law is the heart of the theorem, and its proof is short enough to show. Fix $$s$$ and $$p$$, and compare two curves:

$$
\alpha(t) = \theta_{t+s}(p), \qquad \beta(t) = \theta_t(\theta_s(p)).
$$

Both are integral curves of $$X$$. For $$\alpha$$ this is just a time shift of $$\gamma_p$$, and for $$\beta$$ it is the definition of $$\gamma_{\theta_s(p)}$$. At $$t = 0$$ both equal $$\theta_s(p)$$. By uniqueness, $$\alpha = \beta$$. In words: **flowing for $$s$$ and then for $$t$$ is the same as flowing for $$s + t$$**, because the stream doesn't remember when you entered it. That is precisely what "autonomous" means, and it is why the time-dependent case of §1.5 does not give a group.

The hard part of the theorem is smoothness in $$p$$ (smooth dependence on initial conditions), which needs real ODE analysis. The algebra above is the part worth remembering.

### 2.2] Examples

**Linear fields.** For $$X(x) = Ax$$ on $$\mathbb{R}^n$$, the flow is the matrix exponential

$$
\theta_t(x) = e^{tA} x, \qquad \text{since}\quad \frac{d}{dt} e^{tA}x = A\, e^{tA} x = X(\theta_t(x)).
$$

The group law is $$e^{tA}e^{sA} = e^{(t+s)A}$$. For the rotation field, $$A = J = \begin{pmatrix} 0 & -1 \\ 1 & 0 \end{pmatrix}$$ and

$$
e^{tJ} = \begin{pmatrix} \cos t & -\sin t \\ \sin t & \cos t \end{pmatrix},
$$

which is rotation by angle $$t$$. For the Euler field, $$A = I$$ and $$\theta_t(x) = e^t x$$ is dilation. So the *exponential map* of matrix groups is the special case "flow of a linear vector field", a fact we'll lean on when we get to Lie groups.

**A nonlinear, incomplete example.** For $$X = x^2\,\partial_x$$ on $$\mathbb{R}$$, §1.3 gives $$\theta_t(x) = \frac{x}{1 - tx}$$, defined only while $$tx < 1$$. The group law still holds where defined:

$$
\theta_s(\theta_t(x)) = \frac{\frac{x}{1-tx}}{1 - \frac{sx}{1 - tx}} = \frac{x}{1 - tx - sx} = \theta_{s+t}(x).
$$

**The pendulum.** For $$X = y\,\partial_x - \sin x\,\partial_y$$ (angle $$x$$, angular velocity $$y$$), there's no closed form. The figure shows what the flow does to a whole disk of initial conditions:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/blogs/manifold4/pendulum_blob.png" class="img-fluid rounded z-depth-1" zoomable=true width="75%" %}
    </div>
</div>
<div class="caption">
    A disk $$D$$ of pendulum states carried by the flow $$\theta_t$$. Each $$\theta_t(D)$$ is the image of the disk under a diffeomorphism: it bends and shears, but never tears or overlaps itself. The area stays at $$0.385$$ throughout, which §4.3 explains.
</div>

Two things are visible here. Each $$\theta_t$$ is a diffeomorphism, so the blob deforms smoothly and stays in one piece. Points near the center swing around faster than points near the outer edge (small oscillations have a shorter period than large ones), so the disk gets sheared into a crescent. The constant area is not a coincidence. We'll see in §4 that it follows from $$\operatorname{div} X = 0$$.

### 2.3] From a flow back to a field

The construction runs the other way too. Suppose $$\theta: \mathbb{R} \times M \to M$$ is any smooth map with $$\theta_0 = \mathrm{id}$$ and $$\theta_t \circ \theta_s = \theta_{t+s}$$, that is, a smooth action of the group $$(\mathbb{R}, +)$$ on $$M$$. Define its **infinitesimal generator**

$$
X_p = \frac{d}{dt}\Big|_{t=0} \theta_t(p) \in T_pM.
$$

Then $$\theta$$ is the flow of $$X$$, because

$$
\frac{d}{dt}\theta_t(p) = \frac{d}{ds}\Big|_{s=0} \theta_{t+s}(p) = \frac{d}{ds}\Big|_{s=0} \theta_s\big(\theta_t(p)\big) = X_{\theta_t(p)}.
$$

Putting §2.1 and §2.3 together:

$$
\boxed{\ \{\text{complete smooth vector fields on } M\} \ \longleftrightarrow\ \{\text{smooth one-parameter groups of diffeomorphisms } \theta_t\}\ }
$$

On a compact manifold the word "complete" can be dropped. This is the precise sense in which a vector field is an **infinitesimal diffeomorphism**: it is the velocity, at $$t = 0$$, of a path of diffeomorphisms starting at the identity. If you know Lie groups, this should feel familiar. An element of a Lie algebra generates a one-parameter subgroup through the exponential map. Heuristically, $$\mathfrak{X}(M)$$ plays the role of the Lie algebra of the (infinite-dimensional) group $$\mathrm{Diff}(M)$$. Making that fully rigorous takes more care than in finite dimensions, but the intuition is sound, and it even extends to brackets in §3.

### 2.4] What flows can't do

Not every diffeomorphism is the time-1 map $$\theta_1$$ of some flow. Two quick obstructions:

- **Orientation.** On a manifold with a chosen orientation, the determinant $$\det (d\theta_t)_p$$, computed in oriented charts, depends continuously on $$t$$, equals $$1$$ at $$t = 0$$, and is never zero because each $$\theta_t$$ is a diffeomorphism. So it stays positive, and every $$\theta_t$$ preserves orientation. The reflection $$x \mapsto -x$$ of $$\mathbb{R}$$ is therefore not the time-1 map of any flow.
- **Square roots.** $$\theta_1 = \theta_{1/2} \circ \theta_{1/2}$$, so a time-1 map always has a "square root" that is itself a diffeomorphism. Not every diffeomorphism does.

So flows sit inside the diffeomorphisms that are connected to the identity, and even there they don't exhaust everything. The infinitesimal-to-global dictionary is exact only in the direction of §2.3.

### 2.5] Away from zeros, every field looks the same

Here is a theorem that clarified a lot for me:

> **Flow-box (straightening) theorem.** If $$X_p \neq 0$$, there are coordinates $$(x^1, \dots, x^n)$$ near $$p$$ in which $$X = \partial_1$$.

The idea of the proof is to pick a small hypersurface through $$p$$ that is transverse to $$X$$, parametrize it by $$(x^2, \dots, x^n)$$, and use the flow time $$x^1$$ as the remaining coordinate: $$(x^1, x^2, \dots, x^n) \mapsto \theta_{x^1}(\sigma(x^2, \dots, x^n))$$. In these coordinates, moving along $$X$$ just means increasing $$x^1$$ (Lee, Ch. 9).

A simple instance: on $$\mathbb{R}^2 \setminus \{0\}$$, polar coordinates straighten the rotation field. Since $$x = r\cos\theta$$ and $$y = r\sin\theta$$,

$$
\partial_\theta = \frac{\partial x}{\partial\theta}\partial_x + \frac{\partial y}{\partial\theta}\partial_y = -r\sin\theta\,\partial_x + r\cos\theta\,\partial_y = -y\,\partial_x + x\,\partial_y = W.
$$

The moral is surprising at first: **locally, away from its zeros, a vector field has no features at all.** It looks like a uniform wind. Everything interesting about a vector field is either at its zeros (the phase portraits of §1.4) or global (closed orbits, Poincaré–Hopf, the hairy ball). The local theory is trivial, so the content is in the singularities and the topology.

---

## 3] A vector field lets you differentiate: the Lie derivative

### 3.1] The problem, and a flow-based answer

How does a vector field $$Y$$ change as we move in some direction? In $$\mathbb{R}^n$$ you subtract $$Y$$ at nearby points and divide. On a manifold that subtraction is meaningless: $$Y_q \in T_qM$$ and $$Y_p \in T_pM$$ live in different vector spaces, and nothing canonical identifies them. In the connection post, the remedy was to *add* structure, an affine connection $$\nabla$$, which is a choice.

There is another option that requires no choice at all, *provided* we are differentiating along a vector field $$X$$ rather than a single vector. The flow $$\theta_t$$ of $$X$$ gives a canonical linear isomorphism

$$
d(\theta_{-t})_{\theta_t(p)}: T_{\theta_t(p)}M \longrightarrow T_pM,
$$

which carries vectors at the downstream point back to $$p$$. So: ride the current downstream for time $$t$$, read off $$Y$$ there, let the current carry that arrow back to $$p$$, and compare it with $$Y_p$$:

$$
(\mathcal{L}_X Y)_p = \frac{d}{dt}\Big|_{t=0} d(\theta_{-t})_{\theta_t(p)}\big(Y_{\theta_t(p)}\big) = \lim_{t\to 0} \frac{d(\theta_{-t})\big(Y_{\theta_t(p)}\big) - Y_p}{t}.
$$

This is the **Lie derivative** of $$Y$$ along $$X$$. Both vectors in the difference now live in $$T_pM$$, so the subtraction makes sense. For a function, there's nothing to transport, and the same recipe gives the familiar directional derivative: $$\mathcal{L}_X f = \frac{d}{dt}\big|_{0} f(\theta_t(p)) = Xf$$.

Notice what $$\mathcal{L}_X Y$$ measures. It vanishes exactly when $$Y$$ is carried into itself by the flow of $$X$$. It does **not** measure whether $$Y$$ is "constant" in some absolute sense. It measures how $$Y$$ fails to be dragged along by $$X$$.

### 3.2] The Lie bracket computes it

The definition above is geometric but awkward to compute. The fix is an algebraic gadget. Given $$X, Y \in \mathfrak{X}(M)$$, define

$$
[X, Y] f = X(Yf) - Y(Xf).
$$

At first sight this looks like a second-order operator, but the second derivatives cancel. In coordinates, with $$X = X^i\partial_i$$ and $$Y = Y^j\partial_j$$ (summation implied),

$$
X(Yf) = X^i \partial_i Y^j\, \partial_j f + X^i Y^j\, \partial_i\partial_j f,
$$

and subtracting the same expression with $$X$$ and $$Y$$ swapped, the symmetric $$\partial_i\partial_j f$$ terms cancel:

$$
[X, Y] = \big(X^i\,\partial_i Y^j - Y^i\,\partial_i X^j\big)\,\partial_j.
$$

So $$[X, Y]$$ is again a vector field, the **Lie bracket**. The central fact is:

> **Theorem** (Lee, Ch. 9). $$\mathcal{L}_X Y = [X, Y].$$

Let's check it on an example, both ways. Take $$X = \partial_x$$ and $$Y = x\,\partial_y$$ on $$\mathbb{R}^2$$.

*Via the bracket.* $$X^i\partial_i Y^j$$ is nonzero only for $$j = y$$: $$\partial_x(x) = 1$$. And $$Y^i\partial_i X^j = 0$$ since $$X$$ has constant components. So $$[X, Y] = \partial_y$$.

*Via the flow.* The flow of $$X$$ is translation, $$\theta_t(x, y) = (x + t, y)$$, so $$d\theta_{-t}$$ is the identity matrix. At $$\theta_t(p)$$, $$Y = (0, x + t)$$. Carrying it back changes nothing, so the transported vector is $$(0, x + t)$$, and its $$t$$-derivative is $$(0, 1) = \partial_y$$. The two answers agree.

### 3.3] Geometric meaning: do the flows commute?

The bracket has a second, very visual meaning. Let $$\theta$$ and $$\psi$$ be the flows of $$X$$ and $$Y$$. Starting at $$p$$, flow along $$X$$ for time $$t$$, then along $$Y$$ for $$s$$, then along $$X$$ for $$-t$$, then along $$Y$$ for $$-s$$. If the flows commuted you would be back at $$p$$. In general you aren't, and in coordinates

$$
\psi_{-s}\circ\theta_{-t}\circ\psi_s\circ\theta_t(p) = p + st\,[X, Y]_p + O\big((|s| + |t|)^3\big).
$$

For our example the loop can be computed by hand. Starting at $$p = (x_0, y_0)$$:

$$
(x_0, y_0) \xrightarrow{\ X,\ t\ } (x_0 + t, y_0) \xrightarrow{\ Y,\ s\ } (x_0 + t,\ y_0 + s(x_0 + t)) \xrightarrow{\ X,\ -t\ } (x_0,\ y_0 + s(x_0 + t)) \xrightarrow{\ Y,\ -s\ } (x_0,\ y_0 + st).
$$

The gap is exactly $$st\,\partial_y = st\,[X, Y]_p$$. The mechanism is clear from the picture: $$Y$$ is stronger farther to the right, so the upward trip (taken at $$x_0 + t$$) is longer than the downward return (taken at $$x_0$$).

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/blogs/manifold4/lie_bracket_gap.png" class="img-fluid rounded z-depth-1" zoomable=true width="55%" %}
    </div>
</div>
<div class="caption">
    The "parallelogram" built from the flows of $$X = \partial_x$$ and $$Y = x\,\partial_y$$ ($$t = s = 1$$, starting at $$p = (0.5, 0)$$) fails to close. The gap is $$st\,[X,Y]_p$$.
</div>

This gives a precise answer to a question that bothered me early on: *when can a set of vector fields be coordinate vector fields?* Coordinate fields always commute, $$[\partial_i, \partial_j] = 0$$, because mixed partial derivatives commute. The converse holds too: if $$X_1, \dots, X_k$$ are linearly independent at each point and pairwise commute, there are local coordinates in which $$X_i = \partial_i$$ (Lee, Ch. 9). The bracket is the obstruction.

Polar coordinates give a nice illustration. Physicists like the orthonormal frame $$\hat e_r = \partial_r$$, $$\hat e_\theta = \frac{1}{r}\partial_\theta$$. Its bracket is

$$
[\hat e_r, \hat e_\theta] = \Big[\partial_r, \tfrac{1}{r}\partial_\theta\Big] = \partial_r\Big(\tfrac{1}{r}\Big)\,\partial_\theta = -\tfrac{1}{r^2}\,\partial_\theta = -\tfrac{1}{r}\,\hat e_\theta \neq 0.
$$

So **no** coordinate system has $$\hat e_r, \hat e_\theta$$ as its coordinate vectors. That's why $$\hat e_\theta$$ and $$\partial_\theta$$ have to be kept apart, and why formulas for divergence and Laplacian in polar coordinates pick up those extra $$1/r$$ factors.

### 3.4] Why the Lie derivative is not a connection

Since the Lie derivative differentiates vector fields without any extra structure, why bother with connections? Because it fails one property that a connection must have. A connection is **$$C^\infty(M)$$-linear** in the direction: $$\nabla_{fX}Y = f\,\nabla_X Y$$. This is what lets you define $$\nabla_v Y$$ for a *single* vector $$v \in T_pM$$. The Lie derivative is not:

$$
[fX, Y]g = fX(Yg) - Y(f\,Xg) = fX(Yg) - (Yf)(Xg) - fY(Xg),
$$

so

$$
\mathcal{L}_{fX}Y = f\,\mathcal{L}_X Y - (Yf)\,X.
$$

The extra term $$-(Yf)X$$ means that $$(\mathcal{L}_X Y)_p$$ depends on how $$X$$ behaves *near* $$p$$, not just on $$X_p$$. Here is a concrete case. On $$\mathbb{R}^2$$ take $$Y = \partial_y$$ and compare $$X = \partial_x$$ with $$\tilde X = (1 + y)\,\partial_x$$. At the origin, $$X = \tilde X = \partial_x$$. But $$[X, Y] = 0$$ while $$[\tilde X, Y] = -\partial_y(1 + y)\,\partial_x = -\partial_x$$. Same vector at the point, different Lie derivatives.

This is intuitive once you recall the definition: $$\mathcal{L}_X$$ uses the *flow* of $$X$$, and a flow depends on the whole field. So we have two tools that complement each other:

| | Lie derivative $$\mathcal{L}_X Y$$ | Covariant derivative $$\nabla_X Y$$ |
|---|---|---|
| Extra structure needed | none | a choice of connection |
| Needs $$X$$ near $$p$$, or only $$X_p$$? | near $$p$$ (uses the flow) | only $$X_p$$ |
| Linear over functions in $$X$$? | no | yes |
| Transports vectors along | flow lines of $$X$$ only | any curve |

In that sense a vector field carries its own private notion of transport, but only along its own flow lines. The two notions meet through **torsion**, $$T(X, Y) = \nabla_X Y - \nabla_Y X - [X, Y]$$. For the Levi-Civita connection, which is torsion-free, $$[X, Y] = \nabla_X Y - \nabla_Y X$$. In $$\mathbb{R}^n$$ with the flat connection this reads $$[X, Y] = DY \cdot X - DX \cdot Y$$, which is the coordinate formula from §3.2.

### 3.5] Lie derivatives of other objects

The same recipe (pull back along the flow, then differentiate) works for any geometric object, not just vector fields. Two cases matter a lot later on.

**The metric.** If $$g$$ is a Riemannian metric, $$\mathcal{L}_X g = 0$$ means that every $$\theta_t$$ is an isometry. Such an $$X$$ is a **Killing field**. In the plane, the rotation field $$W$$ is Killing, since rotations preserve lengths. The Euler field is not. Its flow $$\theta_t(x) = e^t x$$ scales all lengths by $$e^t$$, so $$\theta_t^* g = e^{2t} g$$ and $$\mathcal{L}_E\, g = 2g$$.

**The volume form.** If $$\mu$$ is a volume form, then $$\mathcal{L}_X \mu$$ is again a top-degree form, so it is a function multiple of $$\mu$$:

$$
\mathcal{L}_X \mu = (\operatorname{div}_\mu X)\, \mu.
$$

This is the **coordinate-free definition of divergence**: the rate at which the flow of $$X$$ expands volume. In $$\mathbb{R}^n$$ with $$\mu = dx^1\wedge\cdots\wedge dx^n$$ it reduces to $$\sum_i \partial_i X^i$$. For our fields, $$\operatorname{div} W = 0$$ (rotations preserve area) and $$\operatorname{div} E = n$$ (dilation by $$e^t$$ scales volume by $$e^{nt}$$). Divergence is the bridge to the last section.

---

## 4] Vector fields move probability

### 4.1] Pushing a distribution through a flow

So far a flow has moved *points*. Now drop a cloud of particles into the stream. Let $$x_0$$ be random with density $$\rho_0$$, and let each particle follow a (possibly time-dependent) field $$X_t$$, so $$x_t = \theta_t(x_0)$$. The distribution of $$x_t$$ is the **pushforward** $$\rho_t = (\theta_t)_\#\rho_0$$, defined by

$$
\Pr(x_t \in A) = \Pr\big(x_0 \in \theta_t^{-1}(A)\big) \quad\text{for every measurable } A.
$$

Since $$\theta_t$$ is a diffeomorphism, the change of variables formula turns this into a pointwise statement about densities:

$$
\rho_t\big(\theta_t(x)\big)\,\det D\theta_t(x) = \rho_0(x).
$$

The mass a particle carries is conserved. If the flow spreads a small neighborhood over twice the volume, the density there drops by half.

### 4.2] The continuity equation

The pushforward is a global description. Its infinitesimal version is a PDE, and we can derive it with a test function. Take any smooth, compactly supported $$f$$. Then

$$
\frac{d}{dt}\int f(x)\,\rho_t(x)\,dx
= \frac{d}{dt}\int f\big(\theta_t(x)\big)\,\rho_0(x)\,dx
= \int \nabla f\big(\theta_t(x)\big)\cdot X_t\big(\theta_t(x)\big)\,\rho_0(x)\,dx
= \int \nabla f \cdot X_t\, \rho_t\, dx.
$$

The first equality is the definition of pushforward, the second is the chain rule plus $$\frac{d}{dt}\theta_t = X_t\circ\theta_t$$, and the third is the pushforward again. Integrating by parts (no boundary terms, since $$f$$ is compactly supported) gives

$$
\int f\,\partial_t\rho_t\,dx = -\int f\,\nabla\cdot(\rho_t X_t)\,dx.
$$

Since this holds for every $$f$$,

$$
\boxed{\ \partial_t \rho_t + \nabla\cdot\big(\rho_t\, X_t\big) = 0.\ }
$$

This is the **continuity equation**. It reads like a bookkeeping rule: at a fixed location, density goes up or down according to the net flow of mass $$\rho X$$ out of that location. It is the same law that says fluid isn't created or destroyed. On a manifold with volume form $$\mu$$, replace $$\nabla\cdot$$ by the divergence of §3.5.

### 4.3] Riding along with a particle

The continuity equation describes what an observer standing at a fixed point sees (the **Eulerian** view). What does a particle riding the flow see (the **Lagrangian** view)? Expanding $$\nabla\cdot(\rho X) = X\cdot\nabla\rho + \rho\,\operatorname{div}X$$ and using the chain rule along $$x_t$$,

$$
\frac{d}{dt}\rho_t(x_t) = \partial_t\rho_t + X_t\cdot\nabla\rho_t = -\rho_t\,\operatorname{div}X_t,
\qquad\text{i.e.}\qquad
\frac{d}{dt}\log\rho_t(x_t) = -\operatorname{div}X_t(x_t).
$$

The density a particle carries changes at a rate set entirely by the divergence where it is. If the flow is expanding there, the density thins. If it is compressing, the density concentrates. Two consequences:

- **Divergence-free flows preserve volume.** If $$\operatorname{div}X = 0$$, then $$\det D\theta_t \equiv 1$$, and areas or volumes are preserved. The pendulum field $$X = y\,\partial_x - \sin x\,\partial_y$$ has $$\operatorname{div}X = \partial_x(y) + \partial_y(-\sin x) = 0$$, which is why every blob in the §2.2 figure had area $$0.385$$. This is a special case of **Liouville's theorem**: every Hamiltonian flow preserves phase-space volume (Arnold, *Mathematical Methods of Classical Mechanics*).
- **Linear flows send Gaussians to Gaussians.** For $$X = Ax$$ the flow is $$e^{tA}$$, and a Gaussian $$\mathcal{N}(m, \Sigma)$$ is carried to $$\mathcal{N}\big(e^{tA}m,\ e^{tA}\Sigma\, e^{tA^\top}\big)$$. The density height scales like $$1/\sqrt{\det\Sigma_t} = e^{-t\,\mathrm{tr}A}/\sqrt{\det\Sigma_0}$$, and $$\mathrm{tr}A = \operatorname{div}X$$.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/blogs/manifold4/density_transport.png" class="img-fluid rounded z-depth-1" zoomable=true width="100%" %}
    </div>
</div>
<div class="caption">
    The same Gaussian pushed forward by two linear flows. Top: rotation, $$\operatorname{div} = 0$$. The blob turns rigidly and its peak density stays at $$1.59$$. Bottom: contraction toward the origin, $$\operatorname{div} = -2$$. The peak grows like $$e^{2t}$$ ($$1.59 \to 3.5 \to 7.9 \to 17.4$$).
</div>

### 4.4] Many vector fields, one density path

Here is a subtlety that matters a great deal for generative modeling. The continuity equation determines $$\rho_t$$ from $$X_t$$, but **not** $$X_t$$ from $$\rho_t$$. If $$w_t$$ satisfies $$\nabla\cdot(\rho_t\, w_t) = 0$$, then $$X_t$$ and $$X_t + w_t$$ produce exactly the same density path.

A clean example: let $$\rho$$ be the standard Gaussian on $$\mathbb{R}^2$$, so $$\nabla\rho = -\rho\,x$$, and let $$w = W = (-y, x)$$ be the rotation field. Then

$$
\nabla\cdot(\rho W) = \nabla\rho\cdot W + \rho\,\operatorname{div}W = -\rho\,(x, y)\cdot(-y, x) + 0 = 0.
$$

So "nothing moves" ($$X = 0$$) and "everything spins" ($$X = W$$) give the identical, constant density path $$\rho_t \equiv \rho$$. The Eulerian observer sees no difference. The particles, however, are doing completely different things.

How do we pick one field among so many? A natural rule is to choose the field with the **least kinetic energy** $$\int |X_t|^2\rho_t\,dx$$. Decompose $$X_t = \nabla\varphi_t + w_t$$ with $$\nabla\cdot(\rho_t w_t) = 0$$ (such a decomposition exists under mild conditions). The two pieces are orthogonal in $$L^2(\rho_t)$$, because integrating by parts gives

$$
\int \nabla\varphi\cdot w\,\rho\,dx = -\int \varphi\,\nabla\cdot(\rho w)\,dx = 0,
$$

hence

$$
\int |X_t|^2\rho_t = \int |\nabla\varphi_t|^2\rho_t + \int |w_t|^2\rho_t \ \ge\ \int |\nabla\varphi_t|^2\rho_t .
$$

So the most economical field is a **gradient**. Minimizing this kinetic energy over the whole path is the Benamou–Brenier formulation of optimal transport, and its minimizers are gradient fields (Villani, *Topics in Optimal Transportation*; Santambrogio, *Optimal Transport for Applied Mathematicians*). Flow-matching models learn a vector field $$X_t$$ that carries noise $$\rho_0$$ to data $$\rho_1$$. The freedom we just found is exactly why different training recipes can produce different particle paths while matching the same marginals. That story continues in the *Flow of Time* series.

### 4.5] Gradient fields, noise, and a useful identity

One last example ties this to sampling. Take $$X = -\nabla V$$, which pushes particles downhill on a potential $$V$$. Adding Brownian noise gives Langevin dynamics, $$dx_t = -\nabla V(x_t)\,dt + \sqrt{2}\,dB_t$$, whose density obeys the **Fokker–Planck equation**

$$
\partial_t\rho_t = \nabla\cdot(\rho_t\nabla V) + \Delta\rho_t.
$$

Using $$\Delta\rho = \nabla\cdot(\nabla\rho) = \nabla\cdot(\rho\,\nabla\log\rho)$$, this can be written as

$$
\partial_t\rho_t + \nabla\cdot\Big(\rho_t\,\underbrace{\big(-\nabla V - \nabla\log\rho_t\big)}_{X_t}\Big) = 0,
$$

which is a continuity equation with a *deterministic*, density-dependent vector field. At the level of distributions, the noise can be traded for the extra drift $$-\nabla\log\rho_t$$, which is minus the score. Diffusion models exploit exactly this when they replace a stochastic sampler with a "probability flow ODE". It also shows when Langevin dynamics stops: when $$\rho \propto e^{-V}$$, so that $$\nabla\log\rho = -\nabla V$$, the field $$X_t$$ vanishes and the distribution stays put.

---

## 5] Summary

| Idea | One-line version | Where it shows up next |
|---|---|---|
| Vector field | smooth section of $$TM$$, or a derivation of $$C^\infty(M)$$ | everywhere |
| Integral curve | solution of the ODE $$\dot\gamma = X(\gamma)$$ | dynamical systems, neural population dynamics |
| Flow | one-parameter group of diffeomorphisms generated by $$X$$ | Lie groups (the exponential map), Hamiltonian mechanics |
| Zeros and linearization | eigenvalues at equilibria are intrinsic | Poincaré–Hopf, stability, jPCA |
| Lie derivative and bracket | differentiate $$Y$$ along the flow of $$X$$; measures non-commuting flows | Lie algebras, torsion, Frobenius theorem |
| Divergence | $$\mathcal{L}_X\mu = (\operatorname{div}X)\,\mu$$, the volume expansion rate | continuity and Fokker–Planck equations |
| Continuity equation | $$\partial_t\rho + \nabla\cdot(\rho X) = 0$$ | optimal transport, flow matching, diffusion models |

The thread running through the post is that a vector field is a single object seen at three scales: an arrow at each point, a motion of the whole space, and a motion of everything living on that space, functions, other vector fields, metrics and probability distributions. Next in the series, the Lie group post takes the flows of §2 and the brackets of §3 and asks what happens when the manifold is itself a group.

---

## References

- J. M. Lee, *Introduction to Smooth Manifolds*, 2nd ed. Ch. 3 (tangent bundle), Ch. 8 (vector fields, derivations, $$F$$-related fields, Lie brackets), Ch. 9 (integral curves, flows, the fundamental theorem on flows, completeness, canonical form near a regular point, Lie derivatives, commuting vector fields).
- L. W. Tu, *An Introduction to Manifolds*, 2nd ed. Vector fields, integral curves and the Lie bracket, with many computational exercises.
- J. W. Milnor, *Topology from the Differentiable Viewpoint*. The Poincaré–Hopf theorem and the index of a vector field.
- S. H. Strogatz, *Nonlinear Dynamics and Chaos*. Phase portraits, linearization and the fragility of centers.
- V. I. Arnold, *Mathematical Methods of Classical Mechanics*. Liouville's theorem and phase flows.
- C. Villani, *Topics in Optimal Transportation*, and F. Santambrogio, *Optimal Transport for Applied Mathematicians*. The continuity equation and the Benamou–Brenier formulation.
