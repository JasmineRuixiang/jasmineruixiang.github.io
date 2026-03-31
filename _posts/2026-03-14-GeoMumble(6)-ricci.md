---
layout: post
title: 'The Dance of Space: Geom/Topo/Dynam Mumble(6): Ricci Flow and Ricci Curvature (in progress)' 
date: 2026-03-14 23:57:22
description: Introduction of Ricci flows and Ricci curvatures 
series: The Dance of Space
tags:
    - "Geometry Concepts/Tools"
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

## 0] Introduction
Today I will cover a beautiful subject in differenetial geometry: Ricci flows and Ricci curvature. 

I highly recommend readers this [intro paper](https://differentialgeometri.wordpress.com/2022/01/13/an-illustrated-introduction-to-the-ricci-flow-third-version/) written by Gabriel Kahn. 

Generally, in Riemannian geometry, curvature measures how space bends. For instance, on a sphere, geodesics (shortest paths) come closer together compared to flat space; on a hyperbolic surface, they diverge.

Ricci curvature is a particular way of summarizing curvature: Instead of describing how all directions bend (that’s what the full __Riemann curvature tensor__ does), Ricci curvature focuses on __volume distortion__. More concretely, it tells us how the volume of a small geodesic ball deviates from the volume we'd expect in flat Euclidean space.

Intuitively, we could summarize into the following: 
> Positive Ricci curvature (like on a sphere) means geodesics tend to converge, and small balls have less volume than in flat space.
>
>Zero Ricci curvature (like in Euclidean space) means geodesics neither converge nor diverge, so volumes match Euclidean.
>
>Negative Ricci curvature (like on a hyperbolic space) means geodesics diverge, so small balls have more volume than Euclidean.

Mathematically, Ricci curvature is obtained by “tracing” the __Riemann curvature tensor__. It compresses information about how different directions curve into a symmetric 2-tensor `Ric`. 

In order to provide somewht more historical context and motivation, please indulge me in belabouring a little on the story of Poincaré conjecture, because for many Ricci flow is famous as a central tool in proving Poincaré conjecture. But even before that, let's start with a similar (many consider a precursor) theorem in 2D.

### 0.1] Classification Theorem of Surfaces

> Every closed surface is topologically equivalent to
either a sphere with $$g$$ handles (genus $$g$$) or a connected sum of projective planes.

> Every compact simply connected surface is homeomorphic to the sphere.

> any closed orientable surface of genus $$1$$ is topologically the same as a Torus.

This is a classification theorem about the topology, not the applied geometry (for geometry, please see the following two sections). 

---

### 0.2] The Uniformization Theorem
Classifies the geometries of two dimensional surfaces. As Poincaré argued:

> Any __two__ dimensional __closed__ surfaces could be defomred into one of the three following kinds of geometry with __constant curvature__, depending on the number of holes they have:
> 1) Round sphere (curvature 1) if no holes;
> 2) Flat space (curvature 0) if one hole (for example, a donut);
> 3) Hyperbolic plane (curvature -1) if more than one hole.

or 

> Every simply connected Riemann surface is conformally equivalent to exactly one of three spaces:
> 1) the 2-sphere;
> 2) the Euclidean plane;
> 3) the Hyperbolic plane.

or 

> For each __conformal class__ of metrics on a __closed__ surface, there is a special metric with _constant curvature_.

or 

> For any Riemannian metric on a __closed__ surface, there exists a __conformally equivalent__ metric with _constant curvature_, and it is __unique__ within that conformal class.

Consequently, all 2-dimensional geometries collapse into three universal models. This works because curvature in dimension 2 is very simple: the entire curvature tensor is determined by one scalar, the __Gaussian curvature__.

Note that the above is a gentler version. The actual theorem has a stronger claim (conformal transformation, and could also classify non-orientable surfaces). 

As Gabriel Kahn put in his blog, this might seem very bizarre at first glance. I was once so confused here: If the surface has no hole, should not it be a plane? If it has one hole/donut, how could it be made into a flat space?

Well, notice that the classification is towards geometry, not the underlying topology. The above confusion comes from mixing two different things:

* 1) Topology of the surface (how many holes there are / its genus)
* 2) Geometry placed on that surface (the metric and curvature)

The Uniformization Theorem is about the geometry we can put on a given topological surface, not about changing the topology into another object like a plane. What this theorem actually says is that 

> For any connected 2-dimensional surface, we can choose a metric so that its Gaussian curvature is constant everywhere. And here only the above three possibilities exist. 

If the genus is 0, then the constant curvature is positive, which corresponds to a sphere model; if genus 1, then the constant curvature is 0, which corresponds to a flat plane; if genus larger than 1, negative curvature and thus hyperbolic space. 

