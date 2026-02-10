---
layout: post
title: 'High Dimensional Nitty-gritty (2): Z-scoring before PCA?'
date: 2026-02-09 01:22:40
description: Zscoring, covariance, PCA
tags: 
    - "Brain Computer Interface"
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

This blog originates from a daily discussion of neural signal (pre)processing with my mentors and peers. People utilize z-scoring and PCA all the time, and it's a little shameful to admit by hindsight that I haven't dwelled on the following question deep enough. Again, we reencounter the conundrum in high dimensional observations haunted by irreducible noise, under which lies our ambitious intent to extract robust and effective information. 

---

### 0] Problem Setup
Let's say we have a collection of neural data in the format $$X \in \mathbb{R}^{n \times d}$$, where $$n$$ is the number of samples, and $$d$$ the number of features. These are raw features each coming from a single electrode. For simplicity let's assume that there is only 1 kind of feature, like threshold crossing. Let's further say that we want to visualize the low dimensional structure of this collection of data, preserving its global geometry as much as possible (we'll clarify this later). 

Now we'd like to apply PCA on it for the first try, meaning to transform from $$\mathbb{R}^{n \times d}$$ into $$\mathbb{R}^{n \times d'}$$, where $$d'$$ might be just $$3$$, for example. Many methods would start by z-scoring $$X$$ for each feature (so for each column of $$X$$, after z-scoring would be mean 0 and standard deviation 1; for more discussions please refer to the [previous blog](https://jasmineruixiang.github.io/blog/2026/zscore/) of this series) before applying PCA. 

I'm a little unsure about the motivation behind it. There're of course many but I cannot pinpoint a conclusive answer.

More importantly, I'm naively concerned that if we apply this feature-wise normalization, whether that would still preserve the covariance matrix between the original features (the diagonal will be 1, but I'm wondering how the off-diagonal terms would possibly change, or ... would they). 

Let's begin. 

---

### 1] What PCA actually does
The very first step: a quick review of what PCA is doing:

> Standard PCA:
1. Center the data (subtracts column-wise/global means across samples; For the following, $$X$$ will denote the centered data, if without specifications) 
2. Compute the __covariance matrix__: $$\Sigma = \frac{1}{n}X^TX$$
3. Find eigenvectors of $$\Sigma$$

PCA finds directions of maximal variance in the original coordinate system. We could also interpret PCA as finding minimization of reconstruction error, but that's not explicitly helpful for deepening our interpretation here. However, I'll provide another useful perspective in section 3] from the angle of constrained optimization, but this covariance interpretation is what we will grapple with now for this section ---

Because it already makes it obvious that

> PCA is sensitive to feature scale.

If one electrode has variance 100 and another has variance 1, the first will dominate the principal components — even if its structure might not be more meaningful. And that's exactly where z-scoring makes a huge distinction. 

---

### 2] What does z-scoring do?
Column-wise z-scoring transforms

$$\tilde{X}_{ij} = \frac{X_{ij} - \mu_j}{\sigma_j}$$

