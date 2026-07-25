---
layout: post
title: "Manifold and Riemannian Geometry (2): Tangent/Cotangent Space"
date: 2025-09-17 23:30:02
description: Intuition and definition of tangent/cotangent space
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

This is episode 2 on the smooth manifold series. Today we will be diving into concepts that appear initially very intuitive at first glance, but then the extended version of which is indeed quite abstract. 

## 0] Familiar examples

Let me start with two simple concrete examples to illustrate what tangent vectors and tangent space are. Indeed they match up to our intuition!

Let's first say that we have a unit circle in $$\mathbb{R}^2$$, or basically let's denote it as $$S^1$$ (this is a standard notation as 

$$
S^{n} = \{ x \in \mathbb{R}^{n+1} \mid |x| = 1 \}
$$, 

representing the surface of an $$(n+1)$$-dimensional ball). Pick $$p = (1, 0)$$. If I ask you what the tangent vector is starting at $$p$$ and tangnet to $$S^1$$? Your answer is probably a vector pointing upward or downward with its tail at $$p$$. Indeed, 

[TODO: complete the $$S^1$$ picture and add a second concrete example.]

---

## 1] The algebraic scaffolding

Before we can even *state* the modern definition of a tangent vector, we need a small amount of linear-algebraic vocabulary. The definition we are heading toward reads:

> A tangent vector at $$p$$ is a linear functional on $$C^\infty(M)$$ satisfying the Leibniz rule.

Every noun in that sentence deserves scrutiny. What makes $$C^\infty(M)$$ a vector space? What is a linear functional, and why is it called a *functional*? What does it mean to say that the Leibniz rule "carves out a subspace"? This section answers all of these before we get to the geometry.

### 1.1] Why is $$C^\infty(M)$$ a vector space?

$$C^\infty(M)$$ is the set of smooth real-valued functions on $$M$$:

$$
C^\infty(M) = \{f : M \to \mathbb{R} \mid f \text{ is smooth}\}.
$$

To call this a vector space over $$\mathbb{R}$$, we need to define addition and scalar multiplication on it, and check the vector space axioms.

**Addition.** Given $$f, g \in C^\infty(M)$$, define their sum **pointwise**:

$$
(f + g)(p) := f(p) + g(p) \quad \text{for every } p \in M.
$$

The right-hand side is just addition of real numbers, which we already know how to do. The result is a new function $$f + g : M \to \mathbb{R}$$. We also need to check this new function is *still smooth* — which it is, because the sum of smooth functions is smooth (a standard fact from calculus, transported to manifolds via charts).

**Scalar multiplication.** Given $$c \in \mathbb{R}$$ and $$f \in C^\infty(M)$$, define $$cf$$ pointwise:

$$
(cf)(p) := c \cdot f(p).
$$

Again, the result is smooth.

**The axioms.** All the vector space axioms (commutativity of addition, associativity, distributivity, existence of a zero, etc.) follow immediately from the corresponding properties of $$\mathbb{R}$$, because everything is defined pointwise. The zero vector is the constant function $$0$$; the additive inverse of $$f$$ is the function $$-f$$.

So $$C^\infty(M)$$ is a vector space. It's just an **infinite-dimensional** one — there's no finite basis, because there are "too many" smooth functions on a manifold to span by a finite list.

This is the general pattern: any set of functions from some domain into a vector space, closed under pointwise addition and scalar multiplication, is itself a vector space.

### 1.2] Dual spaces and linear functionals

For any real vector space $$V$$, the **dual space** $$V^*$$ is defined as

$$
V^* = \{\text{linear maps } V \to \mathbb{R}\}.
$$

Its elements are called **linear functionals** on $$V$$. The dual space is itself a vector space under pointwise operations: if $$\varphi, \psi \in V^*$$ and $$a, b \in \mathbb{R}$$, then $$a\varphi + b\psi$$ is the functional defined by

$$
(a \varphi + b \psi)(v) = a\,\varphi(v) + b\,\psi(v).
$$

A few quick facts worth knowing:

- If $$V$$ is **finite-dimensional**, then $$\dim V^* = \dim V$$. In fact, given a basis $$\{e_1, \dots, e_n\}$$ of $$V$$, you get a dual basis $$\{\varepsilon^1, \dots, \varepsilon^n\}$$ of $$V^*$$ defined by $$\varepsilon^i(e_j) = \delta^i_j$$.
- If $$V$$ is **infinite-dimensional**, then $$V^*$$ is "even bigger" — its dimension is typically strictly larger than $$V$$'s. For something like $$C^\infty(M)$$, the dual space is genuinely enormous: it contains evaluation functionals at every point, integration against every measure, all derivatives at every point in every direction, distributions, and much more.

Applying this with $$V = C^\infty(M)$$: by definition,

$$
(C^\infty(M))^* = \{\text{linear maps } C^\infty(M) \to \mathbb{R}\}.
$$

An element $$\varphi \in (C^\infty(M))^*$$ is a function that:

- takes a smooth function $$f \in C^\infty(M)$$ as input,
- outputs a real number $$\varphi(f) \in \mathbb{R}$$,
- and is linear: $$\varphi(af + bg) = a \varphi(f) + b \varphi(g)$$ for all $$a, b \in \mathbb{R}$$ and $$f, g \in C^\infty(M)$$.

Examples:

- $$\mathrm{ev}_p(f) = f(p)$$ — linear (because $$(af + bg)(p) = af(p) + bg(p)$$). ✓
- $$\int_M f \, d\mu$$ — linear (linearity of integration). ✓
- The derivation $$D_v|_p f = \frac{d}{dt}\big|_{0} f(p + tv)$$ — linear (differentiation is linear). ✓
- $$f \mapsto f(p)^2$$ — **not** linear. ✗
- $$f \mapsto \int f^2 \, d\mu$$ — **not** linear. ✗

The linear ones live in $$(C^\infty(M))^*$$. The nonlinear ones don't.

Looking ahead: the tangent space $$T_pM$$ will turn out to be a **strict, very small subspace** of the full dual space $$(C^\infty(M))^*$$. The dual space is enormous (infinite-dimensional); $$T_pM$$ has dimension $$n$$. The Leibniz rule is an extremely restrictive condition.

#### 1.2.1] Why the word "functional"?

This is a terminology question, and it's a fair one to ask, because the word "functional" sounds redundant — aren't all maps functions?

The convention is this:

- **Function**: any map between any two sets. Generic word.
- **Functional**: a function whose **codomain is the scalar field** ($$\mathbb{R}$$ or $$\mathbb{C}$$), and typically whose **domain is itself a space of functions** (or more generally, an abstract/infinite-dimensional vector space).

So a "linear functional" is a linear map *to the scalars*. The term comes from classical analysis, where people studied operations like "given a function $$f$$, return $$\int f \, dx$$" — operations on functions that produce numbers. To distinguish these from functions-of-numbers, mathematicians started calling them **functionals**.

A few sharpening points:

- The term is most often used when the domain $$V$$ is infinite-dimensional (a space of functions, sequences, distributions, etc.), but the definition works for finite-dimensional $$V$$ too. People will still call a linear map $$\mathbb{R}^3 \to \mathbb{R}$$ a "linear functional," even though "function" would be technically fine — the word "functional" emphasizes that the codomain is $$\mathbb{R}$$.
- "Functional" is **not** synonymous with "function." It's a specialized word for a specific kind of function. A map $$\mathbb{R}^3 \to \mathbb{R}^3$$ is not a functional (codomain isn't $$\mathbb{R}$$). A map $$C^\infty(M) \to C^\infty(M)$$ (like the operator $$f \mapsto f''$$ on $$\mathbb{R}$$) is not a functional either — it's an **operator**.

So the full hierarchy is:

| Term | Domain | Codomain |
|------|--------|----------|
| Function | anything | anything |
| Functional | (usually) a function/vector space | scalar field ($$\mathbb{R}$$ or $$\mathbb{C}$$) |
| Operator | a function/vector space | a function/vector space |
| Linear functional | vector space | scalar field, *and* linear |

#### 1.2.2] Two natural follow-up questions

**Q: If $$V$$ is not a vector space, could we still talk about its dual $$V^*$$?**

No, not in the standard sense. The definition $$V^* = \{\text{linear maps } V \to \mathbb{R}\}$$ only makes sense if $$V$$ has a vector space structure, because "linear map" requires:

- $$V$$ to have addition (so we can talk about $$f(v + w) = f(v) + f(w)$$),
- $$V$$ to have scalar multiplication (so we can talk about $$f(cv) = c f(v)$$).

Without these operations on $$V$$, the word "linear" has no meaning, so neither does $$V^*$$.

There are generalizations in other contexts — for instance, for a group $$G$$ you can define its **dual** as the set of homomorphisms $$G \to \mathbb{C}^\times$$ (the Pontryagin dual), and for a module over a ring $$R$$ you can define an $$R$$-linear dual. But these are different structures, and the word "dual" gets reinterpreted each time. The common thread: you always need *some* algebraic structure on $$V$$ to define a corresponding notion of "structure-preserving map to scalars."

So the short answer is: $$V$$ being a vector space is exactly what makes $$V^*$$ a meaningful object.

**Q: If $$V$$ is not a space of functions but still a vector space, are elements of $$V^*$$ still called linear functionals?**

Yes. The convention has loosened over time. Today, "linear functional" is the standard name for any linear map $$V \to \mathbb{R}$$ (or $$V \to \mathbb{C}$$), regardless of what $$V$$ is.

So:

- A linear map $$\mathbb{R}^3 \to \mathbb{R}$$ like $$(x, y, z) \mapsto 2x + 3y - z$$: linear functional. ✓
- A linear map $$M_n(\mathbb{R}) \to \mathbb{R}$$ like $$A \mapsto \mathrm{trace}(A)$$: linear functional. ✓
- A linear map $$\ell^2(\mathbb{N}) \to \mathbb{R}$$ like $$(x_n) \mapsto \sum x_n/2^n$$: linear functional. ✓
- The derivation $$v \in T_pM$$ acting on $$C^\infty(M)$$: linear functional. ✓

