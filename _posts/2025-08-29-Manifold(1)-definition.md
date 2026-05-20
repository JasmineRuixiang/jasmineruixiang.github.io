---
layout: post
title: "Manifold and Riemannian Geometry (1): Rigorous Definition"
date: 2025-08-29 02:32:56
description: Rigorous definition of manifold and related topology concepts
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


### 0] Preface 

Starting with this blog, I'll gradually introduce the core elements and the corresponding theories of (differentiable/smooth) manifold, which is, without any undue exaggeration or affected self-indulgence, the most fundamental arean upon which modern geometry unfolds (other than thoeries of curves and surfaces in $$\mathbb{R}^3$$). 

It's only after I seriously learned manifold that I started to appreciate its elegant geometric intuition (unintuited indeed or not easy to grasp at first glance sometimes) and its abstraction and extensions of our known, visually perceived $$\mathbb{R}^3$$ into higher dimensional space. It grants us infinite power to have a glimpse of what we are not quite possible at all for our entire lives to interpret and still acquire the confidence to draw conclusions on properties, hierarchies, and variations in the spave above us. 

As the first blog of the entire manifold Omnibus series, I aim to cover the rigorous definition of manifold, and a general introduction to its properties and different categories, to set the basis for further rigorous discussions. 

I would refrain from diverting to much from the many interesting discussions (but will review together the definition) of topological space, topology, open sets, Hausdorff space, and homeomorphism.  

---

### 1 History and motivation
Motivation of manifolds: 

Geometrically, ...

Algebraically, solution sets for polynomial equations and differnetial equations. 

Immanuel Kant's idea on the word manifold. 

---

### 2] A snapshot
The following might touch upon numerous unfamiliar terms and concepts, no worries I will cover them all as time unfolds. For now, let's get a little intuition and some impressionistic perception of this subject. 

What exactly is a manifold? The most basic kind of a manifold, a topological (n-dimensional) manifold $$M$$ is a topological space locally homeomorphic to $$\mathbb{R}^n$$. Specifically, for each point $$p \in M$$ there exists a chart around $$p$$,
which is essentially a pair $$(U, \Phi)$$ such that $$U \subset M$$ is an open set(in the topology sense) containing $$p$$ and $$\Phi$$ a homeomorphism from $$U$$ to an open subset of $$\mathbb{R}^n$$. For some technical details, the definition of manifolds would also add two other conditions: 1] the Hausdorff property and 2] second-countability. Any manifold has to be a topological manifold at least. 

The main focus of this series will be on smooth manifolds, which add some nice properties other than being topological manifolds. A smooth manifold acquires a collection of charts compatible with each other, such that the composition of coordinate functions is a diffeomorphism (they form what's called a smooth structure on the topological manifold). This nicely ensures differentiability and enables many basic calculus on the manifolds to be valid operations. 

Examples of manifold include both objects we know and some more abstractly constructed entities: $$\mathbb{R}^n$$, any onpen subsets of a manifold, the product space $$M \times N$$ (if $$M$$ and $$N$$ are manifolds). 

Another famous family of manifolds comes from compact connected 2-manifolds, specifically closed surface with genus $$g > 0$$: for example, the familiar $$S^n$$ ($$g = 0$$) and the torus $$S^1 \times S^1$$ ($$g = 1$$). 

If we specify our manifolds to be `non-orientable`, we would obtain some even more famous/esoteric objects like the (open) __Mobius strip__ (open Mobius strip is non-compact, since the boundary is not included) and __Klein bottle__ $$K = I \times I / \sim$$. From the pure topology standpoint, they are actually quite basic topological spaces. Needless to mention the famous theorem about __classification of surfaces__ which states that every compact connected 2-manifold appears as $$S^2$$ glued with tori or Mobius stirps (up to homeomorphism). There's a little subtlety here in that there do exist "manifolds with boundaries" which are locally homeomorphic to $$[ 0 \times \infty) \times \mathbb{R}^{n-1}$$ instead of $$\mathbb{R}^n$$, like Mobius strips in a usual sense. 

Other more abstract example exist such as the projective planes (frequently showing up in Topology) $$\mathbb{RP}^n\cong S^n/\sim$$ (with the equivalence relation $$v \sim -v$$). Notice that there're multiple ways to define quotient spaces for $$\mathbb{RP}^n$$. This example is usually utilized to illustrate quotient space. The Grassmanian Manifold is also frequently studied: $$Gr_k (\mathbb{R}^m) = \{ \mathrm{k\ dimensional\ vector\ subspaces\ of} \; \mathbb{R}^m \}$$, where $$0 \leq k \leq m$$. This is a compact manifold with dimensions $$k(m-k)$$ (Interested readers might come up with a guess or proof). Interestingly, $$Gr_1 (\mathbb{R}^m) \cong \mathbb{RP}^{m-1}$$. 

Since manifold is literally the backbone of modern differential geometry which has evolved into countless sub-branches and domains, it's impossible to even sample a fair amount in these blogs. Consequently, I'll be mainly focusing the following: 

- Definitions of smooth manifolds
- Submanifolds and embeddings
  - An embedding is an injective smooth map from $$M$$ to $$N$$ which is also an isormorphism between $$M$$ and a submanifold of $$N$$ 
- Tangent and cotangent spaces
  - $$p \in M, dim(M) = n, T_p N \cong \mathbb{R}^n$$
  - The differential of smooth maps between manifolds $$M, N$$ is a __linear__ map from $$T_p M$$ to $$T_{f(p)}N$$
  - The cotangent space is the dual vector space of $$T_p M$$, containing elements as $$d_p f \in T_p^{*}M$$, where $$f: M \rightarrow \mathbb{R}$$ is smooth
- Tangent and cotangent bundles
  - $$\pi: TM (\mathrm{2n\ dimensional}) \rightarrow M$$, where $$\pi^{-1}(\{ p\}) = T_p M$$
  - Example of vector bundle
  - Similarly a cotangent bundle $$Y: T^{*}M \rightarrow M$$
- Tangent and cotangent fields
  - A __section__ of the tangent bundle $$X: M \rightarrow TM$$ is a tangent field (or tangent vector field, or vector field on $$M$$): $$X(p) \in T_pM$$
  - $$X$$ as a vector field, $$X(p) \in T_pM, \forall p \in M$$, tracing out a flow on $$M$$
  - This is framed in the language of ODE and flows
  - A __section__ of the cotangent bundle is a cotangent field (or 1-forms, or differentials)
  - Cotangent fields are differential 1-forms, if $$f: M \rightarrow \mathbb{R}$$ is smooth, $$p \in M$$, then $$d_pf \in T_p^{*}M$$
  - Frobenius integrability theorem
- Differnetial $$k$$-forms on manifold $$M$$
  - Tensor bundles and tensor fields
  - A section of the $$k$$th exterior power of the cotangent bundle $$T^{*}M$$ is a differential $$k$$-form 
  - 0-forms: functions on $$M$$
  - 1-forms: cotangent fields
  - k-forms: sections of the $$k$$th exterior power of $$T^{*}N$$
  - de Rham derivative and de Rham cohomology groups: $$d: \Omega^{k}M \rightarrow \Omega^{k+1}M$$
  - For $$M$$ compact and orientable, $$\omega \in \Omega^n M$$, $$\int_{M} \omega \in \mathbb{R}$$
- General Stokes's Theorem
  - If M is compact, oriented, with boundary, n-dimensional and $$\omega \in \Omega^{n-1}M$$, then $$\int_{M}d\omega = \int_{\partial M} \omega$$
- Lie group and Lie algebra
  - Manifold $$+$$ group at the same time

---

### 3] Something dark, or darker
One last note to keep before we starting diving in, especially for readers from computational/systems neuroscience who probably have heard of the concept `neural manifold`. For the past few decades, as the technologies of simultaneous recordings of multiple neurons quickly evolve, data recorded as an entire neural population has gradually become a solid reality for many subdomains of neuroscience, and the level of data analysis promptly and adaptively shifts from the single-neuron perspective to neural population analysis. Many theories have been developed to cope with such high dimensional data (could be as many as several hundreds), like the dynamical system perspective as
my previous two blogs on [jPCA]({% post_url 2025-08-16-Research(1)-jPCA %}) and [Constraint learning]({% post_url 2025-08-25-Research(2)-constraint-learning %}), or neural manifold. This is really not a novel idea, as in many domains there's abundance of observation that among the high dimensional data there exists some low dimensional structure that captures the `patterns` of data. 

However, in most cases there's virtually nothing lost if we simply swap the word "neural manifold" into, say, "__low dimensional structure__". What worries me is that at least up to now there is no theoretical proof that a collected neural population is indeed a (at least topological) manifold. Also, from an ontological perspective, because other than employing this word itself, there's very few rigorous adaptation or usage of the many theorems and properties associated to a manifold, the true essence behind the entire domain of manifold theory is left untouched at all. Indeed, we might argue that at a figurative level such neural manifold is a nice symbol for building up intuition, but there's still a conceptual subtlety that the very first motivation of a manifold is to extend the familiar Euclidean space around us to higher dimensional spaces. In other words, the charm of manifolds shines in the high dimensional space, which, without further notice, might comletely mislead people into linking "manifold" with "low dimesional space". To be fair, in computational/systems neurscience, even when "neural manifold" is used to indicate "low dimensional space", it may still be 10 or 30, which for mathematicians are usually "high" (high vs low is a relative concept. I don't think there's a disagreement with regard to the aboslute "high"). For neuroscientists, "low dimension" is low because of its direct comparison to the original space in $$\mathbb{R}^{384}$$, for example; for a geometer, both $$\mathbb{R}^{10}$$ and $$\mathbb{R}^{384}$$ are "high dimensional space" because they extend beyond $$\mathbb{R}^{3}$$. Finally, manifold not only extends $$\mathbb{R}^n$$ to $$\mathbb{R}^N$$, where $$n >> N$$, but more importantly entirely discards the many constraints of Euclidean space and works with more general spaces (indeed, as in the shift of focus of intrinsic vs extrinsic properties). 

I never doubt that some low dimensional structure is imbedded in neural population, as countless research has corroborated it. And I deeply appreciate the remarkable wisdom and significant efforts others have paid along this line of research under the name of neural manifold. In the end, I'm just hoping concepts to stay clear and to minimize the downplay of any important concepts that are both aesthetically appearling and fundamentally stunning. I'm reluctant to observe those ideas get muddled, mumbled, and straggled among hypes and later turned into buzzwords that, at the down-to-earth applied level, mistakes its potential and hides the key questions regarding how to adapt the abstract manifold concepts into codable algorithms, or at the abstract level noisify our perception of the world (as many other instances already in an awkard situation, like "computation" or "representation"). 

Perhaps I'm being a little over-reactive.

Or...perhaps not so surprising, in recent years among Deep Learning there's more and more serious consideration and introduction of embedding geometry/topology into the modeling pipelines (geometric deel learning, interested readers might want to check out Michael Bronstein). I highly enjoy the geometric approaches whether in DL or neuroscience, and I'm very glad that I live in this era when I could witness the entire trajectory of geometry involvement. Meanwhile, I'll do my best to clarify things and introduce more into the pure beauty of geometry. Enough sentiment, now let's jump in.

---

### 4] Manifold entry point: Topological manifold
Definition of a topological manifold: 

> A topological space $$M$$ is an $$n$$-dimensional topological manifold if 
>
> 1] $$\forall p \in M$$, there exists an open neighborhoood $$U$$ ($$p \in U \subset M$$) such that there exists a homeomorphism between $$U$$ and an open subset of $$\mathbb{R}^n$$
>
> 2] $$M$$ is Hausdorff
>
> 3] $$M$$ is second-countable