(Here's a bit of abuse of notation, as $$\tilde{X}_{ij}, X_{ij}$$ are scalars while $$\mu_j, \sigma_j \in \mathbb{R}^{1\times d}$$)

If we use matrix notation, $$D = diag{(\sigma_1, \cdots, \sigma_n)}$$, then the above could be simplified into (again, assume it's already centered):

$$\tilde{X} = XD^{-1}$$

Now if we look at the new covariance matrix:

$$\tilde{\Sigma} = \frac{1}{n}\tilde{X}^T\tilde{X} = \frac{1}{n}D^{-1}X^TXD^{-1} = D^{-1}\Sigma D^{-1}$$

This is obviously __not the same__ covariance matrix.

In fact, it's not hard to see that, from simple linear algebra:

$$\tilde{\Sigma}_{ij} = \frac{\Sigma_{ij}}{\sigma_i \sigma_j}$$

And this turns out to be exactly the __correlation matrix__.

So:
> PCA on z-scored data = PCA on the `correlation matrix`

> PCA on raw centered data = PCA on the `covariance matrix`

So back to one of our original questions: does normalization preserve `covariance` between original features?

No. The off-diagonal terms change as:

$$\frac{\Sigma_{ij}}{\sigma_i \sigma_j} \leftarrow \Sigma_{ij}$$

So they become `correlations` (or we could say that the correlations are preserved). The important distinction is that `covariance` measures co-variation in physical units, while `correlation` measures co-variation relative to each variable's scale. So z-scoring does not preserve the original covariance geometry: It preserves the `correlation` _structure_ instead.

---

### 3] A geometric way to think about this

The above covariance calculation is clear, yet we could reinterpret PCA in a different way by variational characterization of eigenvectors, i.e. the Rayleigh quotient formulation of PCA (depending on your views, these two migth be considered the exact same thing; but even as an explanation for why we care about eigenvectors of the covariance, let me elaborate below). 

#### 3.1] PCA as a Variational Problem

Let $$X \in \mathbb{R}^{n \times d}$$ be the centered data and the sample covariance matrix 

$$
\Sigma = \frac{1}{n} X^\top X
$$

If we project the data onto a direction $$v \in \mathbb{R}^d$$, the projected variance is:

$$
\mathrm{Var}(Xv)
= \frac{1}{n} \|Xv\|^2
= v^\top \Sigma v
$$

Therefore, the first principal component solves:

$$
\max_{\|v\| = 1} v^\top \Sigma v
$$

This is the Rayleigh quotient, and the solution is the top eigenvector of $$\Sigma$$ (not proved here; many other sources exist online).

#### 3.2] PCA After Z-Scoring

Suppose we z-score each feature (column-wise normalization). Again, let me reiterate from the above that

$$
D = \mathrm{diag}(\sigma_1, \dots, \sigma_d)
$$

and the transformed data is:

$$
\tilde{X} = X D^{-1}
$$

The new covariance matrix becomes:

$$
\tilde{\Sigma}
= \frac{1}{n} \tilde{X}^\top \tilde{X}
= D^{-1} \Sigma D^{-1}
$$

PCA on z-scored data solves:

$$
\max_{\|v\| = 1}
v^\top D^{-1} \Sigma D^{-1} v
$$

---

#### 3.3] Equivalent Reformulation (Changing the Constraint)

Now let's do a simple trick. Let:

$$
w = D^{-1} v
\quad \text{so that} \quad
v = D w
$$

Substitute into the objective:

$$
v^\top D^{-1} \Sigma D^{-1} v
= (Dw)^\top D^{-1} \Sigma D^{-1} (Dw)
= w^\top \Sigma w
$$

Now examine the constraint:

$$
\|v\|^2 = 1
$$

$$
v^\top v
= (Dw)^\top (Dw)
= w^\top D^2 w
$$

So the optimization becomes:

$$
\max_{w^\top D^2 w = 1}
w^\top \Sigma w
$$

---

#### 3.4] Geometric Interpretation

The raw PCA solves:

$$
\max_{v^\top v = 1}
v^\top \Sigma v
$$

Now, the z-scored PCA solves:

$$
\max_{w^\top D^2 w = 1}
w^\top \Sigma w
$$

So at a glance from 2], z-scoring rescales the covariance matrix into the correlation matrix. 

But viewed from this different perspective, it __changes the metric constraint__. Instead of using the standard Euclidean norm:

$$
v^\top v
$$

we now use a weighted norm:

$$
w^\top D^2 w
$$

This means:

- Raw PCA assumes the __standard Euclidean__ inner product.
- Z-scored PCA uses a different inner product __induced by $$D^2$$__.

---

#### 3.5] Multi-Dimensional PCA (k Components)

Up to this point, you might object that the above is just to find one single direction/one principal component. Usually we do multiple components. Well, there isn't too much effort for an extension. 

Raw PCA solves:

$$
\max_{V^\top V = I}
\mathrm{Tr}(V^\top \Sigma V)
$$

where $$V \in \mathbb{R}^{d \times k}$$. The solution is the top-$$k$$ eigenvectors of $$\Sigma$$ (proof omitted, similar as 3.1]). As in 3.2], After z-scoring, we solve:

