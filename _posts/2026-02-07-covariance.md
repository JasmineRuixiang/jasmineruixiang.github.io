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

### 0] Problem Setup
Let's say we have a collection of neural data in the format $$X \in \mathbb{R}^{n \times d}$$, where $$n$$ is the number of samples, and $$d$$ the number of features. These are raw features each coming from a single electrode. For simplicity let's assume that there is only 1 kind of feature, like threshold crossing. Let's further say that we want to visualize the low dimensional structure of this collection of data, preserving its global geometry as much as possible (we'll clarify this later). 

Now we'd like to apply PCA on it for the first try, meaning to transform from $$\mathbb{R}^{n \times d}$$ into $$\mathbb{R}^{n \times d'}$$, where $$d'$$ might be just $$3$$, for example. Many methods would start by z-scoring $$X$$ for each feature (so for each column of $$X$$, after z-scoring would be mean 0 and standard deviation 1; more discussion see the [previous blog](2026-02-06-zscore.md) of this series) before applying PCA. 

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

So PCA finds directions of maximal variance in the original coordinate system. There're many other interpretations of PCA, for example in terms of minimizatin of reconstruction errors, or constrained non-Euclidean optimization, but this covariance interpretation is what we will grapple with in this blog ---

Because it already makes it obvious that

> PCA is sensitive to feature scale.

If one electrode has variance 100 and another has variance 1, the first will dominate the principal components — even if its structure is not more meaningful.

### 2] What does z-scoring do?
Column-wise z-scoring transforms

$$\tilde{X}_{ij} = \frac{X_{ij} - \mu_j}{\sigma_j}$$

If we use matrix notation, $$D = diag{(\sigma_1, \cdots, \sigma_n)}$$, then the above could be simplified into (again, assume it's already centered):

$$\tilde{X} = XD^{-1}$$

Now if we look at the new covariance matrix:

$$\tilde{\Sigma} = \frac{1}{n}\tilde{X}^T\tilde{X} = \frac{1}{n}D^{-1}X^TXD^{-1} = D^{-1}\Sigma D^{-1}$$

This is obviously not the same covariance matrix.

In fact, it's not hard to see that:

$$\tilde{\Sigma}_{ij} = \frac{\Sigma_{ij}}{\sigma_i \sigma_j}$$

Back to one of our original questions: does normalization preserve covariance between original features?

But this is exactly the correlation matrix.

So:

> PCA on z-scored data = PCA on the correlation matrix
> PCA on raw centered data = PCA on the covariance matrix