The first requirement is the frequently cited notion that a manifold is _locally like_ a Euclidean space, but rigorously defined in terms of topological equivalence. The third condition about second-countability indicates that there exists a countable set of topolgy bases. This is required for guaranteeing the existence of the partition of unity on $$M$$ subordinate to any open cover. 

Conditions 1] and 3] could be combined into one statement: 
> $$M$$ is the union of finite or countably infinite open subsets $$U \subset M$$ which is homeomorphic to some open subsets of $$\mathbb{R}^n$$

Whenever a manifold is mentioned without any prefix, it's a topological manifold. More conditions could be specified to add more "flavors" to the manifold: like if we prescribe a metric to the manifold to obtain a Riemannian manifold. But without adding more complicated structure, we will only ask for something simpler to include: the differentiability, which leads to the main subject of this series of blogs: differentiable/smooth manifolds. But before going there, let's take a look at few examples that are not manifolds (to also deepen our interpretation of the definition):

---

### 5] What is not a manifold?
1] An "8"-shaped structure is not a manifold, since near the center it's not homeomorphic to any Euclidean space.  

2] A long line constructed by the first uncountable ordinal number (this violates the countability criterion)

3] A canonical example that fails the Hausdorff condition is a line with two origins: $$X = \mathbb{R}_1 \sqcup \mathbb{R}_2 / \sim $$ where $$x \sim y$$ if and only if $$x = y \neq 0$$ (so the two 0s $$0_1 \in \mathbb{R}_1, 0_2 \in \mathbb{R}_2$$ are still distinct). These two 0s cannot be distinguished because they share the same neighbors all the time. Around the origin we cannot tell from the neighbourhood if it's $$0_1$$ or $$0_2$$. The topology forces neighbourhoods to overlap too much such that the Hausdorff property fails. This example could also be extended into 2D versions. 

Another slightly different counterexample for Hausdorffness is not on separating some point, but glueing spaces together: Consider two copies of Euclidean plane: $$\mathbb{R}^2_1, \mathbb{R}^2_2$$, and glue them on the right half plane 

$$H = \{(x, y) \in \mathbb{R}^2 | x \geq 0 \}$$

Equivalently, $$X = \mathbb{R}^2_1 \sqcup \mathbb{R}^2_2 / \sim$$, where $$(x_1, y_1) \sim (x_2, y_2)$$ iff $$x \geq 0$$. Obviously, $$X$$ is not Hausdorff since there's not disjoint neighbourhood around $$(0, 0)_1$$ and $$(0, 0)_2$$.

---

### 6] Collection of descriptors: Charts and atlas
The above definition emphasizes the essence of manifold that we could only approach it locally. To derive a global view of a maifold is enabled by taking many local "cameras" and projections together. Consequently, we naturally arrive at the following definition:

> Def: An n-dimensional chart for $$M$$ is a pair $$(U, \Phi)$$ such that $$U \subset M$$ is an open sutset, $$\Phi: U \rightarrow \Phi(U)$$ a homoemorphism from $$U$$ to an open subset of $$\mathbb{R}^n$$. 
>
> Def: A chart for $$M$$ is a collection $$\{U_{\alpha}, \Phi_{\alpha} \}_{\alpha \in A}$$ such that $$\bigcup_{\alpha \in A} U_{\alpha} = M $$

With the concept of chart and atlas, we now are fully capable of pulling the manifold into different Euclidean frames to analyze them, since a chart $$(U, \Phi)$$ determines real-valued functions $$x_1, x_2, \dots x_n $$ such that $$\Phi(p) = (x_1(p), x_2(p), \dots, x_n(p))$$ where $$p \in U \subset M$$ and $$x_i: U \rightarrow \mathbb{R}^n$$. These $$x_i$$ are called the coordinate functions of the selected chart. 

Some readers might have a concern now. If there exist some charts $$(U, \Phi)$$ and $$(V, \Psi)$$ where $$U \cap V \neq \varnothing$$, then $$\Phi(U \cap V)$$ and $$\Psi(U \cap V)$$ will have different coordinates. Would this cause a mess? 

---

### 7] A "Bit" of Topology Relatedness
Readers should be already familiar with the concept of topological sapce and homeomorphisms. Here below let me elaborate on a few concepts that once bothered me for some time, and I'd like to share some of my thoughts and intuition into how to interpret them. Most of them are indeed related to topology... Hopefully after reading the following section, your understanding of topolgoical manifold will be deepened.  

#### 7.1] Topological Invariance of Dimensions

While I was learning it, I once questioned why we could determine the dimensionality of a manifold simply by an arbitrary point (given that it's connected). What makes it that $$n(p) = n(p')$$? It turns out this question merrits much more attention and there's deep theory behind it.  

Specifically, the dimension of a topological manifold is specified by the local Euclidean condition. When we talk about a topological manifold of dimension $$n$$, it's not saying that on certain open sets the dimension is $$n$$, but that the entire topological manifold has dimension $$n$$. I understand that from the theorem of `topological invariance of dimensions` we know that the dimension of a local neighborhood around a point $$p \in M$$ is uniquely determined by $$p$$, but then why would $$n(p) = n^*$$ for all $$p \in M$$, not that different points have neighbors of different dimensions?

Well, let's go back to the concept itself: The topological invariance of dimension tells us that 

> If $$\phi: U \to V$$ is a homeomorphism with $$U \subseteq \mathbb{R}^m$$, $$V \subseteq \mathbb{R}^n$$ open (Euclidean), then $$m = n$$.

Notice that here the Euclidean topology is not just a convenience — the proof fundamentally uses analytic/topological properties specific to $$\mathbb{R}^n$$ with its standard topology.

