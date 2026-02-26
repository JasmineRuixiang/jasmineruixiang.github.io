---
layout: post
title: 'The Dance of Space: Geom/Topo/Dynam Mumble(5)'
date: 2026-02-08 14:58:02
description: Geodesic and its construction, Gauss Theorem Egregium, Gauss-Bonnet and Chern
tags: 
    - "Manifold"
    - "Differential Geometry"
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

This weekend I was (re)thinking about the geometric connotations of geodesics, and that reminds me of a brilliant illustration with an intuitive method the eloquent mathematician Tristan Needham noted down in his remarkable and mind-numbing book _Visual Differential Geometry and Forms_. I thought just a little more, and only to recover a long-lasting misconception about Gauss Theorem Egregium which I was both shameful to admit for possessing so long and excited to have it cleared out of my mind. 

However, as I dive more into Needham's method, I discovered a potential and simple failure mode, which naturally echoes another important statement: The Gauss-Bonnet Theorem. This deepens my grasp of the tension between __local isometry__ (the peeling method, see below) and **global topology** (the area enclosed).

Again, since this is the mumble series, I'll not give all definitions and assume you have already known some flavor of the basics. Let's begin. 

---

## 0] The Problem (Confusion) Setup
On Page 12-13, Tristan introduced a method to easily and intuitively construct geodesics on a curved surface (see figure [1.11] below). He claimed that "If a narrow strip surrounding a segment G of a geodesic is cut out of a surface and laid flat in the plane, then G becomes a segment of a straight line." Well, he presented an intuitive proof (figure [1.12] below), but I'm immediately reminded of __Gauss' Theorem Egregium__ and there seemed to be something inconsistent (I'll omit other information and count on you to look up the basics). 

I was thinking: the peeling is an (local) isometry without doubt, so geodesics have to be preserved. That's why geodesics "peeled" off from the surface (the fruit/vegetable used in the illustration) has to be geodesics in Eucliean 2D space, which is straight according to the normal definitions. However, as I'm thinking a bit more deeply, the Gaussian curvature does change (before it's nonzero, on 2D it's zero, which contradicts Theorem Egregium), which leads to me to think backwards towards using this Theorem Egregium again. Immediately I realize that Gaussian theorem egregium is applied only to 2D surfaces (not to 1D curve since there's no "Gaussian curvature" for a curve). Ah, shame to have blundered upon conceptual confusion. But treating this as an opportunity for an upgrade overhaul of my conceptual framework, let's sort this out step by step and see what we might also dabble into. 

<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 text-center">
        {% include figure.liquid 
            loading="eager" 
            path="assets/img/VDGF/VDGF_1.11.png" 
            class="img-fluid rounded z-depth-1" 
            zoomable=true
            width="100%" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0 text-center">
        {% include figure.liquid 
            loading="eager" 
            path="assets/img/VDGF/VDGF_1.12.png" 
            class="img-fluid rounded z-depth-1" 
            zoomable=true
            width="100%" %}
    </div>
</div>

<div class="caption">
    Adapted from Fig [1.11] and [1.12] in {% cite VDGF %}.  
</div>


## 1] The "Peeling" Intuition
Let's make it clear: in Figure [1.11] of Needham's text, he introduces the following powerful intuitive tool: 
> "If a narrow strip surrounding a segment $G$ of a geodesic is cut out of a surface and laid flat in the plane, then $G$ becomes a segment of a straight line."

At first glance, this indeed seems to challenge Gauss's **Theorema Egregium**. Since Gaussian curvature ($$K$$) is an intrinsic invariant, how can a patch of a curved surface ($$K \neq 0$$) be laid flat ($$K = 0$$) without stretching or tearing? 

Well, the resolution lies in the **limit**. Gaussian curvature is a property of a 2D area. By taking a "narrow strip," we are effectively reducing the 2D surface to a 1D curve and its immediate neighborhood. In the limit, as the width of the strip goes to zero, the "area" of the surface being considered goes to zero. In other words, the Gaussian curvature of the surface does not change because we aren't flattening the whole surface; we are only flattening an "infinitesimally" narrow strip.

---

## 2] Geodesic Curvature vs. Gaussian Curvature
The Theorema Egregium applies to __isometries__. While we cannot map a patch of a sphere to a plane isometrically, we can map a curve and its first-order neighborhood to a plane isometrically (often called "developing" the strip; or just developable). 

We could think of it this way:
> The Surface: Has intrinsic curvature $$K$$.
> The Strip: Because it is "infinitesimally narrow," the Gaussian curvature doesn't "__trap__" the strip. We aren't forcing the 2D relationships across the strip to remain the same; we are only preserving the lengths along the geodesic $$G$$.

Consequently, the core of the questions (why such an intuition makes sense, beyond the simple proof shown in Fig.[1.12]) does not lie in Gauss's Theorem, but in another concept: the geodesic curvature: The Theorema Egregium is about the surface ($$K$$), but the property of being a "straight line" is about Geodesic Curvature ($$\kappa_g$$):

> Gaussian Curvature ($$K$$) reflects the property of the surface.
> Geodesic Curvature ($$\kappa_g$$): A property of a curve relative to the surface. It measures how much the curve bends within the surface.

The "straightness" of the peeled strip is governed by **Geodesic Curvature ($\kappa_g$)**, not Gaussian Curvature ($$K$$).

* **Gaussian Curvature ($$K$$):** A property of the surface itself ($$K = \kappa_1 \kappa_2$$). It dictates whether a 2D patch can be flattened.
* **Geodesic Curvature ($$\kappa_g$$):** A property of a curve *relative* to the surface. It measures how much the curve "veers" to the left or right within the surface.

Recall that a geodesic is defined as a curve where $$\kappa_g = 0$$ everywhere (Will do a another blog on the computation of geodesics from the perspective of modern (Riemannian) manifold). Because $$\kappa_g$$ is an intrinsic property (it can be measured by flatlanders like intelligent ants living on the surface), it must be preserved under isometry.

With this, we could demystify the underpinning logic of the peel:

1. The act of peeling the strip is a local isometry along the curve.
2. The $$\kappa_g$$ of the curve on the surface was 0 (by definition of a geodesic).
3. Therefore, the $$\kappa_g$$ of the curve on the flat plane must also be 0 .
4. In the Euclidean plane, a curve with zero curvature is a straight line.

Notice that the 3rd step above relies on the fact that isometry preserves intrinsic properties and here the geodesic curvature, but again this is NOT Gauss Theorem Egregium (same fundamental fact but applied to different objects). 

Gaussian curvature is not defined for a 1D curve. A curve only has curvature (how much it bends in space) and geodesic curvature (how much it bends relative to the surface it's on). By narrowing the strip until the 2D "surface" nature of the paper effectively vanishes, we are bypassing the restriction of the Theorema Egregium regarding the 2D area, while retaining the intrinsic measurement of the curve's straightness. We are essentially "cheating" the theorem by reducing the 2D surface to a 1D line where the concept of Gaussian Curvature has no grip.

The Theorem Egregium is a negative constraint (it tells us what we can't do with a 2D patch), but the Geodesic Curvature ($$\kappa_g$$) is the positive proof. It yet emphasizes again that it's important to separate the topological "budget" of the surface (the Gaussian Curvature) from the local behavior of the curve (the Geodesic Curvature).

There's only one last piece left to make the story rigorous.

---

## 3] Physical Approximation

Let me paraphrase again in another way. This narrow strip, in practice, since it cannot be infinitesimally narrow, by Theorem Egregium, __CANNOT__ lie flat on the plane, but the more narrow we could get, the better aligned it is with respect to the plane. If it goes to limit, then Gaussian Theorem Egregium does not apply because this limit 1D curve is no longer constrained by this theorem.

On the other hand, even in practice we assume that after peeling it "is" flat, this appeal to Theorem Egregium still does not "prove" the fact that once laid down the strip would become straight; we still need to assort to the preserved geodesic curvature, which is covered above. 

Now, back to the reality. In practice, a strip of finite width $$w$$ cannot lie perfectly flat if $$K \neq 0$$. The "error" or distortion required to flatten it is proportional to the area of the strip ($$Area \approx Length \times w$$). As $$w \to 0$$:
* The area vanishes.
* The constraint of the Theorema Egregium vanishes.
* The 1D "straightness" remains perfectly preserved.

Another way to view Needham’s trick is that the strip isn't just being "flattened"; it is being identified with the unrolled __tangent developable__ of the geodesic. If we imagine a sequence of tangent planes along the geodesic, they form a "ribbon" that has zero Gaussian curvature (because it’s a developable surface, like a cylinder or a cone). Because this ribbon has $$K=0$$, it can be laid perfectly flat in a plane without any distortion at all. Needham's "narrow strip" is essentially a physical approximation of this developable ribbon. 

---

## 4] Extension of the original trick: Connection to Gauss-Bonnet

### 4.1] Failure mode of a strip when in closed loop
Now with the above puzzle cleared out, let's think a little deeper into the next single, perhaps most natural topic: How about peeling off not just a single strip segment, but instead a strip which loops back into itself?

Think about the physical reality of Needham's experiment. If we cut a strip along a great circle (a closed geodesic) of a sphere:

* 1] The Segment: If we cut just a small arc (say 30°), the strip is essentially a tiny rectangle. Because it's so narrow, the "surface tension" of the sphere's curvature isn't strong enough to prevent us from pressing it flat.
* 2] The Loop: If we try to cut the entire great circle, we get a "ring" or a "hoop." On the sphere, this hoop has a specific circumference ($$C = 2\pi R$$).
* 3] The Failure: If we try to lay that hoop perfectly flat on a table without stretching it, we'll find it impossible. To lie flat in Euclidean space as a circle, the relationship between its radius and circumference must be $$C = 2\pi r$$. But on the sphere, the "radius" (the distance from the pole to the equator) is an arc length. __The geometry of the 2D area inside the hoop "locks" the hoop's shape__.

Or maybe another perhaps more intuitive example ---

> The "Paper Cone" Analogy: 

Think of a paper cone (like a party hat).

* 1] Local Flattening: You can cut a narrow strip from the cone running from the tip to the base. You can lay this strip flat on the table perfectly. In fact, you can lay any part of the cone flat.

* 2] Global Failure: But if you try to flatten the _entire_ cone at once, you can't. You have to make a cut. When you flatten it, the cut edges don't meet; there is a angular gap.

