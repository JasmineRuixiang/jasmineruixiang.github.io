---
layout: post
title: 'Hillbert Space and Spectral Theorem (in progress)'
date: 2026-03-19 01:39:16
description: Linear/Normed/Banach space, Hillbert space, SVD, Projection and Spectral Theorem
series: Miscellaneous
tags: 
    - "Geometry Concepts/Tools"
    - "Topology Concepts/Tools"
categories: 
    - "Differential Geometry"
    - "Topology"
featured: false
related_posts: true
related_publications: true
citation: true
math: true
toc:
  beginning: true
  sidebar: left
---

## 0] Introduction

We have natural, intuitive, and simple geometry in Euclidean space. Could such geometry be generalized into studying functions? Functional analysis, to some extent, is the geometry on functions in the corresponding infinite dimensional space, the Hillbert space. 

What we aim to still aclaim, is properties such as length and angles, the definition/meaning of orthogonality, and certain decompositions. Consequently, there's the need for inner product, the definition of norms, projection theorem and Spectral Theorem. However, if we do this in a cavalier way, in such infinite dimensional space, limit or convergence might work apriori. Hence we need to design such a space. 


---

## 1] Linear Space
Only addition and multiplication 

### 1.1] Definition

---

### 1.2] Singular Value Decomposition (SVD)

---

## 2] Normed Space
Definition of norm/length. 

---

## 3] Banach Space
Core: Completeness. 

---

## 4] Hillbert Space
All the following (it's a Banach space: linear, normed, and complete), but also with an inner product. 

Key properties: 
* linear/vector space,
* normed,
* complete,
* with an inner product 

---

### 4.1] Linear/Vector Space
The existence of addition and scalar

Also remind us of ring: $$\mathcal{X}(M)$$ is a module on the ring $$\mathcal{C}^{\infty}(M)$$, given $$M$$ a smooth manifold. 

---

### 4.2] Inner Product
This property is the core of Hillbert space, which allows us to define length and angle. If we step back a little bit, it's not apriori clear what it means to call that this function $$f$$ is "shorter" than the other function $$g$$, or if $$f$$ is perpendicular to $$g$$. 

Remind us of the Riemannian metric. In the end, that's exactly what inner product does. 

The chosen inner product:



Practically, this inner product gives us the length (norm) of a function and a way to measure if two functions are orthogonal.

---

### 4.3] Completeness
Now with the above nice properties, we could do normal computation (addition and scaling) while applying certain geometric intuition on it, however there's still the risk of "falling" into some holes withint this space. 

Completeness ensures that the limit we take for sequences of functions still fall within this space. 
 
Some examples on incompleteness:
* Rational numbers $$\mathbb{Q}$$ vs real numbers $$\mathbb{R}$$

Why do we need this completeness property? A well-known fact is that not all Cauchy sequence of $$\mathcal{C}^1$$ functions are continuous (Riemannian integral). That means ths space defined this way has some holes in it, which is not ideal. 

However, if we use $$\mathbb{L}^2$$ (square integrable) with Lebesgue integral, then doing computations on limits we will still stay in this space. Basically, completeness ensures that there's no hole in our space. 

Lebesgue integral: equivalence class

Discussion on whether mathematics is discovered or invented. 

---

### 4.4] Examples

Common space such as (finite) $$\mathbb{R}^n, \mathbb{C}^n$$, or $$\mathcal{l}^2 = \{x_n \mid \sum_n |x_n|^2 < \infty \}$$, or $$L^2(\Omega) = \{f \mid \int |f|^2 d\mu < \infty \}$$ (this is Lebesgue integration, not Rieman integral)

---

### 4.5] Orthogonality and Projection Theorem

If $$<x, y> = 0$$, then the following two theorems still hold:

* 1) Pythagorean theorem still hold:

$$
||x + y||^2 = ||x||^2 + ||y||^2
$$

* 2) Parallelogram theorem still hold:

$$
||x + y||^2 + ||x - y||^2 = 2||x||^2 + 2||y||^2
$$

Projection Theorem: 
> Given $$x \in H, H$$ the Hillbert space, within a closed subspace $$K \subset H$$ there exists a unique element $$p \in K$$ such that $$||x - p||$$ is the minimum. Moreover, $$H = K \oplus K^\perp, p = P_K(x)$$, where $$P$$ is the projection. 

Note that this is basis for least square methods in linear algebra. 

---

## 5] Operators

Intuition: function in function out;

Examples: integral operator, derivative operator;

Bounded linear operators: an extension of matrix algebra: from finite dimensions to infinite dimensions


### 4.1] Self-adjoint Operators

Physical intuition

---

### 4.2] Riesz Representation Theorem


---

## 5] Spectral Theorem

Intuition: Decomposition of operators

Discrete vs Continuous

### 5.1] Statement

* Resolution of Identity

The idea of basis: again, deeply echoeing linear algebra

Side note: Parseval's theorem actuall reveals a certain notion of energy conservation:

$$
||f||^2 = \sum_{i = 1}^\infty |\langle f, e_i \rangle|^2
$$

Also differetiate between Hamel basis (finite) and Schauder basis (infinite, the discussion of convergence depending upon definition of norm and completeness), and Bessel inequality (if basis incomplete, then "energy leak")

---

### 5.2] Examples 

#### 5.2.1] Fourier Transform
In Hillbert space, 