Why is the Euclidean topology load-bearing? Well, the standard proofs go through one of these tools:

- **Singular homology:** $$H_k(\mathbb{R}^n, \mathbb{R}^n \setminus \{p\}) \cong \tilde{H}_{k-1}(S^{n-1})$$, which detects $$n$$ algebraically. This uses the specific homotopy type of $$S^{n-1}$$, which is a Euclidean fact.
- **Invariance of domain** (Brouwer): a continuous injective map from an open subset of $$\mathbb{R}^n$$ into $$\mathbb{R}^n$$ is open. Again, deeply Euclidean.

Both rely on the topology of spheres $$S^{n-1}$$, which is how the "dimension" leaks into the algebra.

What then goes wrong with arbitrary topologies? If we allow arbitrary topologies, the statement is simply false. Two quick counterexamples:

1. **Discrete topology.** Give $\mathbb{R}^m$ and $\mathbb{R}^n$ the discrete topology (every subset is open). Then any bijection $\mathbb{R}^m \to \mathbb{R}^n$ is a homeomorphism — and such bijections exist between any two sets of the same cardinality. Since $|\mathbb{R}^m| = |\mathbb{R}^n|$ for all $m, n \geq 1$, you get homeomorphisms between $\mathbb{R}^1$ and $\mathbb{R}^{100}$.

2. **Indiscrete topology.** Give both spaces the indiscrete topology (only $\emptyset$ and the whole space are open). Then again any bijection is a homeomorphism.

So topology alone — without the specific *structure* of the Euclidean topology — cannot distinguish dimension.

The deeper point is that **dimension is a topological invariant of Euclidean space**. Without topological invariance of dimension, the number $$n$$ wouldn't even be well-defined — a single manifold could appear "$$m$$-dimensional" in one chart and "$$n$$-dimensional" in another. The theorem is what makes the definition coherent.

| Setting | Does $$\phi: U \to V$$ homeomorphism imply $$m = n$$? |
|---|---|
| $$U, V$$ open in $$\mathbb{R}^m, \mathbb{R}^n$$ with Euclidean topology | **Yes** — this is the theorem |
| Arbitrary topologies on $$\mathbb{R}^m, \mathbb{R}^n$$ | **No** — fails for discrete, indiscrete, etc. |

The Euclidean topology is precisely rigid enough to encode dimension, and that rigidity is what the whole theory of manifolds is built on.

---
#### 7.2] The Same Dimension 
However, if you think a little more carefully, the above theorem **does not**, by itself, force $$n(p)$$ to be the same for all $$p \in M$$, which is what we frequently argue for. That requires a separate argument.

Or let me rephase a little bit: The dimension of a topological manifold is specified by the local Euclidean condition. When we talk about a topological manifold of dimension $$n$$, it's not saying that on certain open sets the dimension is $$n$$, but that the entire topological manifold has dimension $$n$$. We already understand that from the theorem of topological invariance of dimensions that the dimension of a local neighborhood around a point $$p \in M$$ is uniquely determined by $$p$$, but then why would $$n(p) = n^*$$, for all $$p \in M$$, not that different points have neighbors of different dimensions? 

The gap we identified is real! The topological invariance of dimension tells us that if $$\phi: U \rightarrow V$$ is a homeomorphism with $$\subseteq \mathbb{R}^m$$, $$\subseteq \mathbb{R}^n$$ open (Euclidean), then $$m = n$$. This tells us the dimension attached to any single chart is well-defined — you can't have two charts around the same point $$p$$ with different dimensions. So $$n(p)$$ is well-defined for each $$p$$. However, notice that this does not, by itself, force $$n(p)$$ to be the same for all $$p \in M$$. That requires a separate argument.

And the answer lies in connectedness! The key fat is that 

> If the function $$p \mapsto n(p)$$ is **locally constant** on $$M$$, and if $$M$$ is connected, then a locally constant function must be globally constant. 

Let's break this done a little bit further. 

**Step 1: $$n(p)$$ is locally constant**

Fix any point $$p \in M$$. By the manifold condition, there exists a chart $$(U, \phi)$$ with $$p \in U$$ and $$\phi: U \to \mathbb{R}^{n(p)}$$ a homeomorphism. Now for *any* other point $$q \in U$$, the same chart gives a local homeomorphism to $$\mathbb{R}^{n(p)}$$ around $$q$$. So $$n(q) = n(p)$$ for all $$q \in U$$. Around every point $$p$$, there is an open neighborhood where the dimension is constantly $$n(p)$$ — that's precisely what locally constant means.

**Step 2: locally constant + connected $$\Rightarrow$$ globally constant**

Suppose $$f: M \to \mathbb{Z}$$ is locally constant and $$M$$ is connected. For any fixed value $$k$$, the set $$f^{-1}(k)$$ is open (by local constancy) and closed (its complement $$f^{-1}(\mathbb{Z} \setminus \{k\})$$ is also open). Since $$M$$ is connected, the only subsets that are simultaneously open and closed are $$\emptyset$$ and $$M$$ itself. So $$f^{-1}(k)$$ is either empty or all of $$M$$ — meaning $$f$$ is globally constant.

Applying this to $$f = n(\cdot)$$: since $$M$$ is connected, $$n(p)$$ is the same for all $$p \in M$$.

Where does connectedness enter the manifold definition? Some authors define topological manifolds to be connected, others don't. If you don't assume connectedness, then a manifold can have **components of different dimensions** — and that's actually allowed in the most general definition.

For example: take $$M = \mathbb{R}^1 \sqcup \mathbb{R}^2$$ (disjoint union). This satisfies every condition of a topological manifold locally, but different points have different $$n(p)$$. It's a perfectly valid manifold in the general sense — just a disconnected one with components of different dimensions.

Most treatments in Riemannian geometry implicitly or explicitly assume connectedness, because the interesting geometric theorems (Hopf-Rinow, Bonnet, etc.) require it. The standard convention is:

> A topological manifold of dimension $n$ is a connected, Hausdorff, second-countable topological space that is locally homeomorphic to $$\mathbb{R}^n$$.

Under this convention, $$n$$ is globally well-defined precisely because connectedness forces local constancy to become global constancy.

Now, let me give a summary of the logical chain: 

$$\text{Topological invariance of dimension} \Rightarrow n(p) \text{ is well-defined at each } p$$

$$\text{Chart overlap} \Rightarrow n(p) \text{ is locally constant}$$

$$\text{Connectedness} \Rightarrow n(p) \text{ is globally constant} = n$$

---

#### 7.3] Compactness

The definition of compactness also tripped me a few times: $$K \subseteq X$$ is called compact if every open cover of $$K$$ has a finite subcover. What's the intuition behind it? Also, you might have heard that in Euclidean topology compactness is equivalent to closed + bounded. How to interpret that?

The core intuition is that 

> **Compact sets behave like finite sets, even when they're infinite.**

Many theorems that are trivially true for finite sets — "a continuous function on a finite set attains its maximum", "a finite union of closed sets is closed" — extend to infinite sets *only if* you have compactness.

I'd like to think of it in this way: An open cover $$\{U_\alpha\}$$ of $$K$$ is a way of "explaining" every point of $$K$$ using open sets. Requiring a **finite subcover** means: no matter how you try to cover $$K$$, you can always do it with finitely many pieces. You can never be forced into an essentially infinite explanation.

Actually here the failure cases perhaps build more intuition: 

**Example 1: $$(0, 1)$$ — open interval, not compact**

Consider the open cover $$U_n = (1/n, 1)$$ for $$n = 2, 3, 4, \ldots$$. Every point $$x \in (0,1)$$ is eventually in some $U_n$, so this is a valid cover. But no finite subcollection covers $$(0,1)$$ — any finite sub-collection $$\{U_{n_1}, \ldots, U_{n_k}\}$$ only covers $$(1/N, 1)$$ where $$N = \max(n_i)$$, leaving out points near $$0$$. The open endpoint at $$0$$ is the "escape hatch."

**Example 2: $$\mathbb{R}$$ — unbounded, not compact**

Cover $$\mathbb{R}$$ by $$U_n = (-n, n)$$. No finite subcollection covers all of $$\mathbb{R}$$. The unboundedness is the escape hatch here.

**The pattern:** non-compact sets always have some "place to escape to" — either an open boundary, or off to infinity. Compact sets have no escape hatches.

Fine. Now let's relate this defnition and interpretation to the mfamous Heine-Borel theorem where compactness is equivalent to closed + bounded in $$\mathbb{R}^n$$: 

> **Heine-Borel Theorem.** $$K \subseteq \mathbb{R}^n$$ is compact (in the Euclidean topology) if and only if $$K$$ is closed and bounded.