Yet perhaps another thought experiment:

> The "Train Track" Experiment

Imagine the "narrow strip" as a set of flexible but straight train tracks.

On the Sphere: You lay the tracks along the equator. They go all the way around and connect perfectly at the start.

The "Peeling" (Transfer to Plane): Now you transfer these tracks to a flat Euclidean floor. Because the tracks are geodesics (straight), you must lay them down as a straight line on the floor. You keep laying them down, inch by inch.

Now the Problem: On the floor, a straight line goes on forever. It never comes back to start.

The Contradiction: To make the tracks close a loop on the floor, you would have to bend them (add Geodesic Curvature). But we know geodesics are straight!

So, the "narrow strip" of a closed geodesic loop on a sphere becomes an infinite straight line on the plane. It loses its "loop-ness" entirely.

Later we will make it clear that this "angular gap" is precisely what the Gauss-Bonnet Theorem calculates ($$\iint K dA$$). The curvature $$K$$ inside the loop on the sphere is responsible for "turning" the geometry so that it closes. The flat plane ($$K=0$$) lacks this "turning power," so the strip simply runs away in a straight line.

But anyway, for now, in short: We can flatten a line because a line has no "inside." We cannot flatten a closed loop without accounting for the gap (which is what we will show later as the holonomy, or "equivalently" integration of the Gaussian curvature of the area it encloses.)

