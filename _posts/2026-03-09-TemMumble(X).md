---
layout: post
title: 'The Flow of Time: Prob/Measure/Transport/Generative Mumble(X): Entropy and Fisher Information'
date: 2026-03-10 23:36:29
description: Displacement Convextiy, Ricci Curvature 
series: The Flow of Time
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

**Fisher information** measures how much information an observable random variable carries about an unknown parameter of a statistical model. Intuitively, it quantifies **how sensitive the probability distribution is to changes in the parameter**. As the name indicates, this concept was introduced by Ronald A. Fisher.

### 2.1] Setup and Definition

Suppose

- A random variable $$X$$ has probability density (or mass) function  

$$
p(x|\theta)
$$

- $$\theta$$ is an unknown parameter we want to estimate.

The **score function** is

$$
s(x,\theta) = \frac{\partial}{\partial \theta} \log p(x|\theta)
$$

which measures how much the **log-likelihood changes with the parameter**.

The `Fisher information` is the variance of this score:

$$
I(\theta) = \mathbb{E}\left[\left(\frac{\partial}{\partial\theta}\log p(X|\theta)\right)^2\right]
$$

where the expectation is taken over $$X \sim p(x|\theta)$$.

An equivalent form (under regularity conditions) is

$$
I(\theta) =
-\mathbb{E}\left[
\frac{\partial^2}{\partial \theta^2}\log p(X|\theta)
\right]
$$

(I'll leave it for the readers to prove it)

---

### 2.3] Intuition of Fisher Information

Think of the log likelihood function (assume $$i.i.d$$ samples):

$$
\log (L(\theta)) = \log (p(X|\theta)) = \mathbb{E}\left[\log p(X|\theta)\right]
$$

and the folloiwng two scenarios:

* 1) Low Fisher information: The likelihood is **flat** with respect to $$\theta$$: small changes in $$\theta$$ barely change the distribution and thus data tells you little about the parameter.
* 2) High Fisher information: The likelihood is **sharp** around the true value: small changes in $$\theta$$ strongly change the distribution and thus data strongly determines $$\theta$$.

Consequently, Fisher information measures **parameter identifiability from data**.

---

### 2.3] Example: Normal Mean

Let

$$
X \sim N(\mu, \sigma^2)
$$

with known $$\sigma$$ and unknown $$\mu$$. The log likelihood is

$$
\log p(x|\mu)
= -\frac{(x-\mu)^2}{2\sigma^2} + C
$$

and the derivative:

$$
\frac{\partial}{\partial \mu} \log p(x|\mu)
= \frac{x-\mu}{\sigma^2}
$$

If we square and take expectation:

$$
I(\mu)
= \mathbb{E}\left[\left(\frac{x-\mu}{\sigma^2}\right)^2\right]
= \frac{1}{\sigma^4} \mathbb{E}[(x-\mu)^2]
= \frac{1}{\sigma^2}
$$

this means that 

$$
I(\mu)=\frac{1}{\sigma^2}
$$

Notice that this is consisten with our interpretation:

- Larger variance indicates less information
- Smaller variance represents more information

---

### 2.4] Multiple Samples

For $$n \;\; i.i.d.$$ samples:

$$
I_n(\theta) = n I(\theta)
$$

Information **adds linearly with independent data**.

---

### 2.5] Connection to Estimation (Cramér–Rao Bound)

Fisher information determines the **best possible accuracy of estimators** via the **Cramér–Rao bound**:

$$
\text{Var}(\hat{\theta}) \ge \frac{1}{I_n(\theta)}
$$

**More Fisher information leads to lower possible estimation variance**.

---

### 2.6] Geometric Interpretation (Information Geometry)

In **information geometry** (A major developer of this geometric viewpoint is Shun-ichi Amari), Fisher information defines a **Riemannian metric** on the space of probability distributions.

The metric tensor is

$$
g_{ij}(\theta)
= \mathbb{E}
\left[
\frac{\partial \log p}{\partial \theta_i}
\frac{\partial \log p}{\partial \theta_j}
\right]
$$

This is called the **Fisher information metric**. The geometry of statistical models (distances, geodesics, curvature) is built from this metric.

---

### 2.7] Conceptual Summary

Fisher information measures:

- **Sensitivity of the distribution to parameters**
- **Amount of information data carries about parameters**
- **Best possible precision of estimators**
- **Riemannian metric on the statistical manifold**

---

### 2.8] Fisher Information, Grigori Perelman, and Ricci Flow


