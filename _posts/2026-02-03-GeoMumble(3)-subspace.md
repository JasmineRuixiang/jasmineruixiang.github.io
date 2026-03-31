---
layout: post
title: 'The Dance of Space: Geom/Topo/Dynam Mumble(3): Subspace Geometry and Computation' 
date: 2026-02-03 21:49:38
description: Subspace Geometry and Computation
series: The Dance of Space
tags: 
    - "Neural Manifold"
    - "Dimensionality Reduction"
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

## Setup

Suppose we compute PCA on two datasets (e.g., train vs.\ test), and we keep the top-$r$ principal components. Let

$$
U_{\text{train}} \in \mathbb{R}^{d \times r}, 
\qquad
U_{\text{test}} \in \mathbb{R}^{d \times r},
$$

where each matrix has **orthonormal columns** (so each is a basis for an $r$-dimensional subspace of $\mathbb{R}^d$).

Our first obvious intuition might be to define the **subspace overlap matrix** as the following:

$$
S = U_{\text{train}}^{\top} U_{\text{test}} \in \mathbb{R}^{r \times r}.
$$

So, each entry is a dot product between basis vectors:

$$
S_{ij} = u_i^{(\text{train})} \cdot u_j^{(\text{test})}.
$$

At first glance, this looks like a direct “basis alignment” comparison, which is exactly what we aim for. How else could we characterize subspace change other than looking at pairs-wise relationships among two sets of basis vectors? Well, there're a few caveats...

---

## 1] Why basis-by-basis alignment is unstable

Comparing PCA vectors one-by-one (e.g., “PC1 vs.\ PC1”) is unstable because PCA eigenvectors are **not uniquely defined** in two common cases. The following problems might pop up ---

### (i) Sign flips

If $$u$$ is an eigenvector, then $$-u$$ is also an eigenvector.

So dot products can flip sign even when the *subspace is identical*.

### (ii) Degenerate / near-degenerate eigenvalues (rotation inside the subspace)

If

$$
\lambda_i \approx \lambda_{i+1},
$$

then the corresponding principal directions inside the 2D span can rotate dramatically under tiny perturbations (noise, finite-sample effects, etc.).

This means that even if the *span* is essentially the same, the individual vectors $u_i$ can change a lot.  
So comparing “PC1 to PC1” is not meaningful.

---

## 2] What singular values do that dot products don’t

The matrix

$$
S = U_{\text{train}}^{\top} U_{\text{test}}
$$

depends on the *chosen bases* inside each subspace. If we change bases within either subspace via orthogonal transformations:

$$
U_{\text{train}} \to U_{\text{train}} R_1,
\qquad
U_{\text{test}} \to U_{\text{test}} R_2,
$$

where $R_1, R_2 \in \mathbb{R}^{r \times r}$ are orthogonal (including sign flips as a special case), then

$$
S \to (U_{\text{train}}R_1)^{\top}(U_{\text{test}}R_2)
= R_1^{\top} S R_2.
$$

So the *entries* of $S$ can change wildly.

### Key fact (invariance)

> The **singular values** of $$S$$ are invariant under left/right orthogonal rotations:

- Left-multiplying by an orthogonal matrix does not change singular values.
- Right-multiplying by an orthogonal matrix does not change singular values.

Therefore, even if PCA “relabels,” flips signs, or rotates the basis vectors within the subspace, the **singular values remain unchanged**.

This means singular values capture a property of the **subspaces**, not of the particular eigenvectors chosen.

---

## 3] Geometric meaning: principal angles

Take the SVD:

$$
S = Q \Sigma R^{\top},
$$

where

$$
\Sigma = \mathrm{diag}(\sigma_1,\dots,\sigma_r).
$$

A fundamental result is:

$$
\sigma_i = \cos(\theta_i),
$$

where $\theta_i$ are the **principal angles** between the two $r$-dimensional subspaces.

Interpretation:

- $\theta_i = 0 \implies$ perfectly aligned direction exists (since $\cos(\theta_i)=1$)
- $\theta_i = 90^\circ \implies$ orthogonal direction (since $\cos(\theta_i)=0$)

