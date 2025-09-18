---
layout: post
title: 'Geom/Topo/Dynam Mumble(2): Ricci Flow and Ricci Curvature'
date: 2025-09-17 17:49:33
description: Introduction of Ricci flows/curvatures 
tags:
    - "Geometry Concepts/Tools"
categories: 
    - "Differential Geometry"
featured: true
related_posts: true
related_publications: true
citation: true
math: true
toc:
  beginning: true
  sidebar: left
---

### Introduction
Today I will cover a beautiful subject in differenetial geometry: Ricci flows and Ricci curvature. 

Generally, in Riemannian geometry, curvature measures how space bends. For instance, on a sphere, geodesics (shortest paths) come closer together compared to flat space; on a hyperbolic surface, they diverge.

Ricci curvature is a particular way of summarizing curvature: Instead of describing how all directions bend (that’s what the full __Riemann curvature tensor__ does), Ricci curvature focuses on __volume distortion__. More concretely, it tells us how the volume of a small geodesic ball deviates from the volume we'd expect in flat Euclidean space.

Intuitively, we could summarize into the following: 
> Positive Ricci curvature (like on a sphere) means geodesics tend to converge, and small balls have less volume than in flat space.
>
>Zero Ricci curvature (like in Euclidean space) means geodesics neither converge nor diverge, so volumes match Euclidean.
>
>Negative Ricci curvature (like on a hyperbolic space) means geodesics diverge, so small balls have more volume than Euclidean.

Mathematically, Ricci curvature is obtained by “tracing” the __Riemann curvature tensor__. It compresses information about how different directions curve into a symmetric 2-tensor `Ric`. 

### Riemannian Geometry and Tensor
The Riemann tensor is written in the following way:

$$
R(X, Y)Z = \nabla_X \nabla_Y Z - \nabla_Y \nabla_X Z - \nabla_{[X, Y]} Z
$$

This tensor captures all information about curvature. Succinctly, this is a 4-tensor: $$R_{ijkl}$$. 

The above is an unfair treatment of Riemannian geometry. I'll have a separate blog on that subject soon. 


### Ricci Curvature 
To get Ricci curvature, we take a __trace__ of the Riemann tensor:

$$
Ric_{ij} = R^{k}_{ikj} = g^{kl}R_{kilj}
$$

This reduces the information down to a 2-tensor (like the metric itself). Geometrically, this represents the volume distortion of geodesic balls.


### Ricci Flow
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
One of the most triumphant use of Ricci flow happens when Grigori Perelman (2002–2003) to prove __the Poincaré conjecture__ and the more general __Thurston geometrization conjecture__. He showed how Ricci flow with “surgery” (cutting and patching when singularities form) classifies 3-manifolds.