Each condition plugs one escape hatch:

- **Bounded** — prevents escape to infinity
- **Closed** — prevents escape to a limit point outside the set

**Important caveat:** Heine-Borel is *special to $$\mathbb{R}^n$$*. In a general metric space or topological space, closed and bounded does **not** imply compact. For instance, in an infinite-dimensional Banach space, the closed unit ball is closed and bounded but not compact. When we move to manifolds, we must use the open cover definition as primary.

Up to now you might wonder why we care so much about compactness. It in the end does not seem to hint at anything significant. Well, let's see the following theorem: 

> **Theorem.** A continuous function $f: K \to \mathbb{R}$ on a compact space $K$ attains its maximum and minimum.

If you've learned any intro level real analysis course, hopefully the above theorem rings a bell. Without compactness this theorem fails immediately: $$f(x) = x$$ on $$(0,1)$$ is continuous but attains neither. This pattern — *compactness of domain forces good behavior of functions* — recurs constantly in manifold theory, e.g. showing that a smooth function on a compact manifold has a critical point, or that geodesics can be extended indefinitely on a compact Riemannian manifold.

A simple summary of the above interpretation:

| Intuition | Precise meaning |
|---|---|
| "Finite-like control" | Every open cover has a finite subcover |
| "No escape hatch" | Can't leak out to boundary or to infinity |
| In $\mathbb{R}^n$: closed + bounded | Heine-Borel (Euclidean-specific) |
| Functions behave well | Continuous images of compact sets are compact |

**One thing to internalize deeply:** the open cover definition is the *right* definition because it works in full generality — on abstract topological spaces, on manifolds, in function spaces. Heine-Borel is a beautiful theorem *about* $$\mathbb{R}^n$$, not the definition of compactness. Always think with the open cover definition; use Heine-Borel only when you're concretely inside $$\mathbb{R}^n$$.

---
#### 7.4] Hausdorff
Let me start with a questio: It's easy to understand the definition of Hausdorff, but how to interpret that; specifically, how does it tell us about coarseness of the given topology? Well, the Hausdorff property is related to coarseness, but the precise relationship is worth unpacking carefully, because it's subtler than it first appears.

Let's remind ourselves again the definition: A topological space $$(X, \mathcal{T})$$ is **Hausdorff** (or $$T_2$$; see later) if:

> For every two distinct points $$x, y \in X$$, there exist open sets $$U, V \in \mathcal{T}$$ with $$x \in U$$, $$y \in V$$, and $$U \cap V = \emptyset$$.

In words, this means that **any two distinct points can be separated by disjoint open sets.** But what is it really saying?  The deeper interpretation is about the **resolving power** of the topology — __whether the open sets are "fine enough" to tell points apart__.

Think of open sets as the things your topology can "observe" or "detect." In a Hausdorff space, for any two distinct points, there exists an observation (an open set) that contains one but not the other — and moreover you can do this simultaneously with no overlap. The topology is rich enough to cleanly separate any two distinct points.

- A **coarse** topology has few open sets $$\rightarrow$$ limited ability to separate points $$\rightarrow$$ more likely to fail Hausdorff
- A **fine** topology has many open sets $$\rightarrow$$ more ability to separate points $$\rightarrow$$ more likely to be Hausdorff

Let me presesent some failure cases so we might deepen our understanding: 

**Example 1: Indiscrete topology — maximally coarse, maximally non-Hausdorff**: On any set $$X$$ with $$|X| \geq 2$$, the indiscrete topology $$\mathcal{T} = \{\emptyset, X\}$$ is not Hausdorff. The only open set containing $$x$$ is all of $$X$$, which also contains $$y$$.

**Example 2: The line with two origins**: Take two copies of $$\mathbb{R}$$, glued together along $$\mathbb{R} \setminus \{0\}$$, leaving two distinct "origin" points $$0_1$$ and $$0_2$$. Every open set containing $$0_1$$ and every open set containing $$0_2$$ must overlap. This space is not Hausdorff — and is important in manifold theory.

**Example 3: Discrete topology — maximally fine, always Hausdorff**: Every subset is open, so $$\{x\}$$ and $$\{y\}$$ are disjoint open sets separating any $$x \neq y$$. Trivially Hausdorff.

Consequently, we could see from above that Hausdorff property and the "coarseness/fineness" of the given topology are closely related. Actually to be precise: 

> **Hausdorff is a lower bound on the fineness of the topology**, relative to the point-set structure of $$X$$.

There is actually a whole hierarchy of **separation axioms** ($T_0, T_1, T_2, T_3, \ldots$):

| Axiom | Condition |
|---|---|
| $$T_0$$ (Kolmogorov) | For any $x \neq y$, some open set contains one but not the other |
| $$T_1$$ (Fréchet) | For any $x \neq y$, each has an open set not containing the other |
| $$T_2$$ (Hausdorff) | For any $x \neq y$, they have *disjoint* open neighborhoods |
| $$T_3$$ (Regular) | Points and closed sets can be separated by disjoint opens |
| $$T_4$$ (Normal) | Disjoint closed sets can be separated by disjoint opens |

Each level is strictly stronger — $$T_2 \Rightarrow T_1 \Rightarrow T_0$$ but not conversely. Hausdorff sits at $$T_2$$.

Why would manifolds require Hausdorff? The "line/plane with two origins" example shows exactly why: that pathological space is locally homeomorphic to $$\mathbb{R}$$ everywhere (it satisfies the chart condition), but it fails Hausdorff at the two origins. It behaves badly: limits are not unique, and the two origins cannot be separated.

The Hausdorff condition ensures that:

- **Limits are unique** — if a sequence converges, it has exactly one limit
- **Diagonal is closed** — $$\Delta = \{(x,x) : x \in X\} \subseteq X \times X$$ is closed, crucial for many arguments (we will use this fact later in the proof of some embedding theorem)
- **Compact sets are closed** — without Hausdorff, a compact set need not be closed

In summary, Hausdorff tells us the topology is **fine enough to separate points**, which is the minimum standard of geometric decency — without it, the space can't reliably distinguish locations, and calculus-like arguments break down. It's in some sense about coarseness in general, but more precisely about a specific threshold of fineness that the topology must clear.

---

#### 7.5] Topology Determines Whether a Set Is a Manifold
Another question that keeps haunting me is that since the definition of topological manifold requires the underlying topology, does that mean that one set with topology $$A$$ (thus forming topological space $$A'$$) could be a topological manifold, while the same exact set with topology $$B$$ (thus forming topological space $$B'$$) might not be a topological manifold?

Exactly. The manifold property lives in the __topological space, not in the underlying set__: A topological manifold is a topological space $$(X, \mathcal{T})$$ satisfying certain conditions — Hausdorff, second countable, locally Euclidean (perhaps adding connected). The set $$X$$ alone carries none of this structure.

Let me present a few concrete examples on the same set $$\mathbb{R}$$ so that we could have a flavor on how the given topology affects the properties of the topological manifold, if any. 

**Topology 1: Standard Euclidean — is a manifold** ✓
$$\mathcal{T}_{\text{std}}$$ generated by open intervals $$(a,b)$$. This is Hausdorff, second countable, and every point has a neighborhood homeomorphic to $$\mathbb{R}^1$$. So $$(\mathbb{R}, \mathcal{T}_{\text{std}})$$ is a 1-dimensional topological manifold.

**Topology 2: Discrete topology — not a manifold** ✗
$$\mathcal{T}_{\text{disc}}$$ where every subset is open. This is Hausdorff, but every point $$p$$ is isolated — its neighborhood $$\{p\}$$ is open and homeomorphic to a single point, not to any $$\mathbb{R}^n$$ with $$n \geq 1$$. So $$(\mathbb{R}, \mathcal{T}_{\text{disc}})$$ fails the locally Euclidean condition.

But hold on. I had a question when I first saw this example: why $$(\mathbb{R}, \mathcal{T}_{\text{disc}})$$ does fail the locally Euclidean condition? Specifically:
1. The locally Euclidean condition only requires that there *exists* one neighborhood of $p$ homeomorphic to a subset of $$\mathbb{R}^n$$, so why must we pick neighborhood $$\{p\}$$?
2. Moreover, even if we pick $\{p\}$, given the discrete topology, why is it homeomorphic to a single point? A single point in $$\mathbb{R}^n$$ is not open, while $$\{p\}$$ in the discrete topology is open. Consequently, we don't have to pick $$\{p\}$$ — we need to check whether **any** neighborhood works. The subspace topology on any $$U \subseteq \mathbb{R}$$ induced by $$\mathcal{T}_{\text{disc}}$$ is again discrete: for any $$q \in U$$, the singleton $$\{q\}$$ is open in $$\mathcal{T}_{\text{disc}}$$, so $$\{q\} = U \cap \{q\}$$ is open in the subspace topology on $$U$$.