Historically the word was reserved for cases where $$V$$ was a space of functions (which is why it's "functional" rather than "function" — to distinguish "functions whose inputs are functions" from "functions whose inputs are numbers"). But the term has been generalized: now "functional" just signals "linear map to the scalar field," irrespective of what $$V$$ looks like.

Why did the terminology spread? Because conceptually, the *role* a linear functional plays — extracting a scalar from a vector in a linear way — is the same whether $$V$$ is $$\mathbb{R}^3$$ or $$C^\infty(M)$$. The unified terminology reflects that unified role.

### 1.3] What is a subspace?

A **subspace** of a vector space $$V$$ is a subset $$W \subseteq V$$ that is *itself a vector space* under the operations inherited from $$V$$.

The minimal checklist — usually called the "subspace test" — is:

**$$W$$ is a subspace of $$V$$ if and only if:**

1. **Non-empty (contains zero):** $$0 \in W$$.
2. **Closed under addition:** if $$w_1, w_2 \in W$$, then $$w_1 + w_2 \in W$$.
3. **Closed under scalar multiplication:** if $$w \in W$$ and $$c \in \mathbb{R}$$, then $$cw \in W$$.

These three conditions together are equivalent to a single combined condition:

> $$W$$ is a subspace of $$V$$ if and only if $$W$$ is non-empty and closed under linear combinations: for all $$w_1, w_2 \in W$$ and $$a, b \in \mathbb{R}$$, the combination $$aw_1 + bw_2$$ is also in $$W$$.

#### Why the checklist is so short

You might worry: "Doesn't $$W$$ also need to satisfy associativity, commutativity, distributivity, the existence of additive inverses, etc.?" — all those vector space axioms.

The beautiful thing is **no, those come for free**. Here's why:

- **Associativity** of addition: $$V$$ satisfies it, so any subset inherits it automatically. $$(w_1 + w_2) + w_3 = w_1 + (w_2 + w_3)$$ holds in $$V$$, hence in $$W$$.
- **Commutativity:** same reason.
- **Distributivity:** same reason.
- **Additive inverses:** if $$w \in W$$, then by closure under scalar multiplication with $$c = -1$$, we get $$-w = (-1)w \in W$$.
- **Zero:** if $$w \in W$$, then $$0 = 0 \cdot w \in W$$ by closure under scalar multiplication.

So the only things you actually need to check are that $$W$$ is non-empty and that doing operations on elements of $$W$$ keeps you inside $$W$$. The rest of the vector space axioms hold "for free" because they already hold in the ambient space $$V$$.

#### Examples

- $$\{(x, y, 0) : x, y \in \mathbb{R}\} \subset \mathbb{R}^3$$ — a subspace (the $$xy$$-plane).
- $$\{(x, y, z) \in \mathbb{R}^3 : x + y + z = 0\}$$ — a subspace (a plane through the origin).
- $$\{(x, y, z) \in \mathbb{R}^3 : x + y + z = 1\}$$ — **not** a subspace (doesn't contain $$0$$; also not closed under addition: $$(1,0,0) + (0,1,0) = (1,1,0)$$ has coordinate sum $$2$$, not $$1$$).
- $$\{(x, y) \in \mathbb{R}^2 : x^2 + y^2 \leq 1\}$$ — **not** a subspace (the closed unit disk; not closed under scalar multiplication, since $$2 \cdot (1, 0) = (2, 0)$$ has norm $$2 > 1$$).
- $$T_pM \subset (C^\infty(M))^*$$ — a subspace. This is the punchline of the next section.

---

## 2] Three equivalent definitions of Tangent Space

We'll cover three equivalent definitions of tangent space. 

### 2.1] Tangent vectors are equivalence classes of curves

The definiton of homeomorphism and charts allow us to pull functional analysis from $$C^{\infty}(M)$$ or $$C^{\infty}(M, N)$$ on $$M$$ into $$\mathbb{R^n}$$ itself and thus we could proceed with techqniues built within the Euclidean space. Later when defining tangent/cotangent space from the geometric standpoint, we will see another side of the same story. 

Tangent vectors are equivalence classes of smooth curves through $$p$$:

$$T_pM = \{\frac{d}{dt}\Big|_{t = 0} \gamma(t) \Big| \gamma: (\epsilon, \epsilon) \rightarrow M, \; \gamma(0) = p \}$$

where $$\gamma_1 \sim \gamma_2$$ if in some (equivalently, any) coordinate chart $$\phi: U \subset M \rightarrow \mathbb{R}^n$$:

$$\frac{d}{dt}\Big|_{t = 0} \phi(\gamma_1(t)) = \frac{d}{dt}\Big|_{t = 0} \phi(\gamma_2(t))$$

Consequently, each tangent vector is represented by the velocity of a curve through $$p$$. 

### 2.2] Tangent vectors are derivations at $$p$$

Let $$M$$ be a manifold, the tangent space at $$p \in M$$, denoted as $$T_pM$$, is the the vector space of all derivations at $$p$$. Notice that a derivation at $$p$$ is a __linear__ operator (at $$p$$):

$$
D: C^{\infty}_{p}(M) \rightarrow \mathbb{R}
$$

satisfying the Leibniz rule:

$$
D(fg) = f(p)D(g) + g(p)D(f), \; \forall f, g \in C^{\infty}(M)
$$

where $$C^{\infty}_{p}$$ is the equivalence class of $$(f, U)$$, where $$f \in C^{\infty}$$ and $$U$$ is a neighbourhood of $$p$$. Two functions $$(f, U)$$ and $$(g, V)$$ are equivalent iff $$\exist \; W \subset U \cap V$$ a neighbourhood of $$p$$ such that $$f(x) = g(x),  \forall x \in W$$, where $$g \in C^{\infty}$$ and $$V$$ is a neighbourhood of $$p$$. 

Intuitively, $$D$$ is like a directional derivative operator acting on smooth functions near $$p$$. Consequently, 

$$
T_p{M} = \{ D \mid D \; \mathrm{is\ a\ derivation\ at} p \}
$$ 

and a tangent vector $$v \in T_p{M}$$ is a derivation $$D$$. In fact, Tu (Theorem 2.2, *An Introduction to Manifolds*) showed that there's a bijection between derivations at $$p$$ and directional derivaties. 

### 2.3] Tangent vectors as equivalences on function germs

[TODO: write up the germ-theoretic definition. Note that the equivalence-class-of-$$(f,U)$$ construction already used in §2.2 *is* the germ construction, so this section is really about making that explicit and showing the derivation definition on germs agrees with the one on global functions.]

---

## 3] Is a derivation "just" a linear functional?

It's worth pinning down exactly how much of the derivation definition is linear-algebraic bookkeeping and how much is genuine content. Consider three claims.

> **(1)** A derivation is just a linear functional.
> **(2)** Every linear functional is a derivation.
> **(3)** A derivation has to satisfy the Leibniz rule, but this has nothing to do with the vector space structure of the collection of such derivations.

Let's take them one at a time.

### 3.1] "A derivation is just a linear functional"

**Half right.** Every derivation at $$p$$ is a linear functional $$v : C^\infty(M) \to \mathbb{R}$$, so the inclusion goes one way. But the word "just" is doing too much work. A derivation is a linear functional that *also* satisfies the Leibniz rule:

$$
v(fg) = f(p)\, v(g) + g(p)\, v(f).
$$

Without the Leibniz rule, you have a linear functional but not necessarily a derivation. The right way to say it: a derivation is a linear functional with an extra constraint.

### 3.2] "Every linear functional is a derivation"

**False.** This is the converse of (1), and most linear functionals on $$C^\infty(M)$$ fail the Leibniz rule. Two clear counterexamples:

**Evaluation at $$p$$.** Define $$\mathrm{ev}_p(f) = f(p)$$. This is a linear functional. But check Leibniz:

$$
\mathrm{ev}_p(fg) = f(p)g(p), \quad f(p)\,\mathrm{ev}_p(g) + g(p)\,\mathrm{ev}_p(f) = 2f(p)g(p).
$$

Unequal in general. So $$\mathrm{ev}_p$$ is a perfectly nice linear functional but not a derivation.

**Integration.** The functional $$f \mapsto \int_M f \, d\mu$$ (when $$M$$ is, say, compact) is linear, but $$\int fg \neq f(p)\int g + g(p)\int f$$ for any $$p$$. Definitely not a derivation.

So the space of derivations $$T_pM$$ is a strict, very small subspace of the full dual space $$(C^\infty(M))^*$$. The dual space is enormous (infinite-dimensional); $$T_pM$$ has dimension $$n$$. The Leibniz rule is an extremely restrictive condition.

### 3.3] "The Leibniz rule has nothing to do with the vector space structure"

**This one needs careful unpacking.** The claim is half right and half wrong, depending on what you mean.

**What's right:** The Leibniz rule is a condition imposed on *each individual derivation*. It's a property of one element, not a property of how elements combine.

**What's wrong (or at least misleading):** The Leibniz rule is *very much* tied to the vector space structure of $$T_pM$$, in a specific way. The point is this:

The Leibniz rule is a **linear condition** on linear functionals — meaning, if $$v$$ and $$w$$ both satisfy it, then so does $$av + bw$$ for any $$a, b \in \mathbb{R}$$.

This is **why** $$T_pM$$ is a vector space at all. The set of derivations is closed under linear combinations precisely because the Leibniz rule is preserved under them. If the Leibniz rule were not linear in this way — say, if it were instead some quadratic condition like $$v(f)^2 = \text{something}$$ — then the "set of things satisfying it" would not be a subspace, and $$T_pM$$ wouldn't inherit vector space structure from the dual.

So the cleanest statement is: **the Leibniz rule is what carves $$T_pM$$ out as a subspace of $$(C^\infty(M))^*$$**, and it does so precisely because the rule itself is linear in the derivation. The vector space structure on $$T_pM$$ is not separate from the Leibniz rule — it's compatible with it, and that compatibility is what allows the structure to exist at all.

**Summary of the three claims:**

- (1) True but understated: derivations are special linear functionals.
- (2) Outright false: most linear functionals aren't derivations.
- (3) Misleading: the Leibniz rule is intimately connected to vector space structure, because it's a *linear* condition that gets preserved under linear combinations. That's exactly why $$T_pM$$ is a vector space.

### 3.4] The verification, step by step

Setup: $$v$$ and $$w$$ are both derivations at $$p$$, meaning each satisfies

$$
v(fg) = f(p)v(g) + g(p)v(f), \qquad w(fg) = f(p)w(g) + g(p)w(f).
$$

We want to show: the linear combination $$av + bw$$ (for any real numbers $$a, b$$) is *also* a derivation at $$p$$. So we need to verify:

$$
(av + bw)(fg) \stackrel{?}{=} f(p)(av+bw)(g) + g(p)(av+bw)(f).
$$

**Step 1 — Unpack the left-hand side using the definition of $$av + bw$$.**

By definition, the linear combination $$av + bw$$ acts on any function $$h$$ by

$$
(av + bw)(h) = a\,v(h) + b\,w(h).
$$

Apply this with $$h = fg$$:

$$
(av + bw)(fg) = a\,v(fg) + b\,w(fg).
$$

**Step 2 — Use the Leibniz rule on $$v$$ and $$w$$ separately.**

We know $$v(fg) = f(p)v(g) + g(p)v(f)$$ and $$w(fg) = f(p)w(g) + g(p)w(f)$$. Plug those in:

$$
a\,v(fg) + b\,w(fg) = a\bigl[f(p)v(g) + g(p)v(f)\bigr] + b\bigl[f(p)w(g) + g(p)w(f)\bigr].
$$

**Step 3 — Distribute $$a$$ and $$b$$.**

$$
= a f(p)v(g) + a g(p)v(f) + b f(p)w(g) + b g(p)w(f).
$$

**Step 4 — Group the terms by their common factors $$f(p)$$ and $$g(p)$$.**

The first and third terms both have $$f(p)$$; the second and fourth both have $$g(p)$$:

$$
= f(p)\bigl[a\,v(g) + b\,w(g)\bigr] + g(p)\bigl[a\,v(f) + b\,w(f)\bigr].
$$

**Step 5 — Recognize the brackets.**

By the definition of $$av + bw$$ again (applied to $$g$$ and $$f$$ respectively):

$$
a\,v(g) + b\,w(g) = (av + bw)(g), \qquad a\,v(f) + b\,w(f) = (av + bw)(f).
$$

So the expression becomes

$$
f(p)(av+bw)(g) + g(p)(av+bw)(f).
$$

This is exactly the right-hand side of what we wanted to verify. ✓

#### What the calculation reveals

The verification works because the Leibniz rule is **linear in the derivation**. Look at the rule schematically:

$$
\underbrace{v(fg)}_{\text{linear in }v} = \underbrace{f(p)v(g) + g(p)v(f)}_{\text{also linear in }v}.
$$

Both sides are linear expressions in $$v$$ (with $$f, g, p$$ held fixed). So if $$v$$ and $$w$$ each make both sides equal, then any linear combination $$av + bw$$ also makes both sides equal. This is the structural reason $$T_pM$$ is a subspace.

Contrast: if the Leibniz rule were something nonlinear in $$v$$, like $$v(fg) = v(f) \cdot v(g)$$, the linearity argument would collapse — you'd compute $$(av+bw)(f)\cdot(av+bw)(g)$$ and get cross terms that aren't tidy, and the set of "things satisfying that rule" wouldn't be closed under addition. So it's no accident that Leibniz takes the form it does.

### 3.5] "Linear condition preserved under linear combinations" — unpacked

This is a phrase mathematicians use a lot, and it's worth unpacking carefully because it captures the structural idea behind subspaces.

#### A "condition" on elements

A **condition** is any property an element might or might not satisfy. Examples:

- "$$v \in \mathbb{R}^3$$ has first coordinate zero" — a condition on vectors in $$\mathbb{R}^3$$.
- "$$f \in C^\infty(\mathbb{R})$$ satisfies $$f(0) = 0$$" — a condition on smooth functions.
- "$$v \in (C^\infty(M))^*$$ satisfies Leibniz at $$p$$" — a condition on linear functionals.

Some elements satisfy the condition; others don't. The set of all elements satisfying the condition is a subset of the ambient space.

#### "Linear" condition

A condition is **linear** if it can be expressed as an equation where both sides are linear in the element under consideration. Equivalently — and this is the operational definition — if $$v$$ and $$w$$ both satisfy the condition, then so does any linear combination $$av + bw$$.

Examples of linear conditions:

- "$$v \in \mathbb{R}^3$$ has first coordinate zero" — linear. (If $$v_1 = 0$$ and $$w_1 = 0$$, then $$(av + bw)_1 = av_1 + bw_1 = 0$$.)
- "$$f$$ satisfies $$f(0) = 0$$" — linear. (If $$f(0) = 0$$ and $$g(0) = 0$$, then $$(af + bg)(0) = 0$$.)
- "$$v$$ satisfies Leibniz at $$p$$" — linear. (This is what we verified in §3.4.)

Examples of **nonlinear** conditions:

- "$$v \in \mathbb{R}^3$$ has unit length, $$|v| = 1$$" — nonlinear. (If $$|v| = 1$$ and $$|w| = 1$$, generally $$|av + bw| \neq 1$$.)
- "$$f$$ satisfies $$f(0) = 1$$" — affine, not linear. (If $$f(0) = 1$$ and $$g(0) = 1$$, then $$(f + g)(0) = 2$$.)
- "$$v$$ satisfies $$v(fg) = v(f)\cdot v(g)$$" — nonlinear in $$v$$ (the right side is quadratic).

#### Why "preserved under linear combinations" is the key phrase

In a vector space, the **only** operations you have are addition and scalar multiplication — i.e., taking linear combinations. So if a condition is preserved when you take linear combinations, then the set of elements satisfying it is **closed under** the vector space operations. That's exactly the definition of a subspace.

So the slogan "linear condition $$\Rightarrow$$ subspace" works like this:

> A linear condition picks out a subspace, because linear combinations of solutions are solutions.

And conversely:

> Every subspace can be described (locally, at least) as the solution set of some linear equations.

#### Applying it to $$T_pM$$

The Leibniz rule $$v(fg) = f(p)v(g) + g(p)v(f)$$ is a linear condition on $$v$$ (you can see this by inspection: $$v$$ appears linearly on both sides). So:

1. The set of $$v$$'s satisfying it is closed under linear combinations.
2. Therefore that set is a **subspace** of $$(C^\infty(M))^*$$.
3. That subspace is, by definition, $$T_pM$$.
4. $$T_pM$$ automatically inherits the vector space structure of $$(C^\infty(M))^*$$.

This is the whole reason $$T_pM$$ is a vector space at all. The Leibniz rule isn't just *compatible* with vector space structure — it's the *mechanism* by which $$T_pM$$ acquires that structure. If you replaced Leibniz with a nonlinear condition, the set you'd carve out wouldn't be a subspace, and tangent vectors couldn't be added or scaled in a meaningful way.

### 3.6] Three vector spaces and their operations

It is worth being completely explicit that $$C^\infty(M)$$, $$(C^\infty(M))^*$$, and $$T_pM$$ are all vector spaces, and about what the operations are in each case.

**$$C^\infty(M)$$ — vector space of smooth functions**

- **Elements:** smooth functions $$f : M \to \mathbb{R}$$.
- **Addition:** pointwise. $$(f + g)(p) := f(p) + g(p)$$.
- **Scalar multiplication:** pointwise. $$(cf)(p) := c \cdot f(p)$$.
- **Zero vector:** the constant function $$0$$.
- **Dimension:** infinite.

**$$(C^\infty(M))^*$$ — dual space**

- **Elements:** linear functionals $$\varphi : C^\infty(M) \to \mathbb{R}$$.
- **Addition:** pointwise on functionals. $$(\varphi + \psi)(f) := \varphi(f) + \psi(f)$$. Note "pointwise" here means "for every input $$f$$," not "for every point $$p \in M$$."
- **Scalar multiplication:** $$(c\varphi)(f) := c \cdot \varphi(f)$$.
- **Zero vector:** the functional that sends every $$f$$ to $$0$$.
- **Dimension:** infinite, and in fact "bigger" than $$C^\infty(M)$$ itself.

**$$T_pM$$ — tangent space at $$p$$**

- **Elements:** derivations $$v : C^\infty(M) \to \mathbb{R}$$ at $$p$$ (linear functionals satisfying Leibniz at $$p$$).
- **Addition:** inherited from $$(C^\infty(M))^*$$. $$(v + w)(f) := v(f) + w(f)$$.
- **Scalar multiplication:** inherited. $$(cv)(f) := c \cdot v(f)$$.
- **Zero vector:** the derivation that sends every $$f$$ to $$0$$ (which trivially satisfies Leibniz).
- **Dimension:** $$n$$, where $$n = \dim M$$.

The key point: **the operations on $$T_pM$$ are the same as the operations on $$(C^\infty(M))^*$$**, because $$T_pM$$ is a subspace of $$(C^\infty(M))^*$$. $$T_pM$$ doesn't get its own separate vector space structure invented from scratch — it inherits one. And the only thing that needs checking is that this inherited structure stays inside $$T_pM$$, i.e., that the Leibniz rule is closed under linear combinations.

This is the same pattern as: $$\mathbb{R}^2$$ is a vector space, and the $$x$$-axis (the subspace $$\{(x, 0) : x \in \mathbb{R}\}$$) is also a vector space, using the *same* addition and scalar multiplication as $$\mathbb{R}^2$$ — restricted to the $$x$$-axis. The subspace doesn't invent new operations; it just notes that the ambient ones preserve the subspace.

### 3.7] What if we replaced Leibniz with a nonlinear condition?

The set of solutions would not be a subspace, because at least one of the closure conditions would fail. Let me make this concrete with the example floated above. Suppose we replaced the Leibniz rule with the **multiplicative** condition:

$$
v(fg) = v(f) \cdot v(g).
$$

This looks superficially similar to Leibniz but is fundamentally different — the right side is **quadratic in $$v$$**, not linear.

Now define $$S = \{$$linear functionals $$v : C^\infty(M) \to \mathbb{R}$$ such that $$v(fg) = v(f)v(g)$$ for all $$f, g\}$$. Let's see what fails.

#### Closure under scalar multiplication fails

Suppose $$v \in S$$, i.e., $$v(fg) = v(f)v(g)$$. Consider $$2v$$. Does it satisfy the rule?

$$
(2v)(fg) = 2 \cdot v(fg) = 2\, v(f)\, v(g).
$$

$$
(2v)(f) \cdot (2v)(g) = 2v(f) \cdot 2v(g) = 4\, v(f)\, v(g).
$$

These are equal only if $$v(f)v(g) = 0$$ for all $$f, g$$, which forces $$v = 0$$. So unless $$v = 0$$, the functional $$2v$$ is **not in $$S$$**. Scalar closure broken.

#### Closure under addition also fails

Suppose $$v, w \in S$$. Is $$v + w \in S$$?

$$
(v + w)(fg) = v(fg) + w(fg) = v(f)v(g) + w(f)w(g).
$$

$$
(v + w)(f) \cdot (v + w)(g) = [v(f) + w(f)][v(g) + w(g)] = v(f)v(g) + v(f)w(g) + w(f)v(g) + w(f)w(g).
$$

For these to be equal we'd need the cross terms $$v(f)w(g) + w(f)v(g)$$ to vanish for all $$f, g$$. Generically they don't. Additive closure broken.

#### What's the solution set actually like?

$$S$$ still contains the zero functional and (it turns out) the evaluation functionals $$\mathrm{ev}_p(f) = f(p)$$, since $$f(p)g(p) = f(p)\cdot g(p)$$ is automatic. But it has no linear combinations of these — the set is a kind of "discrete" collection of functionals, not a vector space. Geometrically, $$S$$ would be more like a variety (a set cut out by polynomial equations) than a linear subspace.

This is why nonlinear conditions don't carve out subspaces: they carve out **algebraic varieties** or **manifolds** or some other curvy thing instead.

#### The general principle

A condition cuts out a subspace iff that condition is linear in the element. Concretely:

| Condition on $$v$$ | Right side's dependence on $$v$$ | Solution set |
|----------------|------------------------------|--------------|
| $$v(fg) = f(p)v(g) + g(p)v(f)$$ (Leibniz) | linear in $$v$$ | subspace ✓ (this is $$T_pM$$) |
| $$v(fg) = v(f)v(g)$$ (multiplicative) | quadratic in $$v$$ | not a subspace ✗ |
| $$v(f_0) = 0$$ for a fixed $$f_0$$ | linear in $$v$$ | subspace ✓ |
| $$v(f_0) = 1$$ for a fixed $$f_0$$ | affine in $$v$$, not linear | not a subspace ✗ (doesn't contain $$0$$) |
| $$\lVert v \rVert = 1$$ (in a normed space) | nonlinear in $$v$$ | not a subspace ✗ |

So the slogan again: **linear conditions $$\Rightarrow$$ subspaces; nonlinear conditions $$\Rightarrow$$ something curvier**.

And the deepest reason $$T_pM$$ gets to be a vector space is that the Leibniz rule, despite looking algebraically intricate, is in fact linear in the derivation $$v$$ — and that's exactly the structural fact that lets tangent vectors be added, scaled, and treated as elements of a finite-dimensional vector space at every point of $$M$$.

---

## 4] Why Leibniz? Why directional derivatives?

We now understand that we need a *linear* condition to carve a subspace out of the dual space. But that raises the obvious next question: why *this* linear condition? What is so special about Leibniz?

### 4.1] The wrong way to think about it

It's tempting to read the definition of a derivation as: "We needed a linear condition to carve out a subspace, and Leibniz happened to be one that worked." That framing makes Leibniz seem arbitrary — just some clever rule someone chose.

The right framing is the opposite: **we knew what tangent vectors should be from geometry, and Leibniz is the algebraic fingerprint that identifies them.** The condition isn't chosen for convenience; it's *discovered* by asking "what algebraic property does a directional derivative satisfy?"

### 4.2] The motivating calculation

Go back to the concrete case of $$\mathbb{R}^n$$, where you already know what a tangent vector is — an arrow at a point. Given a vector $$v \in \mathbb{R}^n$$ at the point $$a$$, the directional derivative of a smooth function $$f$$ in the direction $$v$$ is:

$$
D_v|_a f = \frac{d}{dt}\bigg|_{t=0} f(a + tv).
$$

This is a perfectly concrete, geometric operation: take $$f$$, walk along the straight line from $$a$$ in direction $$v$$, compute the rate of change at the starting point.

Now ask: **what properties does this operation have, viewed as a map $$C^\infty(\mathbb{R}^n) \to \mathbb{R}$$?**

Two pop out immediately.

**Property 1: Linearity in $$f$$.**

$$
D_v|_a(af + bg) = a\, D_v|_a f + b\, D_v|_a g.
$$

This follows directly from the linearity of the ordinary derivative $$d/dt$$.

**Property 2: The product rule.**

$$
D_v|_a(fg) = f(a)\, D_v|_a g + g(a)\, D_v|_a f.
$$

This is just the ordinary product rule from calculus, applied to the function $$t \mapsto f(a + tv) \cdot g(a + tv)$$ at $$t = 0$$:

$$
\frac{d}{dt}\bigg|_{t=0} f(a+tv)g(a+tv) = f(a) \cdot g'(0) + g(a) \cdot f'(0) = f(a) D_v|_a g + g(a) D_v|_a f.
$$

So **the Leibniz rule isn't imposed from outside — it's a *theorem* about directional derivatives**, inherited from the product rule of ordinary calculus.

### 4.3] The brilliant inversion

Here's where things become beautiful. Lee, Tu, and every modern textbook do something clever: instead of *starting* with arrows and *deriving* Leibniz as a property, they **start with Leibniz** and *derive* arrows.

The crucial result (Lee Proposition 3.2 / Tu Theorem 2.2) says:

> **The only linear functionals on $$C^\infty(\mathbb{R}^n)$$ satisfying the Leibniz rule at $$a$$ are the directional derivatives.**

In other words, "linear + Leibniz" is enough to **characterize** directional derivatives completely. There's nothing else satisfying these two conditions — no exotic operators, no weird non-geometric functionals. Linearity plus Leibniz forces you to be a directional derivative.

This is what makes the abstract definition work. We're not saying "Leibniz is one of many possible linear conditions, and we arbitrarily picked it." We're saying "Leibniz is *the* linear condition that singles out directional derivatives among all linear functionals."

### 4.4] Why this is exactly the right thing for manifolds

Now the whole picture clicks into place. We want to define tangent vectors on a general manifold $$M$$, where we have no ambient $$\mathbb{R}^n$$, no arrows, no straight lines, and no notion of "$$a + tv$$." What's left?

- We **do** have smooth functions $$C^\infty(M)$$.
- We **do** have the notion of linearity (because $$C^\infty(M)$$ is a vector space).
- We **do** have the notion of pointwise multiplication of functions (since the product of two smooth functions is smooth).

These three ingredients — smooth functions, linearity, and multiplication — are *exactly* what's needed to write down the Leibniz rule. So we transplant the algebraic characterization of directional derivatives to manifolds:

> A tangent vector at $$p \in M$$ is a linear functional $$v : C^\infty(M) \to \mathbb{R}$$ satisfying $$v(fg) = f(p)v(g) + g(p)v(f)$$.

The Leibniz rule survives the transplant because it only involves operations that exist on any manifold. The geometric picture (arrows, lines, $$a + tv$$) doesn't survive — but we don't need it, because Leibniz is enough to determine the same objects.

### 4.5] What's so special about Leibniz, specifically?

Several deep things, all pointing in the same direction:

**(a) Leibniz captures "first-order infinitesimal information."** A derivation measures the linear part of how $$f$$ changes near $$p$$. Higher-order behavior (curvature of $$f$$, second derivatives) doesn't appear, because Leibniz forces $$v$$ to "ignore" anything that vanishes to second order. Concretely: if $$f(p) = 0$$ and $$g(p) = 0$$, then $$v(fg) = f(p)v(g) + g(p)v(f) = 0$$. So products of functions vanishing at $$p$$ are killed by $$v$$. This is exactly the algebraic statement that $$v$$ sees only first-order behavior.

**(b) Leibniz is what makes the chain rule work.** When you have a smooth map $$F : M \to N$$ and you want to define a "differential" $$dF_p : T_pM \to T_{F(p)}N$$, the construction $$dF_p(v)(f) := v(f \circ F)$$ only produces a derivation at $$F(p)$$ because $$v$$ is a derivation at $$p$$. The whole functorial calculus of differentials — chain rule, naturality, etc. — *depends* on Leibniz being preserved. No other condition would give you this.

**(c) Leibniz is "coordinate-free."** This is what makes the abstract definition work on manifolds. The rule $$v(fg) = f(p)v(g) + g(p)v(f)$$ doesn't mention coordinates, charts, bases, or any choice. It's a relation between $$v$$, $$f$$, $$g$$, and the point $$p$$, expressed purely in terms of operations that any smooth manifold has.

**(d) Leibniz is the algebraic shadow of the geometric derivative.** Differential geometry has a recurring theme: every geometric operation that "differentiates" something — directional derivatives, Lie derivatives, covariant derivatives, exterior derivatives — satisfies some version of the Leibniz rule. It's the universal algebraic signature of "this is a first-order differential operator." Pick any reasonable notion of "derivative" in geometry, and the product rule will show up. So when you abstract away the geometry and keep only the algebra, Leibniz is what you're left holding.

### 4.6] Other linear conditions, other geometric objects

You might now wonder: *are there other linear conditions on functionals that single out interesting geometric objects?* Yes, and the answer reveals more structure:

- **Second-order derivations** (satisfying a modified Leibniz rule involving products of three functions) give rise to **second-order tangent vectors**, related to jets and higher-order infinitesimal neighborhoods.
- **Distributions** (vector subbundles of $$TM$$) are characterized by linear conditions on tangent vectors.
- **Vector fields tangent to a submanifold $$S \subseteq M$$** are exactly the derivations $$v$$ with $$v(f) = 0$$ for every $$f$$ vanishing on $$S$$ — a linear condition. (This is Lee's Proposition 5.37; we will meet it again when we discuss submanifolds.)

In each case, a linear condition cuts out a geometrically meaningful object. The Leibniz rule is just the simplest, most fundamental one: it carves out the most basic geometric object (the tangent space).

### 4.7] Summary: why Leibniz

Leibniz isn't special because it's *a* linear condition. It's special because it's **the** linear condition that:

- comes from the geometric product rule,
- characterizes directional derivatives uniquely (Lee 3.2 / Tu 2.2),
- makes sense on any smooth manifold without needing charts or coordinates,
- captures exactly the "first-order" infinitesimal behavior,
- is preserved by the differential of every smooth map,
- and consistently shows up as the algebraic shadow of *every* notion of "derivative" in geometry.

So when modern textbooks define a tangent vector as "a linear functional satisfying Leibniz," they're not being clever or arbitrary. They're encoding the entire geometric content of "tangent vector at $$p$$" into the cleanest possible algebraic statement — one that survives the leap from $$\mathbb{R}^n$$ to abstract manifolds.

### 4.8] But why *directional derivatives* in the first place?

There is one layer beneath even this. We abstracted the Leibniz rule *from* directional derivatives. But why were directional derivatives the right thing to abstract from? Is it because in $$\mathbb{R}^n$$ a vector is canonically identified with a directional derivative?

Yes — and that identification is precisely the bridge we use to transplant the geometric notion of "vector" into the abstract setting. Let me trace the logic carefully.

#### What we're trying to do

We want to define "tangent vector at $$p$$" on a manifold $$M$$. We have an intuitive picture from $$\mathbb{R}^n$$: a tangent vector is an arrow at a point. We need an *algebraic* definition that captures this intuition but works on a manifold where arrows don't make sense yet.

The strategy is: find some operation associated with arrows in $$\mathbb{R}^n$$ that:

- (a) is in canonical bijection with arrows (so no information is lost when we switch from arrows to this operation),
- (b) can be defined using only ingredients that exist on a general manifold.

Then we **define** tangent vectors on $$M$$ as instances of this operation. The arrows-on-$$\mathbb{R}^n$$ picture becomes a special case, and the same definition extends naturally to the abstract setting.

#### Why directional derivatives win

There are *many* things a vector $$v \in \mathbb{R}^n$$ at point $$a$$ "does." It points somewhere, has a length, has components in a basis, can be added to other vectors, can be the velocity of a curve, can be the directional derivative of functions, etc. Why pick directional derivatives as the thing to abstract?

Because of two facts that work in tandem:

**Fact 1: The map $$v \mapsto D_v|_a$$ is a bijection** between vectors at $$a$$ and directional-derivative operators at $$a$$ (this is Lee Prop 3.2, the surjectivity + injectivity argument). So no information about $$v$$ is lost when you replace it with $$D_v|_a$$. The arrow $$v$$ and the operator $$D_v|_a$$ are interchangeable.

**Fact 2: The operator $$D_v|_a$$ can be characterized algebraically (linearity + Leibniz), and those algebraic properties make sense on any smooth manifold.**

Notice what *doesn't* work:

- "$$v$$ has length $$1$$" — length requires a metric, which a general manifold lacks.
- "$$v$$ has components $$(v^1, \dots, v^n)$$" — components require a basis, which requires a chart, which is non-canonical.
- "$$v$$ is an arrow from $$a$$ to $$a + v$$" — this requires addition of points, which fails on manifolds.
- "$$v$$ is the velocity of a curve $$\gamma$$" — this works! And in fact it's an alternative definition (the "kinematic" definition of tangent vectors, our §2.1). It's equivalent to the derivation definition. Some textbooks use it, but the derivation definition tends to be algebraically cleaner.

So directional derivatives are picked because they're the **algebraically cleanest** thing about vectors that survives the transplant to manifolds. The bijection $$v \leftrightarrow D_v|_a$$ is the canonical identification, and Leibniz is what the operator side looks like once you strip away the arrow side.

#### A useful reframe

Think of it like this. The word "vector" in $$\mathbb{R}^n$$ refers to a single object with many facets:

- a geometric arrow,
- a column of numbers,
- a translation operator,
- a velocity of a curve,
- a directional-derivative operator.

These are all the same thing in $$\mathbb{R}^n$$. On a manifold, most of these facets dissolve — but the **directional-derivative facet** survives because it depends only on smooth functions and pointwise multiplication, both of which exist on any manifold.

So we pick that facet and call it "the" definition of a tangent vector on a manifold. We're not throwing away the geometric meaning — we're keeping the one algebraic shadow that still makes sense in the abstract setting, knowing it carries the full information of the original geometric object (by the canonical bijection).

---

## 5] What a general manifold does and does not have

Throughout the previous section I kept saying things like "a general manifold has no ambient $$\mathbb{R}^n$$, no arrows, no straight lines." These phrases deserve to be spelled out precisely, because they are the whole reason the abstract definition looks the way it does.

### 5.1] Is the manifold embedded?

In the abstract setup, **no, we do not assume the manifold is embedded in any $$\mathbb{R}^n$$**. The definition of a smooth manifold (Hausdorff, second-countable, locally Euclidean, plus a smooth atlas) makes no reference to any ambient space. The manifold is defined *intrinsically* — as its own self-contained object, glued together from chart data.

This is the whole point of abstract manifold theory: to **liberate** manifolds from their embeddings. The same abstract sphere $$S^2$$ can be embedded in $$\mathbb{R}^3$$ in many different ways (round sphere, dented sphere, weirdly stretched sphere, etc.), but as a smooth manifold it's one object. We want our definitions to depend on the manifold itself, not on a particular embedding.

A celebrated theorem (Whitney's embedding theorem) says that *every* smooth $$n$$-manifold can in fact be embedded in $$\mathbb{R}^{2n}$$. So the embedded picture is always available in principle — but using it would mean making an arbitrary choice of embedding, which would pollute our definitions with non-canonical data. The intrinsic approach is cleaner.

### 5.2] Does the manifold have "shape"?

Yes, but a more abstract kind of shape. It is worth laying out the hierarchy of structures explicitly.

A bare **topological space** has only the topological structure: a notion of which sets are open. This is enough to talk about continuity, connectedness, compactness, etc., but you can't talk about smoothness or tangent vectors with only this much structure.

A **topological manifold** is a topological space that is additionally locally Euclidean (every point has a neighborhood homeomorphic to an open subset of $$\mathbb{R}^n$$), Hausdorff, and second-countable. Now it has a definite *dimension* and the local "feel" of Euclidean space, but you still can't do calculus on it.

A **smooth manifold** is a topological manifold equipped with a **smooth atlas** — a collection of charts whose transition maps are smooth. *This* is the extra structure needed to do calculus, define tangent spaces, and talk about smooth functions. The smooth structure gives the manifold its "differential shape" — enough rigidity to distinguish smooth deformations from rough ones.

So the manifold isn't a totally formless thing. It has shape — just not "shape sitting in $$\mathbb{R}^n$$." It has:

- topological shape (open sets, connectedness),
- dimension (a definite number $$n$$ at every point),
- a smooth structure (which makes "smooth function on $$M$$" meaningful).

What it *lacks*, at the manifold level, is:

- a metric (no notion of distance — that requires a Riemannian metric on top),
- a preferred origin (no point is special),
- an embedding (no canonical position in any ambient space),
- linear structure (you can't add points of $$M$$, in general).

The tangent space is exactly the gadget that gives you "infinitesimal linear structure" at each point, even though the global manifold has none.

### 5.3] "No arrows"

In $$\mathbb{R}^n$$, an "arrow at $$a$$" is the picture you'd draw: a directed segment starting at $$a$$ and ending at $$a + v$$. To draw this arrow, you need:

- a place to start (the point $$a$$) ✓
- a notion of "starting at $$a$$ and going somewhere" — i.e., a displacement $$v$$ that you can add to $$a$$ ✗
- the ability to form $$a + v$$ as a new point ✗

On a manifold, you have points (✓), but the operation "$$+$$" of adding a displacement to a point doesn't exist. On the sphere $$S^2$$, what would the "$$+ v$$" mean? You can't add (North Pole) $$+$$ (some vector) and get another point of $$S^2$$ in any canonical way. There's no canonical addition of points and tangent vectors on a manifold.

So you can't literally draw an arrow on a manifold in the way you can in $$\mathbb{R}^n$$. You can draw a *picture* of a tangent vector (e.g., as an arrow visually emerging from the surface of a sphere, sticking out into $$\mathbb{R}^3$$), but that picture cheats — it uses the ambient $$\mathbb{R}^3$$ to interpret the arrow. Intrinsically, the arrow is not part of the manifold's data.

When we say "no arrows survive," we mean: the picture of a tangent vector as a literal geometric arrow at a point — defined by adding a displacement — doesn't have an analog in the abstract setting. What survives is the algebraic operator that the arrow corresponds to.

### 5.4] "No straight lines"

In $$\mathbb{R}^n$$, "the straight line through $$a$$ in direction $$v$$" is $$t \mapsto a + tv$$. We used this curve in the definition of directional derivative:

$$
D_v|_a f = \frac{d}{dt}\bigg|_{t=0} f(a + tv).
$$

On a manifold, the expression "$$a + tv$$" doesn't parse — same problem as with arrows. So we cannot literally compute directional derivatives by walking along a straight line, because there are no canonical straight lines.

(There's a deeper note here that we will return to much later in this series: a Riemannian manifold has **geodesics**, which generalize straight lines using metric structure. But that requires extra data — a Riemannian metric. On a bare smooth manifold, there are no preferred curves.)

### 5.5] What "doesn't survive" means

When we say "arrows and straight lines don't survive the transplant to manifolds," we mean: **these are concepts native to $$\mathbb{R}^n$$ that rely on its linear structure (addition of points, scalar multiplication of displacements). A general manifold has no linear structure globally, so these concepts have no direct analog there.**

Crucially, though, the *algebraic consequence* of these concepts in $$\mathbb{R}^n$$ — the Leibniz rule satisfied by directional derivatives — *does* survive. That rule only mentions:

- linear combinations of operators (which any vector space supports),
- pointwise multiplication of functions (which any smooth manifold supports),
- evaluation at a point (ditto).

None of those depend on linear structure of $$M$$ itself. So the algebra survives the transplant even though the geometric scaffolding (arrows, lines) doesn't.

### 5.6] The slogan

> On $$\mathbb{R}^n$$, tangent vectors have a geometric definition (arrows, lines, $$a + tv$$) and an algebraic definition (linear $$+$$ Leibniz functionals). These two definitions agree. On a general manifold, only the algebraic definition makes sense — but because it agreed with the geometric definition on $$\mathbb{R}^n$$, we trust that it captures the right notion in the abstract setting too.

So the move to abstract manifolds is a *retreat* from geometry into algebra, but it's a principled retreat: we keep exactly the parts of the structure that have intrinsic, coordinate-free meaning.

---

## 6] Local coordinate description

Specifically if we adopt interpretation §2.2, then with $$(U, \phi)$$ a chart with coordinates $$(x^1, \dots, x^n)$$ near $$p$$ 

$$
\{ \frac{\partial}{\partial x^1}\Big|_p, \dots, \frac{\partial}{\partial x^n}\Big|_p \}
$$ 

form a basis of $$T_pM$$. 

Thus any tangent vector $$v$$ can be written uniquely as

$$v = \sum_{i = 1}^{n} v^i \frac{\partial}{\partial x^i}\Big|_p$$

where $$(v^1, \dots, v^n)$$ are the components of the vector in this coordinate system.

This statement is used so casually that it is easy to miss how much machinery is hiding underneath it. The next two sections excavate that machinery: first, why a chart is a diffeomorphism at all (and hence why $$d\phi_p$$ is an isomorphism), and second, why we are allowed to use a chart defined only on $$U$$ to produce a basis of $$T_pM$$ rather than merely of $$T_pU$$.

---

## 7] Charts are diffeomorphisms, and $$d\varphi_p$$ is an isomorphism

### 7.1] The apparent puzzle

Here is a subtlety that is very easy to trip over. Recall:

- A chart on a *topological* manifold is just a homeomorphism $$\varphi : U \to \varphi(U) \subseteq \mathbb{R}^n$$.
- A *smooth* structure only directly constrains the **transition maps** $$\psi \circ \varphi^{-1}$$ to be diffeomorphisms (between open subsets of $$\mathbb{R}^n$$, where ordinary multivariable calculus applies).

So the smoothness condition is imposed on *compositions of charts*, never on a chart by itself. The puzzle: to even *ask* whether $$\varphi : U \to \varphi(U)$$ is a diffeomorphism, we need a notion of "smooth map" between $$U$$ and $$\varphi(U)$$ as **manifolds**, not just as topological spaces. Where does that come from, and why does $$\varphi$$ pass the test?

### 7.2] The key move: $$U$$ is itself a smooth manifold

The resolution starts by noticing that both the domain and codomain of $$\varphi$$ carry smooth structures:

- **$$\varphi(U)$$** is an open subset of $$\mathbb{R}^n$$, so it has its standard smooth structure (one global chart: the identity map).
- **$$U$$** is an open subset of $$M$$, so it inherits the **open submanifold** smooth structure from $$M$$. Its charts are the restrictions of $$M$$'s charts.

Now $$\varphi : U \to \varphi(U)$$ is a map between two honest smooth manifolds, so the general definition of "smooth map between manifolds" applies. Recall that definition: $$F : N \to P$$ is smooth if, for charts on the domain and codomain, the coordinate representation (chart on $$P$$) $$\circ\; F \;\circ$$ (chart on $$N$$)$$^{-1}$$ is smooth in the ordinary calculus sense.

### 7.3] Why $$\varphi$$ passes the test (Tu Proposition 6.10)

Here's the punchline. To test whether $$\varphi$$ is smooth, we get to **choose which charts to use** — and the natural choice is to use $$\varphi$$ *itself* as the chart on $$U$$, and the identity as the chart on $$\varphi(U)$$.

**Testing smoothness of $$\varphi$$:** Use the atlas $$\{(U, \varphi)\}$$ on the domain $$U$$, and the atlas $$\{(\varphi(U), 1_{\varphi(U)})\}$$ on the codomain. The coordinate representation of $$\varphi$$ is:

$$
1_{\varphi(U)} \circ \varphi \circ \varphi^{-1} = 1_{\varphi(U)},
$$

the identity map on $$\varphi(U)$$. The identity map is $$C^\infty$$. So $$\varphi$$ is smooth. ✓

**Testing smoothness of $$\varphi^{-1}$$:** Use the same two atlases. The coordinate representation of $$\varphi^{-1} : \varphi(U) \to U$$ is:

$$
\varphi \circ \varphi^{-1} \circ 1_{\varphi(U)} = 1_{\varphi(U)},
$$

again the identity, which is $$C^\infty$$. So $$\varphi^{-1}$$ is smooth. ✓

Both $$\varphi$$ and $$\varphi^{-1}$$ are smooth, and $$\varphi$$ is a homeomorphism (a bijection) to begin with — so $$\varphi$$ is a diffeomorphism onto its image. That's Tu's Proposition 6.10 exactly: *the coordinate map $$\varphi : U \to \varphi(U)$$ is a diffeomorphism.*

### 7.4] Why this isn't circular — and the deeper point

You might feel a flicker of suspicion: "Wait, we tested $$\varphi$$'s smoothness using $$\varphi$$ as the chart — isn't that rigged?" It is rigged, but legitimately so, and understanding why is the real lesson.

The definition of "smooth map between manifolds" is set up so that the answer is **independent of which compatible charts you use** to compute the coordinate representation. That independence is *guaranteed by the smooth compatibility of the atlas* — i.e., by the transition maps being diffeomorphisms. So if $$\varphi$$ comes out smooth using one choice of charts, it comes out smooth using any other compatible chart too. The transition-map condition is doing the work behind the scenes; it's what makes "use $$\varphi$$ as its own chart" a legitimate computation rather than a cheat.

Put differently: **the entire purpose of the smooth-compatibility (transition map) condition is precisely to make the charts themselves into diffeomorphisms.** That's not a side effect — it's the design goal. We define smooth structure by constraining transition maps because that's the minimal constraint that promotes every chart to a diffeomorphism, which in turn lets us pull the standard calculus of $$\mathbb{R}^n$$ back onto $$M$$ consistently. The charts being homeomorphisms gives you topology; the transition maps being smooth upgrades them to diffeomorphisms, which gives you calculus.

So the logical flow is:

1. Charts are homeomorphisms (topological manifold) — gives continuity.
2. Transition maps are diffeomorphisms (smooth structure) — the compatibility axiom.
3. *Consequence:* each chart $$\varphi$$ is now a diffeomorphism onto its image (Tu 6.10), because its coordinate representation in its own coordinates is the identity, and compatibility makes this chart-independent.
4. *Consequence:* $$d\varphi_p : T_pU \to T_{\varphi(p)}\varphi(U)$$ is a linear isomorphism (next subsection).

### 7.5] The converse (Tu Proposition 6.11)

There's a nice partial converse: if $$U \subseteq M$$ is open and $$F : U \to F(U) \subseteq \mathbb{R}^n$$ is *any* diffeomorphism onto an open subset of $$\mathbb{R}^n$$, then $$(U, F)$$ is automatically a chart in $$M$$'s maximal atlas. So "is a chart" and "is a diffeomorphism onto an open subset of $$\mathbb{R}^n$$" are, for open domains, the same thing (given the maximal atlas). This is the cleanest way to see that being a chart and being a diffeomorphism-to-$$\mathbb{R}^n$$ are two sides of the same coin.

### 7.6] Hence $$d\varphi_p$$ is an isomorphism

A chart $$(U, \varphi)$$ is, by definition, a diffeomorphism from $$U$$ onto its image $$\varphi(U)$$, an open subset of $$\mathbb{R}^n$$ (here $$U$$ carries its open-submanifold smooth structure). And **the differential of any diffeomorphism is an isomorphism.** The proof is a one-line chain-rule argument.

Let $$F$$ be a diffeomorphism with smooth inverse $$G = F^{-1}$$, so $$G \circ F = \mathrm{id}$$ and $$F \circ G = \mathrm{id}$$. Applying the chain rule and the fact that $$d(\mathrm{id})_p = \mathrm{id}$$:

$$
dG_{F(p)} \circ dF_p = d(G \circ F)_p = d(\mathrm{id})_p = \mathrm{Id}_{T_pM},
$$

$$
dF_p \circ dG_{F(p)} = d(F \circ G)_{F(p)} = \mathrm{Id}_{T_{F(p)}N}.
$$

So $$dF_p$$ has a two-sided inverse, namely $$dG_{F(p)}$$. A linear map with a two-sided inverse is an isomorphism. Applying this with $$F = \varphi$$ gives: **$$d\varphi_p : T_pU \to T_{\varphi(p)}\varphi(U)$$ is an isomorphism.** (This is Tu's Corollary 8.6 / Lee's Proposition 3.6(d).)

**A subtlety: image, not all of $$\mathbb{R}^n$$.** $$\varphi$$ is a diffeomorphism onto $$\varphi(U)$$, which is an *open subset* of $$\mathbb{R}^n$$, generally not all of $$\mathbb{R}^n$$. That's fine — $$\varphi(U)$$ is an open submanifold of $$\mathbb{R}^n$$, and $$T_{\varphi(p)}\varphi(U) \cong T_{\varphi(p)}\mathbb{R}^n$$ by the open-submanifold identification (§8). So nothing is lost; you just need that one extra identification to land in $$T_{\varphi(p)}\mathbb{R}^n$$.

Also worth a sanity check: an isomorphism requires equal dimensions. $$T_pM$$ has dimension $$n$$ (that's the dimension of the manifold), and $$T_{\varphi(p)}\mathbb{R}^n$$ has dimension $$n$$. The dimensions match, as they must.

### 7.7] The deeper structure: it's an "iff"

We phrased the above in one direction ("if local diffeomorphism, then $$d\varphi_p$$ iso"). But the converse holds too, and this is the **inverse function theorem** for manifolds:

> For a smooth map $$F : M \to N$$ and a point $$p$$, the differential $$dF_p$$ is an isomorphism **if and only if** $$F$$ restricts to a diffeomorphism from some neighborhood of $$p$$ onto some neighborhood of $$F(p)$$ — i.e., $$F$$ is a local diffeomorphism at $$p$$.

So "$$d\varphi_p$$ is an isomorphism" and "$$\varphi$$ is a local diffeomorphism at $$p$$" are *equivalent* conditions, not just one implying the other. The forward direction (local diffeo $$\Rightarrow$$ iso) is the easy chain-rule argument above. The reverse direction (iso $$\Rightarrow$$ local diffeo) is the substantive content of the inverse function theorem, and it requires real analysis (it's not just algebra).

One distinction to keep straight: a chart is actually *better* than a mere local diffeomorphism. A local diffeomorphism at $$p$$ only promises good behavior on *some* neighborhood; a chart $$\varphi$$ is a genuine diffeomorphism on *all* of its domain $$U$$ onto $$\varphi(U)$$. Either property is more than enough to force $$d\varphi_p$$ to be an isomorphism — but it's worth knowing a chart gives you the stronger global statement on $$U$$, not just a local one.

---

## 8] $$T_pU \cong T_pM$$ for open $$U$$: why bother proving it?

There's a proposition (Lee Proposition 3.9) saying: if $$p \in U \subseteq M$$ with $$U$$ open, and $$\iota : U \hookrightarrow M$$ is the inclusion map, then

$$
d\iota_p : T_pU \to T_pM
$$

is an isomorphism.

The natural first reaction is: *aren't $$T_pU$$ and $$T_pM$$ the exact same object? Why even state this, let alone prove it?*

That intuition is correct *in spirit* — and indeed Lee explicitly says we'll identify these from now on. But the proposition is doing real work, because formally $$T_pU$$ and $$T_pM$$ are **not the same set**. They become "the same" only because of this proposition.

### 8.1] Why they're formally different

Go back to the definition: a tangent vector at $$p \in M$$ is a **derivation on $$C^\infty(M)$$ at $$p$$**. That is, a linear map

$$
v : C^\infty(M) \to \mathbb{R}
$$

satisfying the Leibniz rule. The crucial point is that the input is a function defined on **all of $$M$$**.

Now what is a tangent vector at $$p \in U$$? It's a derivation on $$C^\infty(U)$$ at $$p$$ — a linear map

$$
w : C^\infty(U) \to \mathbb{R}
$$

whose input is a function defined on **all of $$U$$**, which is a strictly smaller domain.

These act on **different function spaces**. An element of $$T_pU$$ eats functions on $$U$$; an element of $$T_pM$$ eats functions on $$M$$. So as sets, $$T_pU \not\subseteq T_pM$$ and $$T_pM \not\subseteq T_pU$$ — they're not even comparable. They're two different vector spaces sitting in two different ambient categories of linear functionals.

### 8.2] What the proposition actually claims

The proposition says: even though these are formally different, the differential of the inclusion $$\iota : U \hookrightarrow M$$ provides a *canonical* isomorphism between them. Concretely, $$d\iota_p$$ sends $$w \in T_pU$$ to the derivation $$v \in T_pM$$ defined by

$$
v(f) = w(f|_U) \quad \text{for } f \in C^\infty(M).
$$

In other words, "given a tangent vector at $$p$$ that knows how to differentiate functions on $$U$$, you can turn it into a tangent vector at $$p$$ that knows how to differentiate functions on $$M$$, by simply restricting any input function to $$U$$ first."

The proposition is not saying "these spaces are equal." It's saying "this particular map between them is bijective."

### 8.3] Why proving it is non-trivial

The proof relies on a deep fact: **derivations only see the local behavior of functions** (Lee's Proposition 3.8, the lemma right before this one). This means if two functions agree in a tiny neighborhood of $$p$$, every derivation at $$p$$ gives them the same value.

But proving the isomorphism requires more than just locality — it requires that smooth functions on $$U$$ can be **extended** to smooth functions on $$M$$. This is the **extension lemma for smooth functions**, which itself relies on the existence of bump functions, which in turn relies on the manifold being a smooth manifold (paracompact, etc.). The argument breaks roughly into:

**Injectivity of $$d\iota_p$$:** Suppose $$w \in T_pU$$ and $$d\iota_p(w) = 0$$. Take any $$f \in C^\infty(U)$$. Pick a small ball $$B$$ around $$p$$ whose closure is in $$U$$. The extension lemma gives a smooth function $$\tilde f \in C^\infty(M)$$ that equals $$f$$ on $$B$$. Now apply $$w$$ to $$f$$: since $$f$$ and $$\tilde f|_U$$ agree near $$p$$, locality says $$w(f) = w(\tilde f|_U) = w(\tilde f \circ \iota) = d\iota_p(w)(\tilde f) = 0$$. So $$w$$ kills everything $$\Rightarrow w = 0$$.

**Surjectivity of $$d\iota_p$$:** Given $$w \in T_pM$$, you have to *construct* a derivation $$v \in T_pU$$ mapping to it. You define $$v(f) := w(\tilde f)$$ where $$\tilde f$$ is any smooth extension of $$f$$ to $$M$$. You then check this is well-defined (different extensions give the same answer, by locality of $$w$$) and that it really is a derivation.

So the proof is doing two non-trivial things: (1) invoking the existence of smooth extensions (a deep manifold-theoretic fact), and (2) using locality of derivations to show the construction is consistent.

### 8.4] The deeper moral

The proposition is a *bridge between two different formal definitions of the same intuitive object*. The intuition that "a tangent vector at $$p$$ shouldn't care whether $$p$$ is sitting inside $$U$$ or inside $$M$$ — it's the same geometric arrow" is exactly right. The proposition is what *makes* that intuition rigorous. Without it, you'd be stuck saying "well, $$T_pU$$ and $$T_pM$$ are different vector spaces by definition, so how do I compare them?"

After the proposition, Lee remarks that from then on we identify $$T_pU$$ with $$T_pM$$, on the grounds that $$d\iota_p(v)$$ is *the same derivation* as $$v$$, merely regarded as acting on functions defined on the larger manifold rather than on $$U$$.

That remark is doing a lot of work. The phrase "the same derivation" is meaningful only because the proposition has been established.

### 8.5] Why this matters in practice

This identification is what lets you use **coordinate charts** to do calculations. A chart $$(U, \varphi)$$ is defined only on an open subset $$U \subseteq M$$, so strictly speaking it gives you a basis $$\partial/\partial x^i|_p$$ for $$T_pU$$, not $$T_pM$$. But because of this proposition, you can transfer that basis to $$T_pM$$ and use it to compute things globally. Every concrete tangent-vector calculation you'll do depends on this identification working — including everything in §6 and §11 of this post.

It's also what justifies the dimension count. The proof that $$\dim T_pM = n$$ (Lee Proposition 3.10) chains together:

$$
T_pM \;\underset{\text{open submfd}}{\cong}\; T_pU \;\underset{d\varphi_p}{\cong}\; T_{\varphi(p)}\varphi(U) \;\underset{\text{open submfd}}{\cong}\; T_{\varphi(p)}\mathbb{R}^n \;\cong\; \mathbb{R}^n.
$$

The first and third links are the open-submanifold proposition of this section; the middle link is the diffeomorphism fact of §7.6. Drop the first link and the chain breaks.

So: the intuition is right, but the proposition is the formal handshake that lets you cash in on it.

---

## 9] $$T_pM$$ has no canonical isomorphism with $$\mathbb{R}^n$$

We have just built a chain of isomorphisms ending in $$T_pM \cong \mathbb{R}^n$$. It is therefore extremely important to understand in what sense this isomorphism is *not* canonical — because this is one of the central conceptual points of the whole subject.

### 9.1] What "no canonical isomorphism" means

You might think: "$$T_pM$$ is an $$n$$-dimensional real vector space, and $$\mathbb{R}^n$$ is an $$n$$-dimensional real vector space — so aren't they just *the same thing*?"

They are *abstractly* isomorphic — any two $$n$$-dimensional real vector spaces are. But **isomorphisms are not unique**. There are infinitely many different isomorphisms $$T_pM \to \mathbb{R}^n$$, and none of them is *preferred* or *naturally singled out* by the structure of $$M$$ alone.

A **canonical isomorphism** would be one that:

- requires **no arbitrary choices** to define,
- is completely determined by the intrinsic data (the manifold structure, the point $$p$$),
- and would be agreed upon by everyone, everywhere, without coordination.

### 9.2] The concrete obstruction: coordinate charts

The natural way to build an isomorphism $$T_pM \to \mathbb{R}^n$$ is to pick a smooth coordinate chart $$(U, \varphi)$$ around $$p$$. Then the differential

$$
d\varphi_p : T_pM \to T_{\varphi(p)}\mathbb{R}^n \cong \mathbb{R}^n
$$

is an isomorphism. It sends the coordinate basis vectors $$\partial/\partial x^i|_p$$ to the standard basis vectors of $$\mathbb{R}^n$$.

But **this depends on your choice of chart**. If you pick a different chart $$(U, \psi)$$, you get a *different* isomorphism — the two are related by the Jacobian of the transition map $$\psi \circ \varphi^{-1}$$. There's no intrinsic reason to prefer one chart over another.

So $$T_pM \cong \mathbb{R}^n$$, yes — but only *after you make an arbitrary choice*.

### 9.3] Contrast: when a canonical isomorphism *does* exist

This sharpens the intuition by contrast. When $$M$$ is itself a **vector space** $$V$$ (or an open subset of one), there *is* a canonical isomorphism $$T_aV \cong V$$, for any $$a \in V$$. It's the map

$$
v \mapsto D_v\big|_a, \quad \text{where } D_v\big|_a f = \frac{d}{dt}\bigg|_{t=0} f(a + tv).
$$

This doesn't require picking any basis or chart — you use the linear structure of $$V$$ itself to "translate" vectors to the point $$a$$. It works the same way for everyone.

Similarly, for a **product manifold** $$M \times N$$, there's a canonical isomorphism $$T_{(p,q)}(M \times N) \cong T_pM \oplus T_qN$$, defined purely by the projections — no choices needed. (This is Lee Proposition 3.14.)

This contrast is important enough that it gets its own detailed section below (§10).

### 9.4] The deeper point

The phrase "no canonical isomorphism" is really a statement about **what structure the manifold has**.

- A general smooth manifold has only a *smooth structure* (overlapping charts, transition maps). That's not enough to single out a preferred basis for $$T_pM$$.
- A vector space has *linear structure*, which is exactly the extra ingredient needed to build the canonical identification.

This is also why the tangent–cotangent isomorphism $$T_pM \cong T_p^*M$$ (identifying tangent and cotangent vectors) is **not canonical** either — it requires a choice of Riemannian metric. Without a metric, there's no preferred way to "lower an index." We will return to this at length when we discuss the cotangent space and, later, musical isomorphisms.

**Short summary:** $$T_pM$$ is always isomorphic to $$\mathbb{R}^n$$, but the isomorphism requires picking a coordinate chart — an arbitrary choice. "No canonical isomorphism" means there's no isomorphism that the manifold structure itself selects for you.

---

## 10] The vector-space contrast in detail: where does the linear structure come in?

The whole story of §9.3 turns on one tiny piece of notation: **"$$a + tv$$."** It's worth slowing down on this.

### 10.1] The construction

For a finite-dimensional vector space $$V$$ and a point $$a \in V$$, we define a map

$$
\Phi : V \to T_a V, \qquad v \mapsto D_v|_a
$$

where the derivation $$D_v|_a$$ acts on a smooth function $$f : V \to \mathbb{R}$$ by

$$
D_v|_a f = \frac{d}{dt}\bigg|_{t=0} f(a + tv).
$$

So given the vector $$v$$, the corresponding tangent vector at $$a$$ is "the derivative of $$f$$ along the straight line through $$a$$ in the direction $$v$$." Now look very carefully at the right-hand side. The whole construction hinges on the expression **$$a + tv$$** living inside $$V$$. Let's see what's needed to even *write that down*.

### 10.2] Where the linear structure enters

To form $$a + tv$$, you need three operations:

**1. Scalar multiplication.** Given a real number $$t$$ and a vector $$v \in V$$, you need to form $$tv \in V$$. This requires $$V$$ to have a scalar-multiplication operation.

**2. Vector addition.** You then need to add the point $$a$$ and the vector $$tv$$ to get another point $$a + tv \in V$$. This requires $$V$$ to have an addition operation.

**3. The fact that points and vectors live in the same space.** This is the subtle one. On a generic manifold $$M$$, a point $$p$$ and a "tangent vector at $$p$$" are very different kinds of objects — you cannot add them. But on a vector space $$V$$, every element is *simultaneously* a point and a vector, because $$V$$ is its own tangent space (in a sense we're about to make precise). So the expression "$$a + tv$$" makes sense: $$a$$ is being treated as a point, $$v$$ as a displacement, and their sum is a new point.

The curve $$\gamma(t) = a + tv$$ is a smooth curve in $$V$$ passing through $$a$$ at $$t = 0$$, and the derivation $$D_v|_a$$ is precisely the velocity of this curve at $$t = 0$$, acting on $$f$$.

### 10.3] Why this is canonical (no choices made)

Now compare this construction to the one for a generic manifold:

**On a generic manifold $$M$$:** To get an isomorphism $$T_pM \cong \mathbb{R}^n$$, you must pick a chart $$(U, \varphi)$$. Different charts give different isomorphisms. There's no preferred chart, so there's no preferred isomorphism.

**On a vector space $$V$$:** To define $$\Phi(v) = D_v|_a$$, look at what choices we made. We picked... nothing. We just used:

- the point $$a$$ (which was given),
- the vector $$v$$ (which was given),
- the addition and scalar multiplication built into $$V$$ itself.

We did **not** pick a basis. We did **not** pick a chart. We did **not** pick coordinates. The construction uses only the linear-space structure that $$V$$ was born with.

That's exactly what "canonical" means: every step of the construction is forced by the data already present.

### 10.4] A subtle point in Lee's proof

Lee's proof of Proposition 3.13 says, in effect, that once we choose a basis for $$V$$, the argument from Proposition 3.2 shows both that $$D_v|_a$$ is a derivation at $$a$$ and that $$v \mapsto D_v|_a$$ is an isomorphism.

This sentence often confuses people — *didn't he just say a basis is required?* The answer is: a basis is used to **prove** that $$\Phi$$ is an isomorphism (because the proof technique borrows from the $$\mathbb{R}^n$$ case, which needed coordinates). But the **definition** of $$\Phi$$ itself, given by the formula above, used no basis at all. Different bases would give different proofs of the same fact, but they're all proving that the *same* map $$\Phi$$ is an isomorphism. The map exists and is well-defined before any basis is mentioned. Lee himself flags this explicitly: the isomorphism is canonically defined, notwithstanding the fact that a basis was used to prove it is one.

This is a recurring distinction worth internalizing: *using* a basis to prove something is fine; *needing* a basis to even define something is what makes a construction non-canonical.

### 10.5] What this buys you in practice

Because $$V \cong T_aV$$ canonically, you can stop distinguishing between "the vector $$v \in V$$" and "the tangent vector $$D_v|_a \in T_aV$$." Everyone in the field uses the same identification, so no information is lost by conflating them.

This is why, for example, when you do calculus on $$\mathbb{R}^n$$ in a standard course, nobody worries about the difference between "the vector $$(1, 2, 3)$$ in $$\mathbb{R}^3$$" and "the tangent vector $$\langle 1, 2, 3 \rangle$$ at the point $$p$$." They're connected by a canonical isomorphism, so it's harmless to treat them as the same.

Combining this with §8, if $$M$$ is an *open submanifold* of a vector space $$V$$, we can chain the identifications $$T_pM \leftrightarrow T_pV \leftrightarrow V$$ to canonically identify every tangent space of $$M$$ with $$V$$ itself. Lee's favorite example: since $$GL(n,\mathbb{R})$$ is an open submanifold of the vector space $$M(n,\mathbb{R})$$, each of its tangent spaces is canonically the full space of matrices $$M(n,\mathbb{R})$$.

### 10.6] Where it breaks

On a general manifold like the sphere $$S^2$$, this identification breaks down completely. There's no way to "add" two points on $$S^2$$ (what would the North Pole plus Tokyo even mean?). The whole construction "$$a + tv$$" fails at step 2 above. So you genuinely need the more abstract derivation-based definition of tangent vectors — and the resulting tangent space is not canonically anything except itself.

---

## 11] Equivalence between derivations and curves

Perhaps not so surprisingly, the definitions in §2.1 and §2.2 are compatible and even equivalent to one another. 

**From curves to derivations:**

[TODO: given $$\gamma$$ with $$\gamma(0) = p$$, define $$v_\gamma(f) := \frac{d}{dt}\big|_{t=0}(f \circ \gamma)$$; check linearity and Leibniz; check that $$\gamma_1 \sim \gamma_2 \Rightarrow v_{\gamma_1} = v_{\gamma_2}$$ so the map is well defined on equivalence classes. Note that §12.1 below already carries out the coordinate computation in full, so this section can largely cite it.]

**From derivations to curves:**

[TODO: given a derivation $$v = \sum_i v^i \partial/\partial x^i|_p$$, produce the curve $$\gamma(t) = \varphi^{-1}(\varphi(p) + t(v^1,\dots,v^n))$$ for small $$t$$ and check $$v_\gamma = v$$; then check the two constructions are mutually inverse.]

---

## 12] A concrete computational example

A coherent walkthrough using the sphere $$ S^2 $$.

Let's re-emphasize again that a vector field on a Manifold is defined as the following:

Let $$ M $$ be a smooth manifold. A **vector field** on $$ M $$ is a smooth section/assignment

$$
p \mapsto X_p \in T_p M
$$

where $$ T_p M $$ is the tangent space at $$ p $$.

**Fundamental viewpoint**:
> A tangent vector is a **derivation**: a linear map
> $$
> X_p : C^\infty(M) \to \mathbb{R}
> $$
> satisfying the Leibniz rule.

We'll also give the local coordinates and coordinate vector fields as the following:

Let

$$
(U, \varphi), \quad \varphi : U \subset M \to V \subset \mathbb{R}^n
$$

be a coordinate chart, with coordinates

$$
(x^1, \dots, x^n).
$$

Each coordinate $$ x^i $$ is itself a **function on the manifold**:

$$
x^i : U \to \mathbb{R}.
$$

**Definition of the "Coordinate Vector Field":**

The coordinate vector field

$$
\left.\frac{\partial}{\partial x^i}\right|_p
$$

is defined **intrinsically** by its action on smooth functions:

$$
\boxed{
\left.\frac{\partial}{\partial x^i}\right|_p(f) :=
\frac{\partial}{\partial x^i}
\big(f \circ \varphi^{-1}\big)
\Big(\varphi(p)\Big)
}
$$

where $$ x^i $$ is the $$ i $$-th coordinate on $$ \mathbb{R}^n $$.

Notice that there's an abuse of notation (the left "partial" is what we define, whereas the right partial is the usual partial differentiation). Hopefully, it's obvious that the above definition stems from the following basic definition of graph differential (which I will talk more into next time):

$$
\boxed{
(dF_p(v))(f) := v_p(f \circ F)
}
$$

More importantly, this definition:

- uses **only** the chart, 
- does **not** require an embedding and thus does $$NOT$$ give us components in $$\mathbb{R}^n$$
- explains what "$$ \partial f / \partial x^i $$" actually means: It means the ordinary partial derivative of the coordinate expression of $$f$$ after pulling $$f$$ back to a coordinate chart. Note that nothing is being differentiated on $$\mathbb{R}^n$$ unless we explicitly choose an embedding (see below).

---

Now, let's take a look at the simple example: Sphere $$ S^2 $$ and spherical coordinates. Given the manifold

$$
S^2 = \{(x,y,z) \in \mathbb{R}^3 : x^2 + y^2 + z^2 = 1\}
$$

Coordinate chart (away from poles)

$$
(\theta, \varphi)
$$

with embedding map

$$
F(\theta,\varphi) =
(\sin\varphi\cos\theta,\;
 \sin\varphi\sin\theta,\;
 \cos\varphi).
$$

Here:

- $$ \theta, \varphi $$ are **functions on $$ S^2 $$**,
- not abstract variables.

According to the above discussions, the **intrinsic** meaning of $$ \partial / \partial \theta $$ and $$ \partial / \partial \varphi $$ are the following:

For any $$ f : S^2 \to \mathbb{R} $$,

$$
\frac{\partial}{\partial \theta}(f) =
\frac{\partial}{\partial \theta}
\big(f \circ \varphi^{-1}\big)(\theta,\varphi),
\quad
\frac{\partial}{\partial \varphi}(f) =
\frac{\partial}{\partial \varphi}
\big(f \circ \varphi^{-1}\big).
$$

Again, this is the **definition**, not an interpretation. This offers us one way to calculate the vector field/tangent vectors at $$\forall p \in M$$.

If we set 

$$f(x, y, z) = z$$

then we could pull it back and obtain

$$(f \circ \phi^{-1})(\theta, \phi) = \cos(\phi).$$

Consequently, 

> Example 1: $$ X = \partial/\partial\theta $$

$$
X_p(f)= \frac{\partial \cos(\phi)}{\partial \theta} = 0
$$

at $$p = (\theta, \phi)$$.

---

> Example 2: $$ Y = \partial/\partial\varphi $$

$$
Y_p(f)= \frac{\partial \cos(\phi)}{\partial \phi} = -\sin(\phi)
$$

at $$p = (\theta, \phi)$$.

---

However, because $$ S^2 \subset \mathbb{R}^3 $$, we may compute concrete representatives by viewing it as embedded in $$\mathbb{R}^3$$ and to extrinsically compute the vector fields by pushforward of coordinate basis vectors via the embedding map of the manifold. Consequently, such computation lives in the embedded picture, not the intrinsic definition.

$$
\frac{\partial}{\partial \theta} =
\frac{\partial F}{\partial \theta} =
(-\sin\varphi\sin\theta,\;
  \sin\varphi\cos\theta,\;
  0)
$$

$$
\frac{\partial}{\partial \varphi} =
\frac{\partial F}{\partial \varphi}=
(\cos\varphi\cos\theta,\;
 \cos\varphi\sin\theta,\;
 -\sin\varphi)
$$

These are **actual vectors in $$ \mathbb{R}^3 $$** tangent to $$ S^2 $$.

> Important:  
> This is **not the definition** of coordinate vector fields —  
> it is a **representation** using the embedding.

Consequently, we could act on functions without pulling back to coordinate space:

Let

$$
f(p) = z(p)
$$

be a function on $$ S^2 $$.

Choose an extension

$$
\tilde f(x,y,z) = z
\quad\Rightarrow\quad
\nabla \tilde f = (0,0,1).
$$

Here's the key observation: 

> Once a tangent vector is represented in $$ \mathbb{R}^3 $$, its action on $$f$$ is given by the directional derivative of an extension of $$f$$.

Formally, 

$$
\boxed{
X_p(f) = \nabla \tilde f(p) \cdot X_p
}
$$ 

where 

* $$\tilde{f}$$ is any smooth extension of $$f$$ to $$\mathbb{R}^3$$
* $$X_p \in T_pS^2 \subset \mathbb{R}^3$$.

This works because tangent vectors annihilate normal components.

---

> Example 1: $$ X = \partial/\partial\theta $$

$$
X(f)=
(0,0,1)
\cdot
(-\sin\varphi\sin\theta,\;
  \sin\varphi\cos\theta,\;
  0)
= 0
$$

---

> Example 2: $$ Y = \partial/\partial\varphi $$

$$
Y(f)=
(0,0,1)
\cdot
(\cos\varphi\cos\theta,\;
 \cos\varphi\sin\theta,\;
 -\sin\varphi)
= -\sin\varphi
$$

Same results as the intrinsic definition.

Note that to be very concrete, in the above examples, at $$p = (\theta, \phi)$$, the tangent vector is:

$$X_p = (-\sin\varphi\sin\theta,\; \sin\varphi\cos\theta,\;0)$$ 

(we could interpret this as horizontal circles of latitude) and 

$$Y_p = (\cos\varphi\cos\theta,\; \cos\varphi\sin\theta,\ -\sin\varphi).$$ 

This should make it very clear that we are treating the tangent vectors as actually "vectors" living in $$\mathbb{R}^3$$ like how we usually refer them to be. With this extrinsic embedding, the general form of a vector field coincides with the intrinsic notion, but more tangible:

$$X = a(\theta, \phi)\frac{\partial}{\partial \theta} + b(\theta, \phi)\frac{\partial}{\partial \phi},$$ 

then as a vector in $$\mathbb{R}^3$$: $$X_p = a \frac{\partial F}{\partial \theta} + b\frac{\partial F}{\partial \phi}$$, and 

$$X_p(f) = \nabla \tilde{f}(p) \cdot X_p.$$

Notice that for this computation no coordinate pullback is required. But to be crystal clear, we do __not__ compute the vector field "without coordinates". We simply replaced intrinsic coordinates on $$\mathbb{S}^2$$ by ambient Cartesian coordinates in $$\mathbb{R}^3$$. So coordinates are still there — just in a different space.

---

Before we continue, let's dwell on this formula for a while:

$$
\boxed{
X_p(f) = \nabla \tilde f(p) \cdot X_p
}
$$

This identity guarantees consistency between 1] the derivation definition and 2] the embedded vector representation (Question: is this just chain rule?). It explains why the extrinsic calculation agrees with the intrinsic definition. If we have a nice embedding, two views are nice. However, if we go to the general abstract case without the ambient space, we could only assort to the intrinsic definition — which is exactly the situation §5 described.

---

To be rigorous, you may be concerned about the specific use of the function extension here. However, we could show that extensions do not matter, because __"tangent vectors annihilate normal components"__. Let me explain below:

If $$ g : \mathbb{R}^3 \to \mathbb{R} $$ satisfies

$$
g|_{S^2} = 0,
$$

then for all $$ v \in T_p S^2 $$,

$$
v(g) = 0.
$$

Equivalently, 

$$
\nabla g(p) \perp T_p\mathbb{S}^2
$$

### Proof (via curves):

Let $$ \gamma(t) \subset S^2 $$ with

$$
\gamma(0)=p, \quad \gamma'(0)=v.
$$

Since $$ g(\gamma(t)) = 0 $$ for all $$ t $$,

$$
v(g) = \frac{d}{dt} g(\gamma(t))\big|_{t=0} = 0.
$$

### Concrete example

Let

$$
g(x,y,z) = x^2 + y^2 + z^2 - 1.
$$

Then:

$$
\nabla g = (2x,2y,2z)
$$

which is normal to $$ S^2 $$.

For any tangent vector $$ v $$,

$$
v(g) = \nabla g \cdot v = 0.
$$

### Consequence (crucial)

If $$ \tilde f_1 $$ and $$ \tilde f_2 $$ are two extensions of $$ f $$, then $$g = \tilde{f_1} - \tilde{f_2} $$ vanishes on $$\mathbb{S}^2$$. For any tangent vector $$v$$:

$$
v(\tilde f_1) - v(\tilde f_2) = v(g) = 0
$$

thus: 

$$
v(\tilde f_1) = v(\tilde f_2)
\quad\forall v \in T_p S^2.
$$

Hence directional derivatives along tangent vectors are **well-defined**: this is why directional derivatives along tangent vectors do not depend on how $$f$$ extends off the manifold.

Viewed from another perspective, this indicates that a function that is identically zero on the manifold cannot change when we move tangentially: so tangent vectors "don't see" normal variations. What I mean by the former statement "Tangent vectors annihilate normal components" is that derivatives along tangent directions ignore any part of a function that only varies off the manifold, because those variations are invisible to curves that stay on the manifold.

> A remark connecting back to §4.6: this is precisely Lee's Proposition 5.37 in disguise. There, the tangent space to an embedded submanifold $$S \subseteq M$$ is characterized as
> $$
> T_pS = \{v \in T_pM : vf = 0 \text{ whenever } f \in C^\infty(M) \text{ and } f|_S = 0\}.
> $$
> Note that this is a *linear* condition on $$v$$ — which is exactly why $$T_pS$$ comes out as a genuine subspace of $$T_pM$$, per the machinery of §3.5.

---

### Intrinsic vs Extrinsic Viewpoints (Summary)

| Aspect | Intrinsic | Extrinsic |
|------|----------|-----------|
| Tangent vector | Derivation | Vector in $$ \mathbb{R}^3 $$ |
| Definition | Via charts | Via embedding |
| Computation | Pullback | Directional derivative |
| Dependence | Coordinate-dependent | Embedding-dependent |

Both viewpoints are **equivalent** when an embedding exists.

---

Something to note from the above example:

> Coordinate vector fields are **defined intrinsically as derivations**,  
> may be **represented extrinsically** using embeddings,  
> and act on functions in a way that is **independent of extensions** because tangent vectors annihilate normal components.

Through this simple example we already develop a taste of the difference between intrinsic and extrinsic geometry, the abstract definitions and concrete calculations. Such theme will be recurring for our journey in studying differential geometry. 

One of the advantages of the intrinsic view is that it is independent of coordinate charts. It paves the ground for explaining why 

> __Gradients, geodesics, and Lie brackets make sense without embedding (or ever mentioning) the abstract manifold into $$\mathbb{R}^n$$__.

---

## 13] Discussions

Firstly, note that tangent/cotangent space is an intrinsic property. A usual image of interpreting tangent space is $$S^2$$ embedded in $$\mathbb{R}^3$$. Our definition above does not require so, and in fact the shift of perspective from extrinsic into intrinsic properties of geometric objects is a grand evolution starting from Gauss and Riemann. 

[TODO: An illustrative figure of a sphere embedded in Euclidean space]

### 13.1] Abuse of notation

Many textbooks will carry abuse of notation throughout their content, most often conflating functions/derivations defined on the manifold with them composed with local coordinates. I find it really confusing and hard to interpret many times, so I decide to, once again, present one simple example where no abuse of notation whatsoever happens, so we could get it crystal clear. Let's begin. 

---

#### 13.1.1] Clear Statement

Let:

- $$ M $$ be a smooth manifold.
- $$ c : I \subset \mathbb{R} \to M $$ be a smooth curve.
- $$ (U,\varphi) $$ be a coordinate chart with  
  $$ \varphi : U \to \mathbb{R}^n $$
- Assume $$ c(t_0) \in U $$.

Then the tangent vector $$ c'(t_0) \in T_{c(t_0)}M $$ satisfies:

$$
c'(t_0)
= \sum_{i=1}^n
\left.
\frac{d}{dt}(x^i \circ c)(t)
\right|_{t_0}
\;
\left.
\frac{\partial}{\partial x^i}
\right|_{c(t_0)}
$$

where all objects are defined precisely below.

---

#### 13.1.2] Definition of the Tangent Vector to a Curve

Fix $$ t_0 \in I $$.

The tangent vector $$ c'(t_0) \in T_{c(t_0)}M $$ is defined as the derivation:

$$
c'(t_0)(f)
:=
\left.
\frac{d}{dt}(f \circ c)(t)
\right|_{t=t_0}
$$

for every smooth function $$ f \in C^\infty(M) $$. Notice that this is the **definition** of the velocity vector, and there are no coordinates used here.

---

#### 13.1.3] Introduce a Coordinate Chart

Let $$ (U,\varphi) $$ be a coordinate chart with:

- $$ c(t_0) \in U $$
- $$ \varphi: U \to \mathbb{R}^n $$

Write:

$$
\varphi(p) = (x^1(p),\dots,x^n(p))
$$

where

$$
x^i := \pi^i \circ \varphi
$$

and $$ \pi^i : \mathbb{R}^n \to \mathbb{R} $$ is the standard projection.

Thus each coordinate function

$$
x^i : U \to \mathbb{R}
$$

is smooth.

---

#### 13.1.4] Coordinate Basis of the Tangent Space

For each $$ p \in U $$, define the coordinate vector:

$$
\left.\frac{\partial}{\partial x^i}\right|_p
$$

as the derivation:

$$
\left.\frac{\partial}{\partial x^i}\right|_p (f)
:=
\left.
\frac{\partial}{\partial u^i}
\Big(
f \circ \varphi^{-1}
\Big)
(u)
\right|_{u=\varphi(p)}
$$

for all smooth functions $$ f $$ defined near $$ p $$.

This definition proceeds in three steps:

1. Pull $$ f $$ back to $$ \mathbb{R}^n $$ via $$ \varphi^{-1} $$
2. Take the ordinary partial derivative
3. Evaluate at the coordinate point $$ \varphi(p) $$

These vectors form a basis of $$ T_pM $$, as we know from above.

> Note the two identifications quietly at work in the phrase "for all smooth functions $$f$$ defined near $$p$$": we are applying a derivation to a function that is *not* defined on all of $$M$$. That this is legitimate is exactly the content of §8 — $$T_pU \cong T_pM$$ canonically, so any tangent vector at $$p$$ can be unambiguously applied to functions defined only in a neighborhood of $$p$$.

---

#### 13.1.5] Express the Curve in Coordinates

Define the coordinate representation of the curve:

$$
\gamma := \varphi \circ c
$$

Thus:

$$
\gamma : I \to \mathbb{R}^n
$$

Write:

$$
\gamma(t)
= (\gamma^1(t),\dots,\gamma^n(t))
$$

where

$$
\gamma^i(t)
= \pi^i(\gamma(t))
= \pi^i(\varphi(c(t)))
= (x^i \circ c)(t)
$$

So:

$$
\gamma^i = x^i \circ c
$$

These are ordinary real-valued functions.

---

#### 13.1.6] Compute the Action of $$ c'(t_0) $$

Let $$ f \in C^\infty(M) $$.

By definition:

$$
c'(t_0)(f)
= \left.
\frac{d}{dt}(f \circ c)(t)
\right|_{t_0}
$$

Insert identity:

$$
f \circ c
= (f \circ \varphi^{-1}) \circ (\varphi \circ c)
$$

Define:

$$
F := f \circ \varphi^{-1}
$$

Then:

$$
c'(t_0)(f)
= \left.
\frac{d}{dt}
F(\gamma(t))
\right|_{t_0}
$$

---

#### 13.1.7] Apply the Multivariable Chain Rule in $$ \mathbb{R}^n $$

Since everything is now in $$ \mathbb{R}^n $$, apply the usual chain rule:

$$
\frac{d}{dt}F(\gamma(t))
= \sum_{i=1}^n
\frac{\partial F}{\partial u^i}(\gamma(t))
\,
\frac{d\gamma^i}{dt}(t)
$$

Evaluating at $$ t_0 $$ gives:

$$
c'(t_0)(f)
= \sum_{i=1}^n
\frac{d\gamma^i}{dt}(t_0)
\,
\frac{\partial F}{\partial u^i}
(\gamma(t_0))
$$

---

#### 13.1.8] Return to Manifold Language

Recall:

- $$ \gamma(t_0)=\varphi(c(t_0)) $$
- $$ F = f \circ \varphi^{-1} $$

Thus:

$$
\frac{\partial F}{\partial u^i}(\gamma(t_0))
= \left.
\frac{\partial}{\partial u^i}
(f \circ \varphi^{-1})
\right|_{u=\varphi(c(t_0))}
$$

By definition of the coordinate vector fields:

$$
= \left.
\frac{\partial}{\partial x^i}
\right|_{c(t_0)}(f)
$$

Therefore:

$$
c'(t_0)(f)
= \sum_{i=1}^n
\frac{d\gamma^i}{dt}(t_0)
\,
\left.
\frac{\partial}{\partial x^i}
\right|_{c(t_0)}(f)
$$

Since this equality holds for every smooth function $$ f $$, we conclude:

$$
c'(t_0)
= \sum_{i=1}^n
\frac{d\gamma^i}{dt}(t_0)
\,
\left.
\frac{\partial}{\partial x^i}
\right|_{c(t_0)}
$$

Finally substitute:

$$
\gamma^i = x^i \circ c
$$

so:

$$
\frac{d\gamma^i}{dt}(t_0)
= \frac{d}{dt}(x^i \circ c)(t_0)
$$

---

#### 13.1.9] Final Fully Precise Formula and Summary

$$
\boxed{
c'(t_0)
= \sum_{i=1}^n
\left.
\frac{d}{dt}(x^i \circ c)(t)
\right|_{t_0}
\;
\left.
\frac{\partial}{\partial x^i}
\right|_{c(t_0)}
}
$$

In summary, the above elaboration consists of:

1. Pulling everything to $$ \mathbb{R}^n $$
2. Applying the ordinary multivariable chain rule
3. Translating back via the definition of coordinate vector fields

Thus, the coordinate representation of the velocity vector is precisely the multivariable chain rule expressed in intrinsic manifold language.

---

### 13.2] Orthonormal basis?

Another subtle point that haunted me for while is the question of whether the basis $$\left\{\frac{\partial}{\partial x_i}\Big|_p\right\}_{i=1,2,...,n}$$ are orthonormal or not.

The short answer is No. The coordinate basis is **not necessarily orthonormal**. It's true that these vectors:

- Form a basis of $$T_pM$$.
- Depend on the coordinate choice.
- Are defined geometrically as directional derivatives.

However, nothing in their definition guarantees that they are orthogonal or normalized. In fact, even more basic, orthonormality requires a __metric__! To even ask whether a basis is orthonormal, we must have a **Riemannian metric** $$g$$ (will talk about this in details later; readers not familiar with metric/Riemannian metric feel free to please come back later; I include this short discussion for completeness and I apologize for grabbing future yet-to-be-learned knowledge as part of this section's story). Without a metric, we cannot measure angles and lengths, and thus "orthonormal" is meaningless.

> This is the same phenomenon as §9.4: a bare smooth manifold simply does not carry enough structure to single out preferred directions, preferred bases, or preferred isomorphisms. Orthonormality, like canonicity of $$T_pM \cong T_p^*M$$, is extra data you must supply.

But even given a Riemannian metric $$g$$, usually orthonormality is not guaranteed. We can easily compute

$$
g_{ij}(p)
= g\!\left(\frac{\partial}{\partial x_i}, \frac{\partial}{\partial x_j}\right)_p.
$$

In general,

$$
g_{ij}(p) \neq \delta_{ij}.
$$

So the coordinate basis is usually **not orthonormal**.

Let's do a simple example: the sphere. Consider the 2-sphere $$S^2$$ with spherical coordinates $$(\theta, \phi)$$.

Given a metric:

$$
ds^2 = d\theta^2 + \sin^2\theta \, d\phi^2.
$$

Then:

$$
g\left(\frac{\partial}{\partial \theta}, \frac{\partial}{\partial \theta}\right) = 1,
$$

$$
g\left(\frac{\partial}{\partial \phi}, \frac{\partial}{\partial \phi}\right) = \sin^2\theta.
$$

These coordinate vectors are indeed:

- Orthogonal.
- But **not normalized** unless $$\sin\theta = 1$$.

Even in nice coordinates, the basis is typically not orthonormal.

So then when are they orthonormal? As said above, they are orthonormal only if

$$
g_{ij}(p) = \delta_{ij}.
$$

This happens in special cases, like in 

* **Case 1: Euclidean Space.** In $$\mathbb{R}^n$$ with the standard metric,

$$
g_{ij} = \delta_{ij},
$$

so the coordinate basis is orthonormal everywhere.

* **Case 2: Riemannian Normal Coordinates.** On any Riemannian manifold, we can construct **normal coordinates** around $$p$$ such that

$$
g_{ij}(p) = \delta_{ij}.
$$

However, this holds only at the single point $$p$$. Away from $$p$$, the metric is no longer Euclidean. On a side note, this reflects a deep geometric fact:

> Every Riemannian manifold looks Euclidean to first order at a point.

To do a short summary and comparison:

Coordinate basis vectors:

- Depend on how we label the manifold.
- Reflect the coordinate grid.
- May stretch or skew according to the geometry.

An orthonormal basis instead:

- Is chosen using the __metric__.
- Is independent of coordinates.
- Can be obtained via Gram–Schmidt if needed.

---

## 14] Appendix: a dependency map of this post

It may help to see how the pieces fit together, since the logical order is not the order of intuition.

```
C^∞(M) is a vector space                        (§1.1)
        │
        ▼
(C^∞(M))* = linear functionals on it            (§1.2)
        │
        ▼
Leibniz is a LINEAR condition on functionals    (§3.4, §3.5)
        │
        ▼
⟹ T_pM is a SUBSPACE of (C^∞(M))*              (§1.3, §3.6)
        │
        ▼
Why Leibniz? ← product rule for D_v|_a          (§4.2)
             ← linear + Leibniz ⟺ directional
               derivative (Lee 3.2 / Tu 2.2)     (§4.3)
             ← survives loss of a+tv on manifolds(§4.4, §5)
        │
        ▼
Charts: homeomorphism + smooth compatibility
        ⟹ chart is a diffeomorphism (Tu 6.10)   (§7.3)
        ⟹ dφ_p is an isomorphism                (§7.6)
        │
        ▼
Open submanifold: T_pU ≅ T_pM (Lee 3.9)         (§8)
        │
        ▼
dim T_pM = n, coordinate basis ∂/∂x^i|_p        (§6, Lee 3.10)
        │
        ▼
T_pM ≅ ℝⁿ — but NOT canonically                 (§9)
   contrast: T_aV ≅ V IS canonical               (§10)
```

Every arrow in that diagram is a place where a "seemingly obvious" identification actually required a theorem. That, more than any individual computation, is the real content of this chapter.

---

## References

- J. M. Lee, *Introduction to Smooth Manifolds*, 2nd ed. — Ch. 3 (Tangent Vectors): Prop. 3.2, 3.6, 3.8, 3.9, 3.10, 3.13, 3.14; Ch. 5 (Submanifolds): Prop. 5.37.
- L. W. Tu, *An Introduction to Manifolds*, 2nd ed. — Thm. 2.2; Prop. 6.10, 6.11; Cor. 8.6.
- J. M. Lee, *Introduction to Topological Manifolds* — for the topological substrate (§5.2).