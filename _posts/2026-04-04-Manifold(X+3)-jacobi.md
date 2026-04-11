---
layout: post
title: "Manifold and Riemannian Geometry (X+3): Jacobian Field"
date: 2026-04-04 10:22:42
description: Exploration of Jacobian field
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

## 1] Revisit the Exponential Map

# Differential of the Exponential Map — Complete Discussion (With and Without Jacobi Fields)

This document compiles **everything discussed** about the differential of the exponential map:
- Interpretation of \( (d\exp_q)_w \)
- Computation via **Jacobi fields**
- Computation **without Jacobi fields**
- Proof of the identity
  $$
  \langle (d\exp_p)_v(v), (d\exp_p)_v(w_T)\rangle = \langle v, w_T\rangle
  $$

---

# Part I — Conceptual Setup

Let \( (M,g) \) be a Riemannian manifold with the Levi-Civita connection.

The exponential map at \( q \in M \) is:
$$
\exp_q(v) = \gamma_v(1),
$$
where \( \gamma_v \) is the geodesic satisfying:
$$
\begin{cases}
\nabla_{\dot{\gamma}} \dot{\gamma} = 0, \\
\gamma(0)=q,\quad \dot{\gamma}(0)=v.
\end{cases}
$$

We want to understand:
$$
(d\exp_q)_w : T_qM \to T_{\exp_q(w)}M.
$$

---

# Part II — Interpretation of \( (d\exp_q)_w \)

For \( u \in T_qM \),
$$
(d\exp_q)_w(u)
= \left.\frac{d}{ds}\right|_{s=0} \exp_q(w + s u)
= \left.\frac{d}{ds}\right|_{s=0} \gamma_{w+su}(1).
$$

**Interpretation:**
- Start a geodesic with initial velocity \( w \)
- Perturb the initial velocity by \( u \)
- Measure how the endpoint at time \( t=1 \) changes

So:
> \( d\exp \) measures **sensitivity of geodesic endpoints to initial velocity perturbations**

---

# Part III — Jacobi Field Formulation

Define a 2-parameter variation:
$$
F(s,t) = \exp_q(t(w + s u)) = \gamma_{w+su}(t).
$$

Then:
- For each \( s \), \( t \mapsto F(s,t) \) is a geodesic
- Define:
$$
J(t) := \left.\frac{\partial F}{\partial s}\right|_{s=0}
$$

### Key fact:
\( J(t) \) is a **Jacobi field** along \( \gamma_w(t) \)

### Initial conditions:
$$
J(0) = 0,\quad \nabla_t J(0) = u
$$

### Jacobi equation:
$$
\nabla_t^2 J + R(J, \dot{\gamma})\dot{\gamma} = 0
$$

### Fundamental identity:
$$
(d\exp_q)_w(u) = J(1)
$$

---

# Part IV — Interpretation via Jacobi Fields

- \( d\exp \) is governed by curvature
- The Jacobi equation determines how nearby geodesics spread

### Special case: flat space
If \( R=0 \), then:
$$
J(t) = t u \quad \Rightarrow \quad (d\exp_q)_w(u) = u
$$

### General case:
Curvature introduces deviation from linear behavior

---

# Part V — Radial vs Transverse Decomposition

Fix \( p \in M \), \( v \in T_pM \). Decompose:
$$
w = w_\parallel + w_T,
$$
where:
- \( w_\parallel \parallel v \)
- \( w_T \perp v \)

This decomposition is fundamental.

---

# Part VI — Radial Direction (Jacobi View)

Consider scaling:
$$
\gamma_{(1+s)v}(t) = \gamma_v((1+s)t)
$$

Thus:
$$
(d\exp_p)_v(v) = \dot{\gamma}_v(1)
$$

Interpretation:
- Radial variation = reparametrization of the same geodesic

---

# Part VII — Transverse Direction (Jacobi View)

Let \( J_T \) be the Jacobi field with:
$$
J_T(0)=0,\quad \nabla_t J_T(0)=w_T
$$

Then:
$$
(d\exp_p)_v(w_T) = J_T(1)
$$

---

# Part VIII — Key Identity via Jacobi Fields

Define:
$$
\psi(t) := \langle J_T(t), \dot{\gamma}(t) \rangle
$$