So every neighborhood of $$p$$, with its subspace topology, is a discrete space. The question then reduces to:

> Is any discrete topological space homeomorphic to an open subset of $$\mathbb{R}^n$$?

The answer is no: any nonempty open set in $$\mathbb{R}^n$$ is uncountable and connected, while a discrete space with more than one point is disconnected. More precisely, a nonempty open subset of $\mathbb{R}^n$ with the Euclidean topology is never discrete — every point is a limit point of the set. So no neighborhood of $p$ in $(\mathbb{R}, \mathcal{T}_{\text{disc}})$ can be homeomorphic to an open subset of $\mathbb{R}^n$.

Specifically, the space $$\{p\}$$ with the discrete topology is just a single-point space. The only open subset of $$\mathbb{R}^n$$ homeomorphic to a single-point space is... well, a single point $$\{x\} \subseteq \mathbb{R}^n$$. But $$\{x\}$$ is not open in $$\mathbb{R}^n$$ with the Euclidean topology for any $$n \geq 1$$.
So the issue is not about $$\{p\}$$ being open in the discrete topology — it is open there, and that's fine. The issue is that the image $$\phi(\{p\}) = \{x\}$$ must be open in $$\mathbb{R}^n$$, and a single point is never open in $$\mathbb{R}^n$$ for $$n \geq 1$$. You might then ask: what if $$n = 0$$? Indeed $$\mathbb{R}^0$$ is just a single point, and it is open in itself. So a single point is a 0-dimensional manifold. This is consistent — discrete spaces on countable sets are valid 0-dimensional manifolds.

The full chain of failure goes like the following:

$$\text{Any neighborhood } U \text{ of } p \text{ inherits the discrete topology}$$
$$\Downarrow$$
$$U \text{ is homeomorphic only to another discrete space}$$
$$\Downarrow$$
$$\text{No discrete space with } \geq 2 \text{ points is homeomorphic to an open subset of } \mathbb{R}^n, n \geq 1$$
$$\Downarrow$$
$$\text{The only candidate is } \{p\} \cong \mathbb{R}^0 \text{, giving dimension } 0$$


**Topology 3: Indiscrete topology — not a manifold** ✗
$$\mathcal{T}_{\text{ind}} = \{\emptyset, \mathbb{R}\}$$. Fails Hausdorff immediately (no way to separate distinct points with disjoint open sets).

**Topology 4: Lower limit topology (Sorgenfrey line) — not a manifold** ✗
$$\mathcal{T}_{\ell}$$ generated by half-open intervals $$[a, b)$$. Still Hausdorff, but it fails to be locally homeomorphic to $$\mathbb{R}$$ with the standard topology — the half-open intervals create a fundamentally different local structure.

More importantly, this hints at a deeper fundamental fact: The layers of structures that we __choose to add__ are genuinely independent:

$$\underbrace{\text{Set } X}_{\text{no structure}} \longrightarrow \underbrace{(X, \mathcal{T})}_{\text{topological manifold}} \longrightarrow \underbrace{(X, \mathcal{T}, \mathcal{A})}_{\text{smooth manifold}} \longrightarrow \underbrace{(X, \mathcal{T}, \mathcal{A}, g)}_{\text{Riemannian manifold}}$$

