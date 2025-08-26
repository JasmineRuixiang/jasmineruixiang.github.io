---
layout: post
title: Constraints upon Learning from a Neural Manifold Perspective 
date: 2025-08-25 22:09:34
description: Employ perturbations of neural manifold to explore learning constraints
tags: 
    - "Learning"
    - "Neural Manifold"
    - "Dimensionality Reduction"
categories: 
    - "Brain-Computer Interface"
featured: true
related_posts: true
related_publications: true
citation: true
math: true
toc:
  beginning: true
  sidebar: left
---


## Experiment setup
For {% cite Sadtler2014 %}, two male Rhesus macaques were trained to perform closed-loop BCI cursor task (Radial 8). Around 85-91 neural units (threshold-crossings) were recorded. The experiment pipeline is demonstrated below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/fig.1a.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="65%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig. 1a in {% cite Sadtler2014 %}. Note that on the right hand side the authors already presented where to place the two kinds of perturbation. The color scheme (green, yellow, and black) is consistent throughout figures in this paper.
</div>


## Dimensionality reduction paradigm
The control space is just 2D because the decoder output is cursor velocities in $$\mathbb{R}^2$$, illustrated as black line in Fig.2 black line (Note: it's actually a 2D plane, but here for simplicity shown as a black line ($$\mathbb{R}^1$$)). 

They used XXX to extract what they called the "intrinsic manifold", which captures the co-modulation patterns among the recorded neural population. This is shown as the underlying yellow plane in Fig.2 (might be confusing, but it's not the 2D control space). Note that at the time of publication, neural manifold was not yet in a popular trend, so the authors briefly characterized the term "intrinsic manifold" with the following illustration:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/fig.1b.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="65%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig. 1b in {% cite Sadtler2014 %}. .
</div>


## Perturbation method
Then the core methodology of this study is to change the BCI mapping so that the altered control space would be lying either within or outside of the insintric manifold. The paper does present some confusion as to how intuitive mapping and control space would be distinguished. My interpretation is that the control space refers to the ideal potential neural subspace for which to control the cursor optimally. Since within a short time neural connectivity is kept unaltered, the true intrinsic manifold is approximately invariant and thus the required potential neural subspace might not be reachable. By default the control space/intuitive mapping lies within the intrinsic manifold (that's why it's called "intuitive", because that's is what the neural network system has learned to achieve). 

In short, a within-manifold perturbation only reoriented the control space such that it still resides in the intrinsic manifold (shown in Fig.3 red line). This does not require monkeys to readapt neuronal connectivity patterns to achieve such new control space. It only altered the function from the intrinsic manifold to cursor kinematics. On the other hand, an outside-manifold perturbation alters the control space allowing it to live off the intrinsic manifold (Fig.3 blue line). Notice that if such outside-manifold perturbation pushes the control space along the orthogonal subspace that passes through the original control space, then the mapping from the neural comodulation patterns to cursor kinematics is preserved (basically just project the altered control space to the intrinsic manifold, then would recover the original control space). However, the underlying comodulation/covariation patterns among the neural population are altered. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/fig.1c.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="65%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig. 1c in {% cite Sadtler2014 %}. .
</div>

After the perturbation, the authors observed if the monkeys could eventually learn to readapt to the new mapping, to achieve great cursor control performance. For within-manifold perturbation, the monkeys only need to learn to associate cursor kinemaitcs to a new set of neural comodulation patterns (still within reach because lying in the same intrinsic manifold). However, for outside-manifold perturbation, they had to generate new co-modulation patterns in order to reach outside of the existing intrinsic manifold. Consequently, the authors predicted that within-manifold perturbation is easier to learn compared to outside-manifold perturbation. The authors did find results to back up this claim and since they are not the main focus of this blog, I'll refer interested readers to the original paper to take a look (FIgure 2). 

Other than that, I do want to dive deep into how such within/outside-manifold perturbations were implemented. Specifically, 



## Discussions
Since Sadtler et. al. employed closed-looop BCI control, they were able to causally alter the model/map from neural activities to the decoded cursor velocities. 

This paper highlights a potential methodology of BCI research: since the mapping from neural activities to control correlates is __fully specified__, thus could be causally perturbed to explore the corresponding changes of controlled behavior. This allowed the authors to __design/know apriori__ the optimal/required neural activities (specified by the altered mapping) to achieve task success, and thus to observe if animals could generate such neural patterns. 

An outside-manifold perturbation does not necessarily specify that it lives in orthogonal subspace of the intrinsic manifold. Also, because here the intrinsic manifold is illustrated as a plane, in real world scenario, it is unlikely that it exists as a linear subspace. Consequently, specifying a space that is "orthogonal" to the potential manifold (nonlinear, other than a linear subspace) might be problematic. 


## Conclusions
The neural manifold reflects the inherent connectivity which constrains (in a short term) the potentially learnable patterns. 