So the theorem says every surface admits a metric of one of these three constant curvatures. Importantly, the topology does not change but only the metric changes.

Now come back to the original confusion: Why does the sphere corresponds to “no holes”? 

Well, __a closed surface with no holes is topologically a 2-sphere__. Uniformization says we can choose a metric with constant positive curvature. The canonical model is the 2-sphere. Very importantly, this does not mean the surface becomes a plane. The plane has different topology (it is even not compact).

Remember that the Uniformalization theorem concerns only closed (compact and no boundary) surfaces, and a Euclidean plane is not compact at all. Uniformization does not change the surface into a plane or sphere. It says we can choose a metric on that same surface whose curvature is constant, and the sign of that curvature depends only on the number of holes.

Interestingly, this also reveals one subtle phenomenon when we think about why a torus can be flat. Think of the following construction:

Take a square in the plane and then glue opposite edges. This produces a torus, topologically. Then because the square came from the Euclidean plane, the metric inherited is flat everywhere. Formally this torus is

$$
\mathbb{T}^2 = \mathbb{R}^2/\mathbb{Z}^2
$$

Locally it looks exactly like the Euclidean plane, but globally the edges wrap around. A classic physical analogy is a video game map with periodic boundaries: you walk straight and eventually return to where you started (this might also be used to illustrate rational vs irrational cycling).

Also notice that compact surfaces with non-positive curvature cannot be smoothly immersed in $$\mathbb{R}^3$$. 

[TODO]: This has to do with embedding theorems (Isometric embedding theorems from John Nash and Mikhail Gromov).

You might have a natural question here: aren't there donuts everywhere in coffee shops (thus in this 3D world)? The key issue is the difference between intrinsic curvature and extrinsic shape. The above statement refers to intrinsic flatness, not just the object sitting in 3D space. 

A physical donut shape in $$\mathbb{R}^3$$ is a torus embedded in space. Mathematically this is the standard Torus, but this surface is not __intrinsically__ flat.

If we compute the Gaussian curvature of the standard torus in $$\mathbb{R}^3$$, the outer side has positive curvature whereas the inner side has negative curvature. Consequently, in general the curvature is not zero.

However, here we are talking about _flat_ torus and flat surface means its Gaussian curvature is zero everywhere. For examples, Euclidean plane, cylinder, or a cone (away from the bottom). These are also called developable surfaces: we can cut them and lay them flat without stretching.

The Uniformization Theorem says a genus-1 surface admits a flat metric. It says that it's __possible__ to choose this metric, but not necessarily it has to be a uniform constant metric (Gaussian curvature is intrinsic and could be determined entirely by the choosen metric). The standard construction for such flat torus is the $$T^2 = \mathbb{R}^2 / \mathbb{Z}^2$$ from above. 

This object is intrinsically flat everywhere. The distance is just the Euclidean distance. But it __cannot be embedded smoothly into $$\mathbb{R}^3$$__ without distorting the metric.

Why this is not possible? There is a deep geometric obstruction coming from Gauss’s Theorema Egregium. This theorem says that Gaussian curvature depends only on the metric, not on how the surface sits in space. If a torus embedded in $$\mathbb{R}^3$$
were flat, then its Gaussian curvature would have to be zero everywhere. But any smooth torus embedded in $$\mathbb{R}^3$$ must have regions of positive and negative curvature, so a smooth flat embedding is impossible.

Consequently, the everyday chocolate donut works fine because it "isn't flat". It stretches the metric compared with the flat torus. That stretching creates curvature:

* outer rim: bulges (positive curvature)
* inner rim: saddle-like (negative curvature)

It lives in $$\mathbb{R}^3$$, but not with the flat metric anymore. It could have a flat metric, but then it would not live in $$\mathbb{R}^3$$ anymore. Think of trying to bend a sheet of paper: we can roll it into a cylinder without stretching, but making a donut requires stretching somewhere. Since the flat torus metric forbids stretching, it cannot be realized smoothly in $$\mathbb{R}^3$$. 

A side note: although a flat torus cannot embed smoothly in $$\mathbb{R}^3$$, it can embed in $$\mathbb{R}^4$$. This is one of the first places where higher-dimensional geometry behaves very differently. 

Also to be rigorous, even among flat metrics there are infinitely many choices. A flat torus is determined by a lattice in the plane. Instead of identifying edges of a square, we can identify edges of any parallelogram. Formally:

$$
T^2 = \mathbb{R}/\Lambda
$$