(we will see the following structures shortly) Each arrow adds genuine structure not determined by the previous layer. The famous example: $$\mathbb{R}^4$$ admits uncountably many exotic smooth structures, all on the same underlying topological manifold (also see Milnor's exotic sphere).

In summary, a set is just a collection of points with no geometry, no nearness, no dimension. All of those concepts enter through the topology. So yes, the same set can be a manifold under one topology and fail to be a manifold under another, because "being a manifold" is a property of the topological space $$(X, \mathcal{T})$$, not of $$X$$ alone.

---
#### 4.6] Open Subsets of Manifolds Are Manifolds

I once read a claim that 

> if $$M$$ is an $$n$$-dimensional topological manifold, and $$M' \subseteq M$$ is an open subset, then $$M'$$ is also an $$n$$-dimensional topological manifold. 

I thought that we'd require $$M'$$ to also be a manifold at the same time to conclude that its dimension is also $$n$$, not just that it's an open subset of $$M$$.

Let me sketch through the proof here since I think it's really interesting: 

Given: $$M$$ is an $$n$$-dimensional topological manifold, $$M' \subseteq M$$ is open, $$M'$$ carries the **subspace topology** from $$M$$. We need to verify $$M'$$ satisfies all conditions: Hausdorff, second countable, locally Euclidean of dimension $$n$$, and connectedness (though connectedness can fail).

* Condition 1: Hausdorff ✓

Take any two distinct points $$x, y \in M' \subseteq M$$. Since $$M$$ is Hausdorff, there exist disjoint open sets $$U, V \subseteq M$$ with $$x \in U$$ and $$y \in V$$. Then $$U \cap M'$$ and $$V \cap M'$$ are open in the subspace topology on $$M'$$, they are disjoint, and they separate $$x$$ and $$y$$.

*General fact used:* every subspace of a Hausdorff space is Hausdorff.

* Condition 2: Second Countable ✓

Since $$M$$ is second countable, it has a countable basis $$\{B_i\}_{i=1}^\infty$$. The collection $$\{B_i \cap M'\}_{i=1}^\infty$$ is a countable basis for the subspace topology on $$M'$$.

*General fact used:* every subspace of a second countable space is second countable.

* Condition 3: Locally Euclidean of Dimension $$n$$ ✓

Take any $$p \in M'$$. Since $$p \in M$$, there exists a chart $$(U, \phi)$$ in $$M$$ with $$p \in U$$ and:
$$\phi: U \xrightarrow{\sim} \tilde{U} \subseteq \mathbb{R}^n$$
where $$\tilde{U}$$ is open in $$\mathbb{R}^n$$ and $$\phi$$ is a homeomorphism.

Consider $$U' = U \cap M'$$. Since both $$U$$ and $$M'$$ are open in $$M$$, their intersection $$U'$$ is open in $$M$$, and since $$U' \subseteq M'$$, it is also open in $$M'$$.

Restrict $$\phi$$ to $$U'$$:
$$\phi|_{U'}: U' \xrightarrow{\sim} \phi(U')$$

The image $$\phi(U')$$ is open in $$\mathbb{R}^n$$ because $$\phi$$ is a homeomorphism and $$U'$$ is open in $$U$$. And $$\phi|_{U'}$$ is a homeomorphism.

So $$(U', \phi|_{U'})$$ is a valid chart for $$M'$$ around $$p$$, mapping homeomorphically onto an open set in $$\mathbb{R}^n$$. The dimension is $$n$$ — not because we assumed $$M'$$ is a manifold — but because the chart inherited from $$M$$ maps into $$\mathbb{R}^n$$, and by topological invariance of dimension, no other dimension is possible.

* Condition 4: Connectedness — Can Fail ✗

A simple example: $$M = \mathbb{R}$$ (a connected 1-manifold), and $$M' = (-2, -1) \cup (1, 2)$$, which is open in $$\mathbb{R}$$ but disconnected. It's still a 1-dimensional manifold — just not a connected one. So the precise statement is:

> If $$M$$ is an $$n$$$-dimensional topological manifold and $$M' \subseteq M$$ is open, then $$M'$$ is an $$n$$-dimensional topological manifold, **possibly disconnected**.

If you require manifolds to be connected by definition, then you'd say $$M'$$ is a manifold if and only if it happens to be connected, but each connected component is individually an $$n$$-dimensional manifold.

From the aboe proof we should see why openness is essential. Openness does two things simultaneously:

1. **It makes the subspace topology well-behaved** — open subsets of Hausdorff, second countable spaces inherit those properties cleanly.
2. **It ensures chart restrictions are still charts** — the restriction $$\phi|_{U \cap M'}$$ lands in an open subset of $$\mathbb{R}^n$$ precisely because $$U \cap M'$$ is open in $$U$$, and homeomorphisms send open sets to open sets.

If $$M'$$ were merely a *closed* or *arbitrary* subset, step 2 would fail — $$\phi(M' \cap U)$$ need not be open in $$\mathbb{R}^n$$. For example, $$\phi$$ maps a closed half-disk to a closed half-disk in $$\mathbb{R}^2$$, which is not open — this is exactly the situation of **manifolds with boundary**, which require a separate definition.

Let's do a short summary table: 

| Condition | Inherited by open subsets? | Reason |
|---|---|---|
| Hausdorff | ✓ always | Subspace of Hausdorff is Hausdorff |
| Second countable | ✓ always | Subspace of second countable is second countable |
| Locally Euclidean of dim $n$ | ✓ always | Charts restrict to charts via __openness__ |
| Connected | ✗ not always | Open subsets can be disconnected |

---

#### 4.7] Connectedness: Clopen Sets
Another important concept from topolog which I read a bunch of times before finally interpreting it is connectedness. 

> A topological space $$X$$ is connected if the only subsets that are open and closed are the empty set and $$X$$ itself. 

What does this entail? How is it consistent with the intuitive understanding of connected/disconnected? Well, let's start with what disconnected should mean: Intuitively, a space is **disconnected** if it can be split into two separate pieces with no overlap and no shared boundary. 

Why is clopen the right language? Notice that if $$A$$ and $$B$$ are disjoint, cover $$X$$ , and both are open, then:

$$B = X \setminus A$$

so $$A$$ is open and its complement is also open — meaning $$A$$ is both open and closed (clopen). So the existence of a nontrivial clopen set is exactly equivalent to the existence of a nontrivial separation. The definition of connectedness simply rules this out directly:

$$X$$ 
is connected  $$\iff$$ the only clopen subsets are $$\emptyset$$ and $$X$$

Think of $$(-2,-1) \cup (1,2)$$ — two intervals floating apart. If $$X$$ splits into two pieces $$A$$ and $$B$$, then $$A$$ and $$B$$ are disjoint ($$A \cap B = \emptyset$$), they cover $$X$$ ($$A \cup B = X$$), and neither piece "touches" the other. That last condition — "neither piece touches the other" — is exactly what requiring both to be open captures. If $$A$$ and $$B$$ are disjoint, cover $$X$$, and both are open, then $$B = X \setminus A$$, so $$A$$ is open and its complement is also open — meaning $$A$$ is **clopen** (both open and closed). The existence of a nontrivial clopen set is exactly equivalent to a nontrivial separation.

Let me be very explicit: Take $$X = (-2,-1) \cup (1,2)$$ with the subspace topology from $$\mathbb{R}$$. Let $$A = (-2,-1)$$ and $$B = (1,2)$$. In the subspace topology on $$X$$:

- $$A$$ is open — it's the intersection of the open set $$(-2,-1)$$ with $$X$$
- $$A$$ is also closed — its complement in $$X$$ is $$B = (1,2)$$, which is also open

So $$A$$ is a nontrivial clopen subset of $$X$$, witnessing disconnectedness. The two intervals are genuinely separated — no sequence in $$A$$ can converge to a point in $$B$$, no path can travel from one to the other without leaving $$X$$. Now contrast with $$X = (-2, 2)$$: any clopen subset $$A$$ would need to be open (no boundary points inside $$X$$) and closed (contains all its limit points in $$X$$). In a connected interval, there are none besides $$\emptyset$$ and $$X$$ itself.

We could interpret the closedness as a wall, intuitively: A clopen set is a subset with no boundary inside $$X$$. Recall that the boundary of $$A$$ consists of points where every neighborhood meets both $$A$$ and its complement. If $$A$$ is closed, it contains its boundary. If $$A$$ is open, it contains no boundary points. If $$A$$ is both — clopen — then it has no boundary at all (within $$X$$). So connectedness says: you cannot draw a wall anywhere inside $$X$$ that cleanly separates it. Any attempted dividing wall would have to have points on it, and those points would belong to one side or the other, creating a contradiction.

---

#### 4.8] Connected Manifolds Are Path-Connected
One more distinction worth having here is between connected and path-connected. Connected means no clopen split exists, which is purely topological, whereas path-connected says that any two points can be joined by a continuous path. 

Path-connected $$\Rightarrow$$ connected, but not conversely. The classic counterexample is the topologist's sine curve. However, for manifolds, both notions coincide — a connected manifold is automatically path-connected (thought in general topology they are distinct).

What's the intuition why this is true? What components/definitions from topological manifolds makes connectedness and path-connectedness equivalent? Here below I want to present some thoughts and a quick sketch of proof. 

The key insight is that the locally Euclidean condition is doing all the work here — it's what bridges the gap between the purely topological notion of connectedness and the geometric notion of path-connectedness.

Specifically, note that in a general topological space, connectedness is a global condition (no clopen split) while path-connectedness is a local-to-global condition (you can travel between any two points). These can come apart because the local structure of a general space can be arbitrarily pathological. But a manifold has **controlled local structure** — every point looks like $$\mathbb{R}^n$$ nearby. And $$\mathbb{R}^n$$ is path-connected in the simplest possible way: any two points are joined by a straight line. This local path-connectedness is the bridge. The rough slogan is:

> Local path-connectedness + global connectedness $$\Rightarrow$$ global path-connectedness

Consequently, the Key lemma is that manifolds are locally path-connected. Why? Every point $$p \in M$$ has a neighborhood homeomorphic to $$\mathbb{R}^n$$. Since $$\mathbb{R}^n$$ is path-connected (join any two points by a straight line segment), and path-connectedness is preserved by homeomorphisms (it's a toopological property (topological invariant)), every such neighborhood is path-connected. **Every point has a path-connected neighborhood.**

The sketch of the proof is the following: Given $$M$$ connected and locally path-connected, fix any point $$p \in M$$. Define:

$$A = \{q \in M : \text{there exists a path from } p \text{ to } q\}$$

We show $$A$$ is both open and closed in $$M$$. Since $$M$$ is connected, this forces $$A = M$$.

**$$A$$ is open:** Take any $$q \in A$$, so there is a path $$\gamma$$ from $$p$$ to $$q$$. By local path-connectedness, $$q$$ has a path-connected neighborhood $$U$$. Every point $$r \in U$$ can be joined to $$q$$ by a path $$\sigma$$ inside $$U$$. Concatenating $$\gamma$$ and $$\sigma$$ gives a path from $$p$$ to $$r$$. So $$U \subseteq A$$, and $$A$$ is open.

**$A$ is closed:** Take any limit point $$q \in \overline{A}$$. By local path-connectedness, $$q$$ has a path-connected neighborhood $$U$$. Since $$q \in \overline{A}$$, there exists some $$r \in U \cap A$$. There is a path from $$p$$ to $$r$$ (since $$r \in A$$), and a path from $$r$$ to $$q$$ inside $$U$$. Concatenating gives a path from $$p$$ to $$q$$, so $$q \in A$$. Thus $$A$$ is closed.

**Conclusion:** $$A$$ is nonempty (contains $$p$$ via the constant path), open, and closed. Since $$M$$ is connected, $$A = M$$.

From aboe we know that the logical structure is the following: 

$$\underbrace{\text{Locally Euclidean}}_{\text{manifold condition}} \Rightarrow \underbrace{\text{locally path-connected}}_{\text{key lemma}}$$

$$\underbrace{\text{locally path-connected}}_{\text{key lemma}} + \underbrace{\text{connected}}_{\text{assumed}} \Rightarrow \underbrace{\text{path-connected}}_{\text{conclusion}}$$

The proof is essentially a clopen argument — the set $$A$$ of points reachable by paths from $$p$$ is clopen, and connectedness forces it to be everything. The locally Euclidean condition is what guarantees $$A$$ is open and closed in the first place.

Now we could look back on why the topologist's sine curve fails: The topologist's sine curve is connected but not path-connected — it is **not** locally path-connected at points on the vertical segment $$\{0\} \times [-1,1]$$. Every neighborhood of such a point contains infinitely many disconnected oscillating arcs that cannot be reached by a path from the vertical segment. The locally Euclidean condition fails there, and with it, the bridge between connectedness and path-connectedness breaks down.

A manifold, by contrast, has no such pathological local behavior: the locally Euclidean condition is precisely the guarantee that the local structure is as tame as $$\mathbb{R}^n$$.

---
#### 4.9] Local Compactness of Manifolds

Another important basic fact is that

> A topological manifold is locally compact. 

How to interpret this? Well, intuitively, this again relates back to the locally Euclidean condition that any topological manifold acquires. Remember that a topological space $$X$$ is **locally compact** at a point $$p$$ if:

> There exists a compact set $$K$$ such that $$p \in \text{int}(K)$$ — i.e., $$p$$ has a neighborhood whose closure is compact.

Equivalently, every point has a neighborhood basis of compact sets. The space is locally compact if it is locally compact at every point.

> **Locally compact means: zooming in around any point, you can always find a small region that is "finite-like" in the compact sense.**

Note that of course it is not saying the whole space is compact — $$\mathbb{R}^n$$ is locally compact but not compact. It is saying compactness is available locally, on demand, around any point.

Then why are topological manifolds locally compact? Take any $$p \in M$$. By the locally Euclidean condition, there exists a chart $$(U, \phi)$$ with $$p \in U$$ and:
$$\phi: U \xrightarrow{\sim} \tilde{U} \subseteq \mathbb{R}^n$$

Since $$\phi(p) \in \tilde{U}$$ and $$\tilde{U}$$ is open in $$\mathbb{R}^n$$, we can find a closed ball:
$$\overline{B} = \overline{B_\epsilon(\phi(p))} \subseteq \tilde{U}$$

for some small $$\epsilon > 0$$. This ball is compact by Heine-Borel (closed and bounded in $$\mathbb{R}^n$$).

Pull it back via $$\phi^{-1}$$:
$$K = \phi^{-1}(\overline{B}) \subseteq U \subseteq M$$

Since $$\phi$$ is a homeomorphism and $$\overline{B}$$ is compact, $$K$$ is compact. And $$p \in \phi^{-1}(B_\epsilon(\phi(p))) = \text{int}(K)$$, so $$p$$ is in the interior of $$K$$. Thus $$p$$ has a neighborhood with compact closure. $$\blacksquare$$. 

To summarize: 

$$
\underbrace{\text{Locally Euclidean}}_{\text{manifold condition}}
\Rightarrow
\underbrace{\text{small closed balls exist around every point}}_{\text{in } \mathbb{R}^n \text{ via Heine-Borel}}
$$

$$
\Rightarrow
\underbrace{\text{pull back to compact neighborhoods on } M}_{\text{via homeomorphism}}
$$

| Ingredient | Role |
|---|---|
| Locally Euclidean | Gives a homeomorphism to $\mathbb{R}^n$ locally |
| Heine-Borel | Provides compact sets in $\mathbb{R}^n$ (closed balls) |
| Homeomorphism | Transfers compactness back to $M$ |

Heine-Borel is doing the heavy lifting inside $$\mathbb{R}^n$$ — and the locally Euclidean condition is what lets us borrow that power. We could then ask: what nice properties does local compactness give us? Here are a few that we might explore in more details later on: 

**1. Exhaustion by compact sets**
On a connected manifold:
$$M = \bigcup_{i=1}^\infty K_i, \quad K_i \subseteq \text{int}(K_{i+1}), \quad K_i \text{ compact}$$

This is called a **compact exhaustion**. It lets you study global properties of $$M$$ by working on compact pieces and taking limits.

**2. Partitions of unity**
The existence of partitions of unity relies on local compactness together with second countability. This is foundational for defining integrals, Riemannian metrics, and connections globally on a manifold (we will heavily rely on this for many proofs later on).

**3. Proper maps and geodesic completeness**
Local compactness is implicit in many arguments about geodesics and completeness. The Hopf-Rinow theorem, for instance, which characterizes when a Riemannian manifold is geodesically complete, uses the locally compact structure of the manifold.

Up to this point, it is worth being sharp about the distinction between global compactness and local compactness: 

| Property | Meaning | Example |
|---|---|---|
| Compact | Every open cover has finite subcover | $S^n$, closed bounded subsets of $\mathbb{R}^n$ |
| Locally compact | Every point has a compact neighborhood | $\mathbb{R}^n$, any manifold |
| Neither | No compact neighborhoods anywhere | $\mathbb{Q}$ with subspace topology |

To summarize, a manifold is locally compact because every point has a chart homeomorphic to $$\mathbb{R}^n$$, inside which you can always fit a closed ball — compact by Heine-Borel — and pull it back to a compact neighborhood on the manifold. Local compactness is the manifold inheriting, point by point, the "finite-like control in small regions" that $$\mathbb{R}^n$$ possesses.

---

#### 4.10] Local Compactness vs. Paracompactness
Finally, I was once confused on how the concepts of local compactness and paracompactness are different and similar. How are they employed similarly/differently for, e.g. partitions of unity, or geodesic completeness as we illustrated above? 

Let's state the two concepts cleanly and clearly: 

**Local compactness** (as established): every point $$p \in M$$ has a neighborhood with compact closure. It is a *pointwise*, *local* condition.

**Paracompactness**: every open cover $$\{U_\alpha\}$$ of $$X$$ has a *locally finite* open refinement $$\{V_\beta\}$$ — meaning:
- Each $$V_\beta$$ is contained in some $$U_\alpha$$ (refinement)
- Every point $$p \in X$$ has a neighborhood meeting only finitely many $$V_\beta$$ (locally finite)

Paracompactness is a *global* condition — it controls how open covers can be refined across the entire space. Also notice that the concept of a subcover is a special kind of refinement. 

Conceptually, these two differ in the following sense:

> **Local compactness** says: *around each point, the space is finite-like in size.*
>
> **Paracompactness** says: *any attempt to cover the whole space can be organized so that no point is overwhelmed by infinitely many covering sets.*

Local compactness is about the geometry near a point, while paracompactness is about global organizational control over open covers. Also, they are independent in general topological spaces:

- $$\mathbb{R}^n$$ is both locally compact and paracompact
- The long line is locally compact but **not** paracompact
- $$\mathbb{Q}$$ with the subspace topology is paracompact but **not** locally compact

For manifolds, both hold simultaneously. But wait, paracompactness is a global property. How would mnifolds acquire paracompactness given that it's only locally Euclidean? Well, unlike local compactness, paracompactness does __not__ follow from the locally Euclidean condition alone. It requires **second countability**!

> A second countable locally compact Hausdorff space is paracompact.

The proof idea: second countability gives a countable basis, from which we could build a compact exhaustion $$M = \bigcup K_i$$. From this exhaustion we can refine any open cover into a locally finite one by working inductively over the compact shells $$K_{i+1} \setminus \text{int}(K_i)$$. Consequently, the full manifold package — locally Euclidean + Hausdorff + second countable — gives us both local compactness and paracompactness together. Second countability is the ingredient that promotes local compactness to global paracompactness.

$$\underbrace{\text{Locally Euclidean}}_{\Rightarrow \text{ locally compact}} + \underbrace{\text{Second countable + Hausdorff}}_{\text{global control}} \Rightarrow \text{paracompact}$$

A side note on Partitions of Unity, which requires both local compactness and paracompactness: A **partition of unity** subordinate to an open cover $$\{U_\alpha\}$$ is a collection of smooth functions $$\{\psi_\alpha\}$$ with:
- $$0 \leq \psi_\alpha \leq 1$$
- $$\text{supp}(\psi_\alpha) \subseteq U_\alpha$$
- The supports are locally finite
- $$\sum_\alpha \psi_\alpha = 1$$ everywhere

Now watch how the two concepts contribute differently:

**Local compactness:** it guarantees that around each point we can find a bump function — a smooth function that is $$1$$ near $$p$$ and $$0$$ outside a compact neighborhood. Without compact neighborhoods, we cannot build bump functions (bump functions need compact support to exist).

**Paracompactness:** it guarantees the sum $$\sum_\alpha \psi_\alpha$$ is well-defined and equals $$1$$ everywhere. This requires the supports to be locally finite — every point is touched by only finitely many $$\psi_\alpha$$, so the sum is finite at each point. Neither alone suffices. Local compactness without paracompactness gives us local bumps we cannot organize globally. Paracompactness without local compactness gives us organizational control but no bumps to organize.

The example of geodesic completeness is a more subtle picture and I'm just scratching the surface here. **Local compactness** is directly relevant to geodesic completeness through the **Hopf-Rinow theorem**:

> On a connected Riemannian manifold, the following are equivalent:
> - $M$ is geodesically complete (geodesics extend for all time)
> - $M$ is complete as a metric space
> - Closed bounded subsets of $M$ are compact

The third condition — closed bounded subsets being compact — is essentially a global upgrading of local compactness. Local compactness gives us compact neighborhoods at each point; Hopf-Rinow asks whether this extends to all closed bounded sets globally. The proof of Hopf-Rinow uses local compactness at its core: geodesics are extended step by step using compact neighborhoods, and the argument that they extend globally uses the compactness of closed balls in the metric space.

**Paracompactness** is not directly involved in geodesic completeness — completeness is a metric/geometric condition, not a covering condition. However paracompactness enters indirectly: It guarantees the existence of a Riemannian metric in the first place (via partitions of unity patching local metrics together). Without a well-defined global Riemannian metric, the question of geodesic completeness cannot even be posed. 

Consequently, for geodesic completeness, local compactness is directly used in extending geodesics (Hopf-Rinow), whereas paracompactness serves as a prerequisite, needed to construct the Riemannian metric globally. 

We could summarize these two concepts by the diagrams below: 

$$\boxed{\text{Local compactness}} \xrightarrow{\text{pointwise}} \text{bump functions, geodesic extension}$$

$$\boxed{\text{Paracompactness}} \xrightarrow{\text{global organization}} \text{locally finite sums, global metric construction}$$

$$\text{Both together} \Rightarrow \text{partitions of unity} \Rightarrow \text{Riemannian metrics, integration, connections}$$

> **Local compactness** gives you *geometric substance* at each point — compact neighborhoods, bump functions, local Heine-Borel.
>
> **Paracompactness** gives you *global coherence* — the ability to assemble local data into globally well-defined objects without infinite overcounting.

The deepest summary is that local compactness gives us geometric substance at each point — compact neighborhoods, bump functions, local Heine-Borel, whereas paracompactness gives us global coherence — the ability to assemble local data into globally well-defined objects without infinite overcounting. On a manifold, we always have both, and most of the hard work in differential geometry is precisely this __assembly process__: take something defined locally in each chart, and use partitions of unity (which need both concepts) to glue it into something global.


---

### 8] The real arena: Smooth manifold
Given $$f: u \subset \mathbb{R}^n \rightarrow \mathbb{R}$$, where $$u$$ is an open set, $$f$$ is $$C^k$$ if the $$k$$th derivative $$\frac{\partial^k f}{\partial x_j^k}$$ exists and is continuous $$\forall 1 \leq j \leq n$$. Likewise, for a multivariable function $$f: u \subset \mathbb{R}^n \rightarrow \mathbb{R}^m$$, $$f = (f_1, f_2, \dots f_m)$$ is $$C^k$$ if $$f_i$$ is $$C^k \; \forall i$$. If we take $$k$$ to be $$\infty$$, then $$f$$ is called a smooth. In my blogs, whenever any function is called differentiable without specifying the $$k$$th order, we will assume $$k$$ as $$\infty$$ and thus a differentiable function is the same as a smooth function. A differentiable manifold is the same as the smooth manifold, both I'll use interchangeably. 

There's a another way to define smooth of functions in a coordinate-free manner (with linear approximation other than calculating derivatives (See
Extra Notes # 2.))

Another important concept is called diffeomorphism. Diffeomorphism in $$\mathbb{R}^n$$ goes like this: suppose $$\Phi: u \in \mathbb{R}^n \rightarrow v \in \mathbb{R}^n$$ 

$$u, v$$ both being open subsets. $$\Phi$$ is a diffeomorphism if $$\Phi$$ is smooth, $$\Phi$$ exists, and $$\Phi^{-1}$$ is also smooth. In that case, if $$y = \Phi(x)$$, then the matrix $$(\frac{\partial y_i}{\partial x_j})$$ is invertible. If a function is a diffeomophism, then does it have to be a homeomorphism? 


The definiton of homeomorphism and charts allow us to pull functional analysis from $$C^{\infty}(M)$$ or $$C^{\infty}(M, N)$$ on $$M$$ into $$\mathbb{R^n}$$ itself and thus we could proceed with techqniues built within the Euclidean space. Later when defining tangent/cotangent space from the geometric standpoint, we will see another side of the same story. 

The point of adding smooth structure to a topological manifold: this enables us to characterize a real-valued function $$f$$ on $$M$$ to be smooth or not ---- for a chart $$(U, \Phi)$$, $$f$$ is smooth when it composed with the inverse map $$\Phi^{-1}: \Phi(U) \rightarrow U$$: $$f \circ \Phi^{-1}: \Phi(U) \subset \mathbb{R}^n \rightarrow \mathbb{R}$$ is smooth (the composed function $$f \circ \Phi^{-1}$$ maps from Euclidean space to Euclidean space so we could solicit the familiar definitions of smoothness/differentiability). Careful readers might have spotted a subtle issue: the above definition of smoothness (the composed function) hinges upon the specific selection of chart. So we are led to use
only some charts, not all charts. 

---
One core/practical difference between homeomorphism vs diffeomorphism is that in general, **a homeomorphism does *not* imply that \( dF \) is invertible**.

In fact, there are two separate issues:

* 1. A homeomorphism may not even be differentiable:

A homeomorphism only preserves topology (continuity + continuous inverse). It says nothing about smoothness. Consequently, $$ F $$ might **not be differentiable at all**, in which case $$ dF_p $$ is simply **undefined**.

* 2. Even if differentiable, $$ dF_p $$ can fail to be invertible. Even if we assume $$ F $$ is differentiable, invertibility of the derivative can fail badly.

A classic example is:
$$
F : \mathbb{R} \to \mathbb{R}, \quad F(x) = x^3.
$$

where $$ F $$ is a **homeomorphism** (continuous, bijective, continuous inverse $$ x^{1/3} $$). Actually, $$ F $$ is smooth, but:

$$
dF_0 = 3x^2\big|_{x=0} = 0.
$$

So at $$ x = 0 $$, the derivative is **not invertible**.

Why does this happen?

A homeomorphism can:
- “flatten” directions (derivative loses rank),
- “slow down” arbitrarily (derivative → 0),
- without breaking continuity or invertibility at the topological level.

In some sense, topology is **too weak** to control infinitesimal behavior.

The key takeaway is that **Diffeomorphism ⇒ $$ dF $$ invertible everywhere** and **Homeomorphism ⇏ differentiability ⇏ invertible derivative**.


---

### 9] A Few Examples 

#### Cartesian Products of Manifolds and the Torus

Let's do an interesting example: the Cartesian product of manifolds. Here below I'll explain what it really means, why the torus is a product manifold, and why this can feel conceptually confusing at first.

#### What the Cartesian Product of Manifolds Really Means

If $$M$$ and $$N$$ are smooth manifolds, then

$$
M \times N = \{(p,q) \mid p \in M, q \in N\}
$$

is itself a manifold with

$$
\dim(M \times N) = \dim M + \dim N.
$$

Each coordinate varies independently:

- We can move in the $M$-direction without changing $N$.
- We can move in the $N$-direction without changing $M$.

This is what **independent** really means. Yet it does **not** mean geometrically separate in space. It means the coordinates (degrees of freedom) are independent.

---

#### The Torus Example

The standard torus is

$$
T^2 = S^1 \times S^1.
$$

So a point on the torus is

$$
(\theta_1, \theta_2),
$$

where:

- $\theta_1$ = angle around the big circle
- $\theta_2$ = angle around the tube

These two angles vary independently and this is exactly a Cartesian product.

---

##### Visualizing the Torus

There are two important ways to think about the torus:
> View 1: Embedded in $\mathbb{R}^3$

--- It looks like a donut surface in 3D space. But that __embedding__ is **not** what defines it as a product (I'll talk more about what __embedding__ represents later). The product structure exists abstractly, independent of how we draw it.

> View 2: Square with Opposite Sides Glued

Start with the square

$$
[0,2\pi] \times [0,2\pi].
$$

Then identify:

- left edge with right edge
- bottom edge with top edge

This construction is literally $$S^1 \times S^1$$. So the torus is not two separate circles floating apart. It is the space of ordered pairs of points on two circles.

---

##### Why This Feels Confusing

It’s natural to think:

> If they’re independent, shouldn’t they be separate?

But independence in product manifolds means:

- The local coordinates split.
- The tangent spaces split.

At each point $$(p,q)$$:

$$
T_{(p,q)}(M\times N) \cong T_pM \oplus T_qN.
$$

So at each point on the torus:

- One tangent direction corresponds to motion along the first circle.
- One corresponds to motion along the second circle.

Those directions are geometrically distinct, even though they lie on the same connected surface.

---

##### A Key Insight

A product manifold does **not** mean:

- The pieces are spatially separated.

It means:

- The geometry factorizes.
- The degrees of freedom split.
- The tangent spaces split as direct sums.

The torus is globally connected, but its structure decomposes:

$$
T^2 = S^1 \times S^1.
$$

Its topology and smooth structure split into two independent circular directions.

---

##### Compare with Something That Is NOT a Product

The sphere $$S^2$$ is **not**

$$
S^1 \times S^1.
$$

Even though both are 2-dimensional.

Why not? Because:

- On the torus, we can move in two globally independent circular directions.
- On the sphere, we cannot globally split motion into two independent circles.

Their topology is fundamentally different.

For example, the torus has two independent non-contractible loops, while the sphere has none.

---

### Quick Summary

The Cartesian product of manifolds means:

> The space is built from independent degrees of freedom.

It does **not** mean:

> The pieces live separately in space.

The torus is the perfect example of this distinction.

On a side note if you are obsessed with Why $$S^2$ is not a product, you may want to check fundamental groups. Also, later we will cover how product metrics work on $$M \times N$$, and how curvature behaves on product manifolds when we start to discuss Riemannian Manifolds.