Compute:
$$
\frac{d^2}{dt^2} \psi(t) = 0
\quad \Rightarrow \quad \psi(t) \text{ is affine in } t
$$

### Initial conditions:
$$
\psi(0) = 0
$$
$$
\psi'(0) = \langle w_T, v \rangle
$$

Thus:
$$
\psi(t) = t \langle w_T, v \rangle
$$

Evaluate at \( t=1 \):
$$
\langle J_T(1), \dot{\gamma}(1) \rangle = \langle w_T, v \rangle
$$

So:
$$
\boxed{
\langle (d\exp_p)_v(v), (d\exp_p)_v(w_T)\rangle
= \langle v, w_T\rangle
}
$$

---

# Part IX — Derivation Without Jacobi Fields

We now redo everything without Jacobi fields.

---

## 1. Two-parameter variation

Define:
$$
F(s,t) = \gamma_{v+sw_T}(t)
$$

Then:
$$
(d\exp_p)_v(w_T)
= \left.\frac{\partial F}{\partial s}\right|_{s=0,\,t=1}
$$

---

## 2. Known expressions

- Radial:
$$
(d\exp_p)_v(v) = \dot{\gamma}(1)
$$

- Transverse:
$$
(d\exp_p)_v(w_T) = \partial_s F(0,1)
$$

---

## 3. Define key quantity

$$
\Phi(t) := \left\langle \frac{\partial F}{\partial s}(0,t), \frac{\partial F}{\partial t}(0,t) \right\rangle
$$

We want:
$$
\Phi(1)
$$

---

## 4. Differentiate \( \Phi(t) \)

Using:
- metric compatibility
- torsion-free property

$$
\frac{d}{dt} \Phi(t)
= \left\langle \nabla_t \partial_s F, \partial_t F \right\rangle
+ \left\langle \partial_s F, \nabla_t \partial_t F \right\rangle
$$

Use:
$$
\nabla_t \partial_t F = 0
$$
$$
\nabla_t \partial_s F = \nabla_s \partial_t F
$$

So:
$$
\frac{d}{dt} \Phi(t)
= \left\langle \nabla_s \partial_t F, \partial_t F \right\rangle
$$

---

## 5. Use metric compatibility

$$
\left\langle \nabla_s \partial_t F, \partial_t F \right\rangle
= \frac{1}{2} \partial_s \langle \partial_t F, \partial_t F \rangle
$$

---

## 6. Energy conservation

$$
\langle \partial_t F, \partial_t F \rangle = \|v + s w_T\|^2
$$

Thus:
$$
\partial_s \langle \partial_t F, \partial_t F \rangle
= 2 \langle v, w_T \rangle
$$

So:
$$
\frac{d}{dt} \Phi(t) = \langle v, w_T \rangle
$$

---

## 7. Integrate

$$
\Phi(t) = t \langle v, w_T \rangle + C
$$

At \( t=0 \):
$$
\Phi(0)=0 \Rightarrow C=0
$$

Thus:
$$
\Phi(1) = \langle v, w_T \rangle
$$

---

## 8. Final result

$$
\boxed{
\langle (d\exp_p)_v(v), (d\exp_p)_v(w_T)\rangle
= \langle v, w_T\rangle
}
$$

---

# Part X — Geometric Interpretation

- Radial variation:
  - same geodesic, different speed
- Transverse variation:
  - different geodesics

The Levi-Civita structure ensures:
- energy conservation
- symmetry of mixed derivatives

This leads to:

> Preservation of inner product between radial and transverse directions under \( \exp \)

---

# Part XI — Big Picture

- \( d\exp \) encodes curvature
- Jacobi fields give the most precise description
- Without Jacobi fields, the result follows from:
  - geodesic equation
  - metric compatibility
  - torsion-free property
  - conservation of energy

---

# Part XII — Key Takeaways

- \( (d\exp_q)_w(u) \) = endpoint sensitivity of geodesics
- Radial direction behaves trivially
- Transverse directions encode curvature
- Mixed inner product is preserved:
$$
\langle (d\exp)_v(v), (d\exp)_v(w_T)\rangle = \langle v, w_T\rangle
$$

---

# Part XIII — Possible Extensions

- Series expansion of \( d\exp \) involving curvature tensor
- Volume distortion via \( \det(d\exp) \)
- Behavior in normal coordinates