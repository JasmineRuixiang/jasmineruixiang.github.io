---
layout: post
title: Constraints upon learning from a neural manifold perspective 
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


# Neural constraints on learning
## Experiment setup
For {% cite Sadtler2014 %}, two male Rhesus macaques were trained to perform closed-loop BCI cursor task (Radial 8). Around 85-91 neural units (threshold-crossings) were recorded. The experiment pipeline is demonstrated below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Fig_1a.png" class="img-fluid rounded z-depth-1" zoomable=true
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
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Fig_1b.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="65%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig. 1b in {% cite Sadtler2014 %}. .
</div>

The authors further elaborated on the intrinsic manifold and its associated dimensionality at the end of the paper. For consistency of comparisons, Sadtler et. al. used a __linear__ 10-D intrinsic manifold across all days. They then performed some offline analysis to explore if 10 is a legitimate choice. Specifically, they estimated the intrinsic dimensionality (EID) as the peak of maximal cross-validated log-likelihood (LL). In panel a the vertical bars represent the standard error of LL from across 4 cross-validation folds. Panel b. shows EID for all days and both 2 monkeys. The authors also showed the LL difference between 10D manifold vs manifold with EID (panel c., with units being the the number of standard errors of LL for the EID model). From panel c. the authors observed that 89% of days the 10-D manifold only differs within one standard error of LL with the EID manifold. Panel d. indicates the cumulative explained variance by the 10-D manifold. Notice that 10 dimensions already explained almost all neural variance. 



## Perturbation method
Then the core methodology of this study is to change the BCI mapping so that the altered control space would be lying either within or outside of the insintric manifold. The paper does present some confusion as to how intuitive mapping and control space would be distinguished. My interpretation is that the control space refers to the ideal potential neural subspace for which to control the cursor optimally. Since within a short time neural connectivity is kept unaltered, the true intrinsic manifold is approximately invariant and thus the required potential neural subspace might not be reachable. By default the control space/intuitive mapping lies within the intrinsic manifold (that's why it's called "intuitive", because that's is what the neural network system has learned to achieve). 

The neuronal connectivity statistics is referred to as the natural co-modulation patterns.

In short, a within-manifold perturbation only reoriented the control space such that it still resides in the intrinsic manifold (shown in Fig.3 red line). This does not require monkeys to readapt neuronal connectivity patterns to achieve such new control space. It only altered the function from the intrinsic manifold to cursor kinematics. On the other hand, an outside-manifold perturbation alters the control space allowing it to live off the intrinsic manifold (Fig.3 blue line). Notice that if such outside-manifold perturbation pushes the control space along the orthogonal subspace that passes through the original control space, then the mapping from the neural comodulation patterns to cursor kinematics is preserved (basically just project the altered control space to the intrinsic manifold, then would recover the original control space). However, the underlying comodulation/covariation patterns among the neural population are altered. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Fig_1c.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="50%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig. 1c in {% cite Sadtler2014 %}. .
</div>

After the perturbation, the authors observed if the monkeys could eventually learn to readapt to the new mapping, to achieve great cursor control performance. For within-manifold perturbation, the monkeys only need to learn to associate cursor kinemaitcs to a new set of neural comodulation patterns (still within reach because lying in the same intrinsic manifold). However, for outside-manifold perturbation, they had to generate new co-modulation patterns in order to reach outside of the existing intrinsic manifold. Consequently, the authors predicted that within-manifold perturbation is easier to learn compared to outside-manifold perturbation. The authors did find results to back up this claim and since they are not the main focus of this blog, I'll refer interested readers to the original paper to take a look (FIgure 2). 

Other than that, I do want to dive deep into how such within/outside-manifold perturbations were implemented. Specifically, 

## Quantifiable metric
### Amount of the learning
To quantify the potential amount of learning under two perturbation kinds, the authors resorted primarily to two performance metric: (the change of) relative acquisition time and relative success rate across perturbation blocks. Specifically, as shown below, the black dot represents the intuitive mapping, while the red and blue dots indicate the imediate performance just after corresponding perturbations. Red and blue asterisks represent the best performance during the within the perturbation sessions. The dashed line indicates the maximum learning vector $$l_{max}$$ (note that it starts on the red dot), and thus the aount of learning ($$L \in \mathbb{R}$$) is quantified as the length of the projection of the raw learning vector onto the maximum learning vector, normalized by the length of the maximum learning vector:

$$
L = \frac{l_{raw, i} \cdot l_{max}}{||l_{max}||^2}
$$

where $$i \in \{red, blue\}$$. Pictorially, it's illustrated as below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Fig_2cd.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="65%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig.2c and Fig.2d in {% cite Sadtler2014 %}. .
</div>

The amount of learning for all sessions was presented above in the right pannel. Notice that a value of 1 indicates "complete" learning of the new relationship between the required neural co-modulation and cursor kinematics, reverting to the performance level of the intuitive control, while 0 indicates no learning. The authors did observe that there's significant amount of learning for within-manifold perturbations than outside-manifold perturbation. To see changes in success rates and acquisition time during perturbation blocks, instead of a single metric $$L$$ as shown above, the authors also plotted them separately as below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Ext_Fig_2.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="65%" %}
    </div>
</div>
<div class="caption">
    Adapted from Extended Figure 2 in {% cite Sadtler2014 %}. .
</div>

### After-effects
A second metric Sadtler et. al. employed is observe how the monkeys performed as they reintroduce the intuitive mapping, or the so-called after-effects after washout of the perturbed mapping. Specifically, the after-effect is measured as the amount of performance imparement (tentative: acquisition time, success rate) at the beginning at the wash-out block (like how impairement was measured at the beginning of a perturbation block). A large wash-out effect indicates that the monkeys have learned and adapted to the perturbed mapping. For within-manifold perturbation, the authors did observe brief impaired performance but not so for outside-manifold perturbation, indicating that learning did occur during the former. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Ext_Fig_3.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="65%" %}
    </div>