$$
\max_{V^\top V = I}
\mathrm{Tr}(V^\top D^{-1} \Sigma D^{-1} V)
$$

Using the substitution $$V = D W$$, this naturally becomes (similar to 3.3]):

$$
\max_{W^\top D^2 W = I}
\mathrm{Tr}(W^\top \Sigma W)
$$

---

#### 3.6] Generalized Eigenvalue Interpretation
What does this mean geometrically about the solution? 

* For the raw PCA: 
    * Orthonormal basis in standard __Euclidean__ metric
    * Maximizes variance
* For Z-scored PCA:
    * Orthonormal basis under __weighted__ metric $$D^2$$
    * This is equivalent to solving a __generalized eigenvalue__ problem:

$$
\Sigma w = \lambda D^2 w
$$

I'll also skip the details of why the solution is equivalent to finding the generalized eigenvalues/eigenvectors. However, this fact informs us of the fundamental framework of PCA:

the big geometric insight is that PCA always solves:

$$
\max_{W^\top G W = I}
\mathrm{Tr}(W^\top \Sigma W)
$$

where $$G$$ defines the metric.

* Raw PCA: $$G = I$$
* Z-scored PCA: $$G = D^2$$

So z-scoring means that we are not trusting that Euclidean length in raw coordinates is meaningful. We redefine what unit length means.


Which is consistent with the previous observation that

- Raw PCA preserves covariance geometry.
- Z-scored PCA preserves correlation geometry.

---

### 4] What geometry are we preserving?

To some extent, this is the real conceptual issue. 

If we do PCA without z-scoring, it preserves the Euclidean geometry in the original feature space. Variance magnitude is meaningful because we indeed keep such information. We'd hold the underlying premise that 

> Electrodes with larger variance are considered more important.

This suggests that this methood is good if variance magnitude reflects real neural signal strength or the feature scale is physically meaningful.

On the other hand, if we do PCA after z-scoring, then it would preserve geometry under a __reweighted metric__ and all dimensions are treated equally. Each electrode is thus given equal prior importance.

This should work if variance differences are arbitrary (e.g., electrode gain differences) and we care about patterns of co-variation, not absolute magnitude.

Since neural data often has:

* Different firing rates across electrodes
* Different noise levels
* Different dynamic ranges

If we don’t z-score:

> High firing-rate neurons dominate PCA.

whereas if we do z-score:

> Each neuron contributes equally in variance units.

---

### 5] Which one preserves global geometry?

This depends on what geometry we think is meaningful.

If our raw space is: $$\mathbb{R}^d$$ with standard Euclidean metric, then PCA without z-scoring preserves global geometry better. If we believe that true geometry should not depend on firing rate scale, then z-scoring defines a more appropriate metric (as elaborated in section 3]):

$$<x, y>_D = x^T(D^{-1})^{2}y$$

which means we could alternatively interpret this as keeping the underlying space unchanged but essentially altering the metric before doing PCA. 

Usually in systems neuroscience people often z-score across time and then do PCA. The reason behind is that neural manifold studies often care about relative population patterns, not which neuron fires more. If we want true population variance magnitude, then we should not z-score. If we intend to obtain population structure independent of scale, then z-score. 

---

### 6] Summary
In conclusion, if we do z-scoring before PCA, then 

* Z-scoring does NOT preserve the `covariance` matrix.
* It converts `covariance` to `correlation`.
* Off-diagonal terms are divided by product of standard deviations.
* Equivalently, we are changing the __metric__ of the space.
* PCA result can change dramatically depending on scaling.

Finally, in practice we often have more than one kind of features. For example, we could obtain both threshold crossings and spike band power from each electrode at the same time. However, these two measures have drastically different scales. In this scenario, of course we could look into them separately, but if combined, the spike power would dominate. Consequently, z-scoring also helps to re-weight the feature importance apriori.  