---

### 4.2] The formula and geodesic-loop
We resort to The Gauss-Bonnet Theorem, which almost fits in immediately, since it bridges the gap between the local straightness of the geodesic and the global curvature of the surface. 

Well, the Gauss-Bonnet Theorem is essentially a "budgeting" equation. It tells us exactly how much "straightness" we have to give up to account for the curvature of the surface. Let's see if we could intuitively see its effect from the closed loop peeling failure. 

Let me state the theorem:

For a simply connected region $$R$$ bounded by a curve $$C$$, the theorem states:

$$\iint_R K \, dA + \oint_C \kappa_g \, ds + \sum \alpha_i = 2\pi$$

$$K$$ is the Gaussian curvature, $$\kappa_g$$ is the geodesic curvature, $$\alpha_i$$ are the exterior angles at any corners.

Moreover, if we create a closed loop (like a triangle) using only geodesic segments, then $$\kappa_g = 0$$ along the edges by definition. The middle term of the equation vanishes, leaving a direct relationship between the "area-integral of curvature" and how much the geodesics had to "turn" at the corners to close the loop:

$$\iint_R K \, dA = 2\pi - \sum \alpha_i$$

Furthermore, if we use a smooth closed geodesic, there is no geodesic curvature ($$\kappa_g = 0$$) and also no corners ($$\sum \alpha_i = 0$$). The equation thus becomes:

$$\iint_R K \, dA = 2\pi$$

This tells us that for a smooth closed geodesic to exist, the total Gaussian curvature of the area it encloses must equal $$2\pi$$ (the "angle" of a full circle). This is why we can have a closed geodesic on a sphere ($$K > 0$$), but we can never have a simple closed geodesic on a flat plane ($$K=0$$) or a saddle ($$K<0$$)—the "curvature budget" doesn't add up to $$2\pi$$.

Let us dwindle here for a while, as I feel like building up intuition, a feeling of this curvature budge is of significant importance. So let's just try again to reinterpret this equation by the following simple example. 

To walk in a simple closed loop (a circle, a square, a blob) and end up facing the same way we started, we must physically turn a total of 360° ($$2\pi$$ radians).

However, here the Gauss-Bonnet theorem says there are two ways to pay for this 360° budget:

> 1. Steering ($$\kappa_g$$): we physically turn our body (like turning a steering wheel).
> 2. Surface Curvature ($$K$$): The ground itself curves underneath us, effectively "turning" us without we realizing it.

The equation is thus: 

$$\text{Steering} + \text{Surface Curvature} = 360^\circ$$
$$\oint \kappa_g ds + \iint K dA = 2\pi$$

Note that a geodesic is defined as a path where we do not steer. Our steering wheel is locked in the straight-ahead position.

Therefore: Steering = 0. 

Now look at our budget equation again:

$$0 + \text{Surface Curvature} = 360^\circ$$
$$\iint K dA = 2\pi$$. 

This means the only way to close a loop without steering is if the surface itself does 100% of the turning for us.

Consequently, this would explain why it fails on the plane ($$K = 0$$). The surface or the plane is flat, which contributes 0° of turning. The geodesic contributes 0° of steering. Hence the result: $$0 + 0 = 0$$. We have turned 0° and we are walking in a straight line forever. We will never close the loop. Thus our conclusion: simple closed geodesics cannot exist on a plane.

Similarly, simple closed geodesics cannot exist on a saddle, because a saddle has negative curvature. It curves "away" from itself and  contributes negative turning. Along a geodesic there's no contribution of steering, and the net result is still negative, not $$2\pi$$. We are actually diverging away from a closed loop. The surface is actively pushing us path apart. 

In contrast, a sphere has positive curvature. It curves "inward" toward itself and contributes positive turning. If we enclose enough area (specifically, a hemisphere), the total positive curvature adds up to exactly $$2\pi$$. The surface has bent us around exactly enough to meet our own tail without us ever turning the wheel.