So the singular values summarize *how much overlap* the two subspaces have along their best-aligned directions.

---

## 4] Why this is the “stable” comparison

Think of $U_{\text{train}}$ and $U_{\text{test}}$ as **arbitrary coordinate systems** inside their respective subspaces.

A meaningful comparison should ignore that arbitrariness.

Principal angles / singular values do exactly this: they compute the **best possible matching** between directions in the two subspaces.

Instead of comparing “PC1 $\leftrightarrow$ PC1,” we solve an optimal alignment problem:

$$
\max_{\|a\|=\|b\|=1} a^{\top}\bigl(U_{\text{train}}^{\top}U_{\text{test}}\bigr)b,
$$

and the sequence of best matches yields

$$
\sigma_1,\sigma_2,\dots
$$

as the strengths of alignment along the best-aligned directions.

---

## 5] Tiny example intuition (2D case)

Suppose both subspaces are actually the same 2D plane in $\mathbb{R}^d$.

You could pick:

- $U_{\text{train}}$ = standard basis in that plane
- $U_{\text{test}}$ = same plane but rotated by $45^\circ$ inside it

Then $S$ might look like a rotation matrix:

$$
S=
\begin{pmatrix}
\cos 45^\circ & -\sin 45^\circ \\
\sin 45^\circ & \cos 45^\circ
\end{pmatrix}.
$$

The entries are not the identity, so basis-by-basis dot products look “not aligned.”

But the singular values of a rotation matrix are both $1$.

So singular values correctly say: **the subspaces are identical**.

That’s the whole point.

---

## Bottom line

We use singular values of

$$
U_{\text{train}}^{\top}U_{\text{test}}
$$

because:

- ✅ they are invariant to sign flips / rotations / re-ordering of PCA vectors inside the subspace  
- ✅ they define principal angles, which are a true subspace-to-subspace comparison  
- ✅ they give a stable measure of drift even when eigenvectors are not uniquely defined  

---

## A sidenote: Connection to projection distance 

Let $P_{\text{train}}$ and $P_{\text{test}}$ be the orthogonal projection matrices onto the two subspaces:

$$
P_{\text{train}} = U_{\text{train}}U_{\text{train}}^{\top},
\qquad
P_{\text{test}} = U_{\text{test}}U_{\text{test}}^{\top}.
$$

Then one can show the Frobenius-distance relationship:

$$
\|P_{\text{train}} - P_{\text{test}}\|_F^2
= 2r - 2\|U_{\text{train}}^{\top}U_{\text{test}}\|_F^2
= 2\sum_{i=1}^r \sin^2(\theta_i).
$$


---

## 6] QR Decomposition

### 6.1] Definition and Interpretation

Given a matrix $$A \in \mathbb{R}^{m \times n}$$, the **QR decomposition** writes:

$$
A = QR
$$

- $$Q$$: matrix with **orthonormal columns**
- $$R$$: **upper triangular** matrix

Note that $$m$$ is not necessarily the same as $$n$$. Also, here I'm being deliberately vague on the shapes of $$Q$$ and $$R$$. 

QR decomposition clearly separates a linear map into:

> **direction (orthonormal basis) + scaling/mixing (coordinates)**

- $$Q$$ = rotation/reflection (rigid transformation), since orthogonal matrix has determinant $$\pm 1$$.
- $$R$$ = coordinates of the columns of $$A$$ in the orthonormal basis

Specifically, to be more precise, let:

$$
A = [a_1, a_2, \dots, a_n]
$$

Then QR constructs orthonormal vectors:
$$
q_1, q_2, \dots, q_n
$$

such that:
$$
a_j = \sum_{i=1}^j r_{ij} q_i
$$

where:
$$
r_{ij} = \langle q_i, a_j \rangle
$$

In other words, $$R$$ tells us how much each column of $$A$$ points in each orthonormal direction. Furthermore, if $$Q$$ is square:

$$
Q^T Q = QQ^T = I
$$

Then:
$$
\det(Q) = \pm 1
$$

- $\det(Q) = +1$ → **rotation**
- $\det(Q) = -1$ → **reflection (or rotation + reflection)**