where $$\Lambda$$ is a lattice and different lattices produce different flat geometries. This parameter space is studied in _moduli space_ and _Teichmüller Theory_. 

This hints at the final subtlety we've neglected so far, on what “unique” means in the Uniformization Theorem. As we said, for a closed genus-1 surface the constant curvature must be 0, but there are many flat metrics, not just one. The uniqueness statement is within a __conformal class__, not among all metrics.

Rmemember one of our version of this theorem:

> For any Riemannian metric on a __closed__ surface, there exists a __conformally equivalent__ metric with _constant curvature_, and it is __unique__ within that __conformal class__.

So the key phrase is __conformal class__. Two metrics are conformally equivalent if they differ by scaling with a positive function:

$$
g' = e^{2u}g
$$

As conformality suggests, this preserves angles but changes lengths.

Still use the flat torus example: For a torus, the constant curvature must be 0, so the uniformization metric is flat/ 

But a torus has __infinitely many conformal classes__, and each one produces its own flat metric. Hence there are infinitely many flat torus geometries.








Finally, you might wonder why this has anything to do with our main topic today. Well, as we will learn later, the Ricci Flow evolves the metric so that the surface flows toward the uniformization metric. On a surface it essentially smooths the curvature until it becomes constant, recovering the three cases above.That is why the Uniformization Theorem is the 2-dimensional precursor to the Poincaré Conjecture program in 3 dimensions.

### 0.3] An Important Reminder
If you are still confused, I would suggest dissecting the structure/object you are consideirng into the following three components:

* 1) Topology;
* 2) The (Riemannian) Metric;
* 3) if any, the Embedding in $$\mathbb{R}^3$$

The topology could be determined by the surface classification theorem (if we know the genus, then the topology is fixed), whereas the (Riemannian) metric indicates the intrinsic geometry. And please endow me to reinforce the following:

Geometry does __NOT__ necessarily mean a shape in $$\mathbb{R}^3$$. This is perhaps a subtle point: A metric defines intrinsic geometry, meaning measurements made within the surface itself. We already saw this idea from Gauss in his Theorema Egregium: It says curvature depends only on the metric, but __not on how the surface sits in space__. Consequently, a geometry does not require an embedding into $$\mathbb{R}^3$$.

Some geometries cannot appear as smooth surfaces in $$\mathbb{R}^3$$. For example, the flat Torus exists as an intrinsic geometry but it cannot be smoothly embedded in $$\mathbb{R}^3$$. In fact, Most intrinsic geometries do not correspond to a simple shape in $$\mathbb{R}^3$$. 

Then, an embedding places the surface inside space. For example: the familiar Torus embedded in $$\mathbb{R}^3$$. This embedding induces a particular metric (from Euclidean space), which has positive and negative curvature, and that donut corresponds to one specific geometry among infinitely many possible metrics on the torus.

---

### 0.4] Thurston's Geometrization Conjecture
In dimensions $$n \geq 3$$, curvature is much more complicated. Instead of a single number $$K$$, we have the full Riemann curvature tensor, which contains many independent components. Because of this, manifolds can have wildly different curvature structures and there is no classification into just three geometries --- a direct analogue of uniformization does not exist. 

The closest result is __Thurston's Geometrization Conjecture__, proved using Ricci Flow and completed by Grigori Perelman. Instead of three geometries, there are __8__ model geometries for 3-manifolds, including:

* 1) spherical geometry
* 2) Euclidean geometry
* 3) hyperbolic geometry

and several others like $$\mathbb{S}^2 \times \mathbb{R}$$, Nil, Sol, etc. A 3-manifold decomposes into pieces that each admit one of these geometries.Thus, geometrization is sometimes described as the 3-dimensional analogue of uniformization, but it is much more complicated.

Unfortunately, for higher dimensions there is no general geometric classification theorem comparable to uniformization. Instead, geometers study special problems such as: constant scalar curvature metrics, Einstein metrics, or special holonomy manifolds. 

A deep reason this happens (and one that surprises many people) is that dimension 2 is the only dimension where conformal geometry and curvature are tightly constrained, which is why Riemann surfaces are unusually rigid compared with higher-dimensional manifolds.

---

### 0.5] Poincaré Conjecture

From the previous surface classification theorem and the uniformization theorem we know that a simply connected compact surface is topologically equivalent to a sphere. Can we extend this claim to higher dimensions? Or, what about just 3D? Here's the Poincaré conjecture:

> __Poincaré Conjecture__: Any compact and simply connected three dimensional manifold is topologically equivalent to a three dimensional ball.