In some sense, the "Curvature Budget" is like a tax. To close a loop, we must pay $$2\pi$$. On a Geodesic, we refuse to pay (we won't steer). Therefore, the Landlord (Surface) must pay the entire tax for us. Only a positively curved landlord (Sphere) has the cash ($$K>0$$) to pay it. The plane is broke ($$K=0$$), and the saddle is in debt ($K<0$).

Well, you may wonder in the above interpretation where do the exterior angle sum go. Recall that we're talking about smooth geodesic loops. Because the loop is smooth, there are no sharp corners ("kinks"), so there are no exterior angles to sum up. The term $$\sum \alpha_i$$ becomes exactly 0. You may also wonder, then how would geodesic "turn" be different from the exterior angle kink turn? This is a little more subtle, but also intuitive. 

> The "Conservation of Turning"
* Usually, when we smooth out a shape (like turning a square into a circle), we don't lose the turning angles; we just spread them out.
* Square (Polygon): We walk straight ($$\kappa_g=0$$) and make four sharp $$90^{\circ}$$ turns. 

- $$\text{Turning} = \sum \alpha_i = 360^{\circ}$$. I'd more think about these sharp turns/kinks as some colossal mechanical arm sticking from space and grabs our car to turn it around certain angles at the current position. 
* Circle (Smooth Curve): We never make a sharp turn ($$\alpha_i=0$$), but we are constantly steering a tiny bit to the side ($$\kappa_g = \text{constant}$$). 

- $$\text{Turning} = \oint \kappa_g \, ds = 360^{\circ}$$. 

In both cases, we provided the turning. However, for the geodesic "miracle", a smooth closed geodesic is a bizarre object because it refuses to turn in either way: there's no sharp corners (smooth, $$\sum \alpha_i = 0$$) and no steering (geodesic, $$\kappa_g = 0$$). So where does the mandatory $$360^{\circ}$$ ($$2\pi$$) turning come from to close the loop? It must come entirely from the Gaussian Curvature ($$K$$) of the area we enclosed. The surface itself has to rotate the universe under our feet by exactly $$360^{\circ}$$ while we walk in a "straight" line.

---

### 4.3] How does the "strip" fit in?
Now, imagine applying Needham's "peeling" trick to each side of a geodesic triangle. 

> 1. On the surface: We would have three "straight" paths (geodesics) that enclose a region of curvature $$K$$. Because of that $$K$$, the interior angles sum to more than $$\pi$$ (on a sphere).
> 2. __The "Peeling" Conflict__: If we tried to peel a "narrow strip" that followed the entire boundary of the triangle and lay it flat in one go, we would encounter a physical gap or an overlap where the ends meet. 

The Gauss-Bonnet Theorem effectively measures this "gap." The amount of Gaussian curvature "trapped" inside the triangle is exactly equal to the "holonomy"—the amount a vector rotates when transported around that loop:



---

### 4.4] Holonomy, Parallel Transport, and the "Gap"
There's a simple and intuitive relationship between holonomy, parallel transport, and exterior angles. 

Let's start with a simple thought experiment about parallel transport (you might have seen this everywhere):

Imagine walking along a geodesic triangle on a sphere, carrying a spear (a vector) pointing straight ahead.

- Along the edge: Because we are on a geodesic, we never turn our "steering wheel." The spear stays parallel to our path.
- At the corner: we stop and turn our body by an exterior angle ($$\alpha_i$$). We do not turn the spear; it still points where it was pointing.
- Back at the start: When we complete the loop, we compare the spear's current direction to its starting direction. They won't match. This net rotation of the vector after the full trip, the "error" in direction, is called the __Holonomy ($$\Delta \theta$$)__.

The above way of sliding a vector along a geodesic while keeping it "parallel" is called __Parallel Transport__. 

At the same time, just by some simple calculation we would know that for the total turn on a flat plane, our total change in heading (sum of exterior angles $$\sum \alpha_i$$) must be $$2\pi$$ _to close a loop_. In other words, the holonomy $$\Delta \theta = 0$$. However, on a curved surface, the amount we actually turned, the holonomy, is $$2\pi - \sum \alpha_i \neq 0$$. 

Also notice that if we do parallel transport along the "peeled" flat strip, the vector remains parallel in the Euclidean sense because the strip is a straight line. 

However, when we close the loop, 
* On the flat plane, the vector would return to its start pointing in the original direction.
* On the curved surface, the vector returns rotated by an angle $$\Delta \theta$$.

The theorem tells us that this holonomy $$\Delta \theta$$ is precisely the integral of the Gaussian curvature over the area we bypassed:

$$\Delta \theta = 2\pi - \sum\alpha_i = \iint_R K \, dA$$

Intuition Check: If you are on a flat plane, $$K=0$$. Therefore, $$\iint K dA = 0$$. This means $$0 = 2\pi - \sum \alpha_i$$, or $$\sum \alpha_i = 2\pi$$. This is just some high-school geometry rule that the exterior angles of any polygon sum to 360°. On a sphere, the curvature $$K$$ "helps" us turn, so we don't need as much "exterior angle" to close the loop.

Consequently, if we apply Needham's trick, i.e., peel off the strip of geodesic loop, we would find that 

> While we can peel a **single** geodesic segment and lay it **flat** perfectly, we **cannot** peel a **closed loop** of geodesics and lay the resulting "frame" flat in the plane without a gap or overlap. 

The "angle" of that gap is the **holonomy**. The Gauss-Bonnet theorem tells us that this gap is exactly equal to the total Gaussian curvature "trapped" inside the area we just cut out. 

* **On a Sphere ($$K>0$$):** The geodesics turn "toward" each other, and the interior angles sum to $$>\pi$$.
* **On a Saddle ($$K<0$$):** The geodesics flare "away" from each other, and the interior angles sum to $$<\pi$$.

---

## Conclusion
Needham’s trick works because a 1D line has no "area" to trap curvature. The "straightness" we see on the paper is the physical manifestation of zero geodesic curvature, an intrinsic property that survives the transition from the fruit's skin to the flat desk.