---

### 6.2] Important Clarifications

#### 6.2.1] Orthogonal matrix ≠ pure rotation

- Orthogonal matrices can include **reflections**
- QR decomposition may produce reflections depending on construction


#### 6.2.2] Existence of QR
- QR decomposition exists for **any matrix** $A \in \mathbb{R}^{m \times n}$


#### 6.2.3] Uniqueness of QR

The QR solution of a matrix is not unique in general. 

If:
$$
A = QR
$$

then for example
$$
Q' = QD, \quad R' = DR
$$

where $$D$$ is diagonal with entries $\pm 1$. To make it unique, we could impose:

$$
\text{diag}(R) > 0
$$

and now QR becomes **unique** and $$Q$$ has $$\det(Q) = +1 \rightarrow$$ pure rotation

---

#### 6.2.4] Rank Considerations

- If columns of $$A$$ are linearly independent:
  - $$R$$ has nonzero diagonal entries
- If rank-deficient:
  - some diagonal entries of $$R$$ are **zero**

Actually, there's subtle question: in the case of rank-deficiency, it's the entries of $$R$$ being $$0$$, instead of the columns of $$Q$$ being $$0$$ vectors. 

Let $$\operatorname{rank}(A) = r < n$$. We could interpret what happens through the lens of Gram–Schmidt (anyhow, QR is inherently doing Gram-Schmidt). 

At step $$j$$:

$$
\tilde{q}_j = a_j - \sum_{i=1}^{j-1} \langle q_i, a_j \rangle q_i
$$

- If $$a_j$$ is independent $$ \rightarrow \tilde{q}_j \ne 0$$
- If dependent $$ \rightarrow \tilde{q}_j = 0$$

However, columns of $$Q$$ are **never zero**: $$Q$$ has orthonormal columns. 

In $$R$$:

$$
R = \begin{pmatrix} * & * & * & * \\
0 & * & * & * \\
0 & 0 & * & * \\
0 & 0 & 0 & 0 \\
\end{pmatrix}
$$

- Diagonal entries:
$$
r_{jj} = 0 \quad \text{when column } a_j \text{ is dependent}
$$


---

#### 6.2.5] Two Forms of QR

Let $$A \in \mathbb{R}^{m \times n}$$ with $$m \ge n$$. The most common version of QR is the reduced (thin) QR:

$$
A = QR
$$

- $Q \in \mathbb{R}^{m \times n}$
- $R \in \mathbb{R}^{n \times n}$

Just to be super clear:

$$
Q^T Q = I_n, \quad QQ^T \ne I_m
$$

Obviously, $$Q$$ is an orthonormal basis of $$\operatorname{col}(A)$$.

On the other hand, for full QR: 

$$
A = QR
$$

- $Q \in \mathbb{R}^{m \times m}$
- $R \in \mathbb{R}^{m \times n}$

Properties:
$$
Q^T Q = QQ^T = I_m
$$

Now, only the first $$n$$ columns span $$\operatorname{col}(A)$$, and the remaining columns span the orthogonal complement. 

The special case is where $$m = n$$. In this case, $$Q \in \mathbb{R}^{n \times n}$$ and both the reduced and the full QR concide. 

---

### 6.3] Methods of Computation

- Gram–Schmidt (conceptual)
- Householder reflections (numerically stable), so $$Q$$ often includes reflections in practice

---

## 6.4] Least Squares Interpretation

Solve:
$$
\min_x \|Ax - b\|^2
$$

Using $A = QR$:

$$
QRx = b \Rightarrow Rx = Q^T b
$$

This tells us that $$Q^T b$$ is the projection of $$b$$ onto orthonormal basis. On the other hand, $$R$$ = triangular means that this equation is easy to solve. 

The projection onto $$\operatorname{col}(A)$$ is:

$$
QQ^T
$$

(when $$Q$$ has orthonormal columns)

---

### Summary fro QR
QR decomposition is actually just Gram–Schmidt written in matrix form, where:

- $$Q$$: orthonormal basis  
- $$R$$: coordinate transformation