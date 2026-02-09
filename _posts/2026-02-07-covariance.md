---
layout: post
title: 'BCI nitty-gritty (2): Zscoring before PCA?'
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

This blog originates from a daily discussion of neural signal (pre)processing with my mentor and peers. We all utilize z-scoring and PCA all the time, and it's a little shameful to admit by hindsight that I haven't dwelled on the following problem deep enough. Again, high dimensional observations haunted by irreducible noise, under which our ambitious intent to extract robust and effective information. 

### 0] Problem Setup
Let's say we have a collection of neural data in the format $$X \in \mathbb{R}^{n \times d}$$, where $$n$$ is the number of samples, and $$d$$ the number of features. These are raw features each coming from a single electrode. For simplicity let's assume that there is only 1 kind of feature, like threshold crossing. Let's further say that we want to visualize the low dimensional structure of this collection of data, preserving its global geometry as much as possible (we'll clarify this later). 

Now we'd like to apply PCA on it for the first try, meaning to transform from $$\mathbb{R}^{n \times d}$$ into $$\mathbb{R}^{n \times d'}$$, where $$d'$$ might be just $$3$$, for example. Many methods would start by z-scoring $$X$$ for each feature (so for each column of $$X$$, after z-scoring would be mean 0 and standard deviation 1; more discussion see the [previous blog](https://jasmineruixiang.github.io/blog/2026/zscore/) of this series) before applying PCA. 

I'm a little unsure about the motivation behind it. There're many but what's the conclusive answer?

More importantly, I'm naively concerned that if we apply this feature-wise normalization, whether that would still preserve the covariance matrix between the original features (the diagonal will be 1, but I'm wondering how the off-diagonal terms would change, or would they). 

Let's begin. 

---

### 1] What PCA actually does
The very first step: a quick review of what PCA is doing, shall we.

> Standard PCA:
1. Center the data (subtracts column-wise/global means across samples; For the following, $$X$$ will denote the centered data, if without specifications) 
2. Compute the covariance matrix: $$\Sigma = \frac{1}{n}X^TX$$
3. Find eigenvectors of $$\Sigma$$

So PCA finds directions of maximal variance in the original coordinate system. We could also interpret PCA as finding minimization of reconstruction error, but that's explicitly helpful for interpretation here. However, I'll provide another useful interpretation in section 3] from the perspective of constrained optimization, but this covariance interpretation is what we will grapple with now for this section ---

Because it already makes it obvious that

> PCA is sensitive to feature scale.

If one electrode has variance 100 and another has variance 1, the first will dominate the principal components — even if its structure is not more meaningful.

---

### 2] What does z-scoring do?
Column-wise z-scoring transforms

$$\tilde{X}_{ij} = \frac{X_{ij} - \mu_j}{\sigma_j}$$

If we use matrix notation, $$D = diag{(\sigma_1, \cdots, \sigma_n)}$$, then the above could be simplified into (again, assume it's already centered):

$$\tilde{X} = XD^{-1}$$

Now if we look at the new covariance matrix:

$$\tilde{\Sigma} = \frac{1}{n}\tilde{X}^T\tilde{X} = \frac{1}{n}D^{-1}X^TXD^{-1} = D^{-1}\Sigma D^{-1}$$

This is obviously not the same covariance matrix.

In fact, it's not hard to see that, from simple linear algebra:

$$\tilde{\Sigma}_{ij} = \frac{\Sigma_{ij}}{\sigma_i \sigma_j}$$

And this is exactly the correlation matrix.

So:
> PCA on z-scored data = PCA on the correlation matrix

> PCA on raw centered data = PCA on the covariance matrix

So back to one of our original questions: does normalization preserve covariance between original features?

No. The off-diagonal terms change as:

$$\Sigma_{ij} \leftarrow \frac{\Sigma_{ij}}{\sigma_i \sigma_j}$$

So they become correlations (or we could say that the correlations are preserved). The important distinction is that _covariance_ measures co-variation in physical units, while _correlation_ measures co-variation relative to each variable's scale. So z-scoring does not preserve the original covariance geometry: It preserves the __correlation structure__ instead.

But this leads to the next natural question, or the question ---

---

### 3] A geometric way to think about this

The above covariance calculation is clear, yet we could reinterpret PCA in a different way by variational characterization of eigenvectors, i.e. the Rayleigh quotient formulation of PCA. Let me explain below. 







### 4] What geometry are we preserving?

To some extent, this is the real conceptual issue. 

If we do PCA without z-scoring, it preserves the Euclidean geometry in the original feature space. Variance magnitude is meaningful because we indeed keep such information. We'd hold the underlying premise that 

> Electrodes with larger variance are considered more important.

This suggests that this methood is good if variance magnitude reflects real neural signal strength or the feature scale is physically meaningful.

On the other hand, if we do PCA after z-scoring, then it would preserves geometry under a reweighted metric and all dimensions are treated equally. Each electrode is thus given equal prior importance.

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

If our raw space is: $$\mathbb{R}^d$$ with standard Euclidean metric, then PCA without z-scoring preserves global geometry better. 

If we believe that true geometry should not depend on firing rate scale, then z-scoring defines a more appropriate metric:

$$<x, y>_D = x^T(D^{-1})^{2}y$$

which means we could alternatively interpret this as keeping the underlying space unchanged but essentially altering the metric before doing PCA. 

Usually in systems neuroscience people often z-score across time and then do PCA. The reason behind is that neural manifold studies often care about relative population patterns, not which neuron fires more

If we want true population variance magnitude, then we should not z-score. If we intend to obtain population structure independent of scale, then z-score. 

---

### 6] Summary
In conclusion, if we do z-scoring before PCA, then 

* Z-scoring does NOT preserve the covariance matrix.
* It converts covariance to correlation.
* Off-diagonal terms are divided by product of standard deviations.
* Equivalently, we are changing the metric of the space.
* PCA result can change dramatically depending on scaling.

Finally, in practice we often have more than one kind of features. For example, we could obtain both threshold crossings and spike band power from each electrode at the same. However, these two measures have drastically different scales. In this scenario, of course we could look into them separately, but if combined, the spike power would dominate. Consequently, z-scoring also helps to re-weight the feature importance apriori.  