</div>
<div class="caption">
    Adapted from Extended Figure 3 in {% cite Sadtler2014 %}.
</div>



## Discussions
Since Sadtler et. al. employed closed-looop BCI control, they were able to causally alter the model/map from neural activities to the decoded cursor velocities. 

This paper highlights a potential methodology of BCI research: since the mapping from neural activities to control correlates is __fully specified__, thus could be causally perturbed to explore the corresponding changes of controlled behavior. This allowed the authors to __design/know apriori__ the optimal/required neural activities (specified by the altered mapping) to achieve task success, and thus to observe if animals could generate such neural patterns. 

An outside-manifold perturbation does not necessarily specify that it lives in orthogonal subspace of the intrinsic manifold. Also, because here the intrinsic manifold is illustrated as a plane, in real world scenario, it is unlikely that it exists as a linear subspace. Consequently, specifying a space that is "orthogonal" to the potential manifold (nonlinear, other than a linear subspace) might be problematic. 

The amount of learning is entirely dependent upon performance itself, which is difficult to causally link to neural changes. 

For the amount of learning metric, why would 0 indicate no learning? Orthogonal learning?

Notice that the after-effects analysis echoed a lot of research methodology in force-field or curl-field perturbation for cursor control in monkey motor control studies. 

The authors also showed that learning did not improve through sessions (readers might refer to Extended Figure 4 in {% cite Sadtler2014 %} for further information). 

The perspective and consideration that Sadtler et.al took to ensure alternative explanations for the observed distinction of learnability do not hold are informative. I enjoyed its rigorosity, especially when they considered perturbed maps which might be initialy difficult to learn and carefully implement the controls (demonstrate that they have controled). To not diverge from the main focus of this blog, I'll not cover those dicussions. The way that the authors listed clearly alternative explanations and how they tackled each is a very inviting, powerful, and efficient way of writing. 

From the methods the authors used, they only estimated a linear manifold. According to {cite}, linear manfolds should require more dimensions than a nonlinear manifold which could explain similar amount of variance. 



## Conclusions
The neural manifold reflects the inherent connectivity which constrains (in a short term) the potentially learnable patterns. Consequently, the neural connectivity network structure dictates possible neural patterns and corresponding behavior repertoire the animals are capable of performing. 

This paper strengthens my belief in the legit usability of the low dimensional structure among neural population, and more crucially the value of perturbation methods to causally verify the neural manifold. 


# Batista 2025

## Discussions


## Conclusions


# Discussions
These two studies offer powerful information that dimensionality reduction could be not just a visualization tool, but a causal summary of the underlying neural connectivity and anatomical constraints, which correlates to the neural computations that neural population could implement. 

# Conclusions