The Poincaré conjecture and the higher dimensional counterparts have attracted mathematicians' interest for decades. Numerous brilliant minds are setlted upon strivng to solve it, resulting in all higher dimensions resolved except 3 by the 80s. 



---

### 0.6] Richard Hamilton's Ricci Flow

---

### 0.7] Obstacle and Final Puzzle by Grigori Perelman

---


## 1] Riemannian Geometry and Tensor
The Riemann tensor is written in the following way:

$$
R(X, Y)Z = \nabla_X \nabla_Y Z - \nabla_Y \nabla_X Z - \nabla_{[X, Y]} Z
$$

This tensor captures all information about curvature. Succinctly, this is a 4-tensor: $$R_{ijkl}$$. 

The above is an unfair treatment of Riemannian geometry. I'll have a separate blog on that subject soon. 

How to understand: Riemannian metric tensor informs the manifold where to expand, shrink, and curve.  How does Riemannian metric tensor relate with curvature? 


## 2] Ricci Curvature 
Based on the Riemann tensor, what is the curvature? 

To get Ricci curvature, we take a __trace__ of the Riemann tensor:

$$
Ric_{ij} = R^{k}_{ikj} = g^{kl}R_{kilj}
$$

This reduces the information down to a 2-tensor (like the metric itself). Geometrically, this represents the volume distortion of geodesic balls.


## 3] Ricci Flow
Introduced by `Richard Hamilton (1982)`, Ricci flow is a process that evolves a Riemannian metric $$g(t)$$ over time:

$$
\frac{\partial g_{ij}}{\partial t} = -2 Ric_{ij}
$$

The factor -2 is just convention (to simplify later computations). We could think of this as a heat equation for geometry: Just as heat diffuses to smooth out temperature differences, Ricci flow smooths out __irregularities__ in curvature. As illustrated in the above section of Ricci curvature, we could naturally arrive at the result that positive curvature regions tend to shrink and negative curvature regions expand. Over time, the underlying geometry becomes more "regular”, like ironing out wrinkles. 

The effect of Ricci flow on curvature could be expressed in the following way. The derivative of scalar curvature $$R$$ under this flow is: 

$$
\frac{\partial R}{\partial t} = \Delta R + 2 |Ric|^2
$$

This resembles a heat equation $(\Delta R)$ plus a positive correction. Consequently, curvature tends to diffuse out but also grows in positive-curvature regions.


#### Examples of Ricci Flow
Perhaps the simplest example is to imagine how a sphere evolves under Ricci flow. We all know that a round sphere has positive Ricci curvature. The Ricci flow equation says:

$$
\frac{\partial g_{ij}}{\partial t} = -2 Ric_{ij}
$$

Since Ricci is positive, the metric shrinks, and thus makes the sphere to contract uniformly, eventually collapsing to a point. This closely mirrors the idea that positive curvature makes geodesics converge. Under the Ricci flow, it tightens further.

Another simple example is the flat torus. Since the Ricci curvature is 0 everywhere, 

$$
\frac{\partial g_{ij}}{\partial t} = 0
$$

the torus will stay unchanged after the flow forever. This is analogous to heat diffusion on a perfectly uniform temperature field where nothing would effectively changes. 

Likewise, a hyperbolic surface has negative Ricci curvature. Consequently, under the Ricci flow, the metric would expand and the hyperbolic surface would grow larger and more uniform in curvature.

In a nutshell, irregular geometries with bumps or folds (different curvature in different regions) get “smoothed” over time. All the high-curvature “wrinkles” would get flatten out, like how heat equalizes temperature.


### Application of Ricci Flow
Poincare conjecture, Ricci flow, surgery theory, what Terrence Tao called "one of the most impressive recent achievements of modern mathematics"

Poincare conjecture: 
> Any closed 3-manifold that is simply-connected, compact, and boundless is homeomorphic to a 3-sphere. 

Specifically, Poincare conjecture in higher dimensions has been solved around 1961, and dimension 4 case has been proved by Michael Freedman who by which won Fields medal in 1986. The $$n = 3$$ case seemed really difficult to crack and it was only at 2002 that Grisha Perelman proved it using Ricci flow.

Very briefly, since a sphere has positive curvature, by applying Ricci flow through time such sphere will contract and eventually vanish. Perelman proved the opposite also holds: if metric goes to 0, it must have been a sphere. To prove Poincare's conjecture using Ricci flow, 

One of the most triumphant use of Ricci flow happens when Grigori Perelman (2002–2003) to prove __the Poincaré conjecture__ and the more general __Thurston geometrization conjecture__. He showed how Ricci flow with “surgery” (cutting and patching when singularities form) classifies 3-manifolds.

---

## 4] Heat Flow