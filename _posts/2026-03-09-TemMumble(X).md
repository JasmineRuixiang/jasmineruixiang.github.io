---
layout: post
title: 'The Flow of Time: Prob/Measure/Transport/Generative Mumble(X): Entropy and Fisher Information (in progress)'
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

## 1] Entropy 

### 1.1] Entropy Definitions
In this sectio, I mean to clarify the relationships between **entropy**, **negative entropy**, and their convexity properties under different interpolations of probability distributions.

#### 1.1.1] Discrete Shannon Entropy

For a discrete distribution:

$$
H(p) = - \sum_i p_i \log p_i
$$

This is the original **Shannon entropy**.

---

#### 1.1.2] Differential Entropy (Continuous Case)

For a continuous density $$p(x)$$:

$$
H(p) = -\int p(x)\log p(x)\,dx
$$

This is called **differential entropy**.

Important properties:

- It **depends on the coordinate system**
- It can be **negative**
- It is **not invariant under reparameterization**

Note that this is not the continuous analogue of the discrete entropy (Shannon entropy). 

---

#### 1.1.3] Boltzmann / Gibbs Entropy

In statistical physics:

$$
S = k_B \log W
$$

where $$W$$ is the number of microstates.

In distribution form (Gibbs entropy):

$$
S = -k_B \int p(x)\log p(x)\,dx
$$

Thus, up to the constant $$k_B$$, this is the same functional as differential entropy.

---

#### 1.1.4] Negative Entropy

Negative entropy is defined as

$$
F(p) = \int p(x)\log p(x)\,dx
$$

So

- **Entropy** is concave
- **Negative entropy** is convex

---

#### 1.1.5] Relative Entropy

Relative entropy is also frequently known as and referred to by the name of Kullback Leibler (KL) Divergence:

$$
D_{KL}(p|q) = \int p(x)\log\frac{p(x)}{q(x)}dx
$$

This should be interpreted as the continuos counterpart of the discrete Shannon entropy. They are both scale invariant. I will elaborate this below. 

---

### 1.2] Mixture Interpolation of Distributions

Given two distributions $$p$$ and $$q$$, their **mixture interpolation** is

$$
\rho_t = (1-t)p + tq, \quad t \in [0,1].
$$

Interpretation:

- With probability $$1-t$$, sample from $$p$$
- With probability $$t$$, sample from $$q$$

This is a **straight line in the linear space of densities**.

---

### 1.3] Convexity Under Mixtures

#### 1.3.1] Negative Entropy is Mixture Convex

Remember the definition of the negative entropy:

$$
F(\rho) = \int \rho(x)\log \rho(x)\,dx
$$

For mixture interpolation

$$
\rho_t = (1-t)\rho_0 + t\rho_1
$$

we have

$$
F(\rho_t)
\le
(1-t)F(\rho_0) + tF(\rho_1).
$$

Therefore

**Negative entropy is convex under mixtures.**

The reason is that $$ \phi(x)=x\log x $$ is a convex function.

---

#### 1.3.2] Entropy is Mixture Concave

Differential entropy is

$$
H(\rho) = -\int \rho\log\rho.
$$

Since it is the negative of a convex functional, then

$$
H(\rho_t)
\ge
(1-t)H(\rho_0) + tH(\rho_1).
$$

Thus **Differential entropy is concave under mixtures.**

---

### 1.4] Why Differential Entropy Depends on Coordinates

Now we have a general feel about different kinds of entropy and its convexity properties, you might have a natural question as to why the differential entropy is not a continuous counterpart of the discrete Shannon entropy. Afterall, it seems to be just taking limits. Also, what does it mean to say that differential entropy is not invariant under change of variables. Let's investigate into this a little bit more. 

Let

$$
y = g(x).
$$

The density transforms according to the Jacobian:

$$
p_Y(y) = p_X(x)\left|\frac{dx}{dy}\right|.
$$

Computing the entropy after transformation gives (this is not hard; consider as a little exercise for yourself)

$$
H(Y)
= H(X) + \mathbb{E}[\log |g'(X)|].
$$

Thus differential entropy **changes under reparameterization**.

For example, let

$$
X \sim \text{Uniform}(0,1).
$$

Then, perhaps a little surprisingly, 

$$
H(X)=0.
$$

Define $$ Y=2X$$, then $$ Y\sim\text{Uniform}(0,2)$$ and thus

$$
H(Y)=\log 2
$$

This demonstrates that simply **changing the scale** changes differential entropy.

Why does this happen? Well, differential (continuous) entropy involves densities:

$$
p(x)=\frac{dP}{dx}.
$$

Since the base measure $$dx$$ depends on the coordinate system, entropy does too.

In other words, differential entropy is really **entropy relative to the Lebesgue measure**.

---

### 1.5] Coordinate-Invariant Quantities

The good news is, quantities involving **density ratios** remain invariant. For example: **KL divergence**

$$
D_{KL}(p||q) = \int p(x)\log\frac{p(x)}{q(x)}dx.
$$

Under coordinate change, Jacobians cancel:

$$
\frac{p_Y(y)}{q_Y(y)} = \frac{p_X(x)}{q_X(x)}.
$$

Thus, **KL divergence is coordinate invariant.**

---

### 1.6] Mixture Geometry vs Transport Geometry

Mixture interpolation does not move mass.

Example:

- $$p$$: Gaussian centered at $$-5$$
- $$q$$: Gaussian centered at $$5$$

Mixture interpolation produces **two bumps**, because the distributions coexist, but physical transport should produce **one bump moving across space**.

I also discussed this difference in the previous blog XX. 

In optimal transport, particles move. If $$T(x)$$ is the optimal transport map, then

$$
x_t = (1-t)x + tT(x).
$$

The density evolves via

$$
\rho_t = ((1-t)\mathrm{Id}+tT)_\# \rho_0.
$$

This path is the **Wasserstein geodesic**.

---

### 1.7] Displacement Convexity
The subjects of the following two sections have been discussed in the previous blog and I will expand them in the next post. For now, as a review and also to reinforce our interpreation, please allow me to state the findings again: 

McCann (1997) proved that the functional (negative entropy)

$$
U(\rho) = \int \rho\log\rho
$$

is convex along Wasserstein geodesics:

$$
U(\rho_t)
\le
(1-t)U(\rho_0) + tU(\rho_1).
$$

Thus, **negative entropy is displacement convex.**

Since entropy is the negative of this functional,

$$
H(\rho) = -U(\rho),
$$

we obtain that **entropy is displacement concave.**

In summary, differnetial entropy is both mixture concave and displacement concave, and negative entropy is convex in both concepts. 

---

### 1.8] Why Displacement Convexity Matters

Mixture interpolation corresponds to **randomly choosing distributions**, where optimal transport corresponds to **moving mass in space**. 

This might sound trivial, but many physical and probabilistic dynamics follow transport geometry.

For example, the **Fokker–Planck equation**

$$
\partial_t \rho = \nabla\cdot(\rho\nabla V)+\Delta\rho
$$

Jordan–Kinderlehrer–Otto (1998) showed:

> This PDE is the **gradient flow of entropy in Wasserstein space**.

This connects optimal transport, diffusion equations, statistical physics, and Riemannian geometry into a unified framework.

---

## 2] Fisher Information 
