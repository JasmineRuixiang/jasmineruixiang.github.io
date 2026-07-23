---
layout: post
title: 'Neural Representation: A Rewinding Narrative (in progress)'
date: 2026-06-28 16:56:31
description: Neural representation, computation, dynamics
series: Computational Neuroscience/BCI Research
tags: 
    - "Representation"
    - "Learning"
    - "Neural Manifold"
    - "Dimensionality Reduction"
categories: 
    - "Neuroscience"
featured: false
related_posts: true
related_publications: true
citation: true
math: true
toc:
  beginning: true
  sidebar: left
---

This blog is a collection of thoughts on the concept of neural representation, a notion lonng proposed and debated, yet demonstrates a relentless trajectory of thoughts devoted to understanding neural computation and dynamics. In this blog I will share my thoughts revolving around certain key assumptions, concepts, and applications of this notion, based on various sources of research articles, reviews, and opinion papers I read and pondered upon through the past few years. 

---

### [1] What is a representation?
A representation, at first glance, is a way of expressing some internal/external variables/stimuli. Very naturally, it is further analyzed/utilized by downstream processes for other motion and cognition purposes. So already from this description we could extract a few key properties of representation: 1) It needs to capture some information about what it describes (ideally in a structural way); 2) It acuiqres an objective correct/incorrect standard; 3) we need to verify that it's actually employed (not merely some statistical patterns).  

Let me specify 2): The reason normativity isn't decorative and has to be stated as a condition is that it's the thing that separates representation from mere correlation/causation. A tree ring correlates with the year, a footprint with the foot, smoke with fire. None of these can be wrong. They covary and that's all. The moment we want to say a system represents X, rather than merely responds to X, we need it to be possible for the system to get X wrong while the machinery keeps running. Misrepresentation is the test that tells us there's content there to be incorrect about. This is standard (it's why Dretske, Millikan, Ramsey's "job description challenge" all foreground it (cite)). Consequently, structural relationship alone is just homomorphism/correlation, and that's possessed by tree rings and shadows. If we accept structure-only as sufficient, we've defined representation so broadly that it includes every reliable causal trace in nature, and we've lost the very thing that made positing representations explanatorily special.

#### [1.1] A logic sequence: correlation, causation, homomorphism, and representation

From the above definitions we could sense a potential urge or risk of confounding several common-sense notions: correlation, causation, and homorphism with representation. Let me make a clarification:

__Correlation__ is the weakest notion here indeed. Bare correlation is cheap, and any reliable covariation counts, and as we established, mere correlation can't even misrepresent in the right way without extra machinery.

Between __causality and representation__, it's not that representation is non-causal or more than causal in some spooky additive sense. The claim (via Chirimuuta, M. (2024). The brain abstracted: Simplification in the history and philosophy of neuroscience) is about explanatory purchase, not metaphysical surplus: positing a representation lets us explain behavior by reference to the distal relationship between organism and environment, rather than by tracing the full proximal causal chain.

Very concretely: we can in principle explain a rat solving a maze as a giant chain of synaptic events. This is pure causality, no representation talk at all. But that explanation doesn't make the pattern perspicuous: it doesn't tell us why this rat succeeds and that one fails, why a lesion here matters and there doesn't, what the rat is getting right or wrong about the world. Representation talk buys us a compression: it groups the indefinitely many causal micro-stories that all amount to "tracking the location of the goal" and lets us predict and explain at that level. The "beyond" is beyond the micro-causal description, not beyond causation as such.

However, one laziness about (Burnston and Ryan, 2026) is that they never __differentiate__ the encoding vs latent structure views on explanatory power/causality: does their own view deliver this explanatory purchase any better than the encoding view does? The encoding view also explains via distal relations (V1 "says edge," the message is about the world). If "beyond mere causality" is the prize, the encoding view already claims it. 

For __homomorphism__: in structural representation theory (Cummins, Swoyer, O'Brien & Opie, Shea's book which (Burnston and Ryan, 2026) cites, especially Swoyer (1991)) uses "homomorphism" to mean a structure-preserving mapping from relations among representational vehicles to relations among represented items. Notice that this is not an abstract algebra/group theoretic notion at all. Homomorphism (in the strong, structure-preserving sense) is expensive and impressive—it's the manifold-geometry-mirrors-task-geometry story that makes the view sound rigorous.

In (Burnston and Ryan, 2026), they briefly mentioned homomorphism (a homomorphism between relational structures is in some sense a precise object). However, the deficiency is that the precision is unusable until we fix three things the authors never fix and usually glossed over in many other texts:

(a) The signature problem: which relations get preserved? A homomorphism is only defined relative to a chosen set of relations. Change the relation set, change whether the map is a homomorphism. This is not a footnote. It's the whole game. Any two sufficiently rich structures admit some homomorphism between them under some relabeling of relations; Newman's objection (a classic problem for structuralism in philosophy of science) is precisely that "X is homomorphic to Y" is nearly vacuous unless we independently pin down which relations are the privileged ones. So when someone says a manifold's structure "mirrors" task structure, the rigorous question is: under which relations? Distance? Adjacency? Betweenness? Topology? And the answer matters enormously: a ring manifold for head direction preserves cyclic order and rough metric, but if we only demand order preservation we get a much weaker (and much cheaper) claim than if we demand metric-preservation. The term "homomorphism" hides this choice instead of forcing it. See (Shea 2014, Exploitable Isomorphism and Structural Representation) for more discussions. 

Indeed, (Shea 2014) acknowledged that founding an account of representational content on isomorphism, homomorphism or structural resemblance has proven elusive, largely because these relations are too liberal when the candidate structure over representational vehicles is unconstrained (again, this is Newman's problem, conceded by the principal). And in many cases where there is a clear isomorphism, it is not relied on in the way the representations are used. Shea's fix is to shift all the load-bearing rigor off the structural relation and onto use: "an isomorphism must be used, hence usable, if it is to be an ingredient in a theory of content". 

Also, BCI provides a causal confirmation of the usefulness aspect (e.g., Sadtler 2014). 

(b) Directionality and degeneracy: homomorphism is too weak in many scenario. Plain homomorphism is not injective. Many states can map to one world-item/stimulus, or worse for representation the structure can collapse. The constant map that sends everything to a single element is a homomorphism for many signatures. If all we require is "a structure-preserving map exists," we've required almost nothing, because trivial and degenerate maps qualify. What representation actually needs is something stronger and directional: roughly, that variation in the stimuli is recoverable from variation in the states (this sounds closer to an embedding or an isomorphism). "Homomorphism" names the weak thing and lets the readers supply the strong thing, which is a non-responsible rhetoric trick. 

(c) Exact vs. approximate: the math notion is brittle and the neural reality is graded. Algebraic and model-theoretic homomorphism is an all-or-nothing relation: the equation holds or it doesn't. But every interesting neural case is approximate: the manifold roughly mirrors task geometry, with noise, distortion, and various sources of variance. The moment we go to approximate structure-preservation, we've left the domain where the crisp definition lives, and we now need a metric on how good the mapping is, a tolerance, and a story about which distortions are representationally benign versus content-changing. None of that is supplied by the word "homomorphism." And the bite specific to (Burnston and Ryan, 2026) is that their five physiological properties (see below section 2, e.g., mixed selectivity, mixed variance) are arguments that the latent structure is systematically contaminated by non-task variation. That's an argument that the homomorphism is bad—lossy and low-fidelity. Consequently, we need an approximate, graded notion of structure-preservation far more than most structuralists do, and the authors invoke the exact term while their own evidence demands the inexact one.

In conclusion, using the word homomorphism in structural relations sense not in abstract algebra is fine, but the kill is that "homomorphism" is precise only once we specify a signature, a directionality/injectivity strength, and an exact-vs-approximate tolerance. The authors specify none of these, while their own empirical argument forces specific (and demanding) answers to all three. The word imports the connotation of rigor while the parameters that would make it rigorous are left blank. 

#### [1.2] Snapping back to the structural relationship vs exploitation

With the above discussions on the often mixed up notions, we could dive one layer deeper onto the definition itself. There's an inherent tention between the structural information (homomorphism) a representation carries vs the usage/exploitation by the downstream systems. 

And here's where the whole thread snaps back onto the (Burnston and Ryan, 2026)'s real vulnerability. If the rigor lives entirely in the use/exploitation condition, then whether (Burnston and Ryan, 2026) have a rigorous account at all depends entirely on whether their notion of use is as disciplined as Shea's. And recall what they do with Shea's use condition: they swap his "algorithmic/exploitation" reading (the downstream systems process the representation information from the last system as input) for their "organismic" notion of use, meaning that __the whole system uses the state, no downstream consumer required__. That is a substantive weakening of precisely the condition that was carrying all the rigor because Shea's exploitation is constrained (some downstream process must actually rely on the relation in producing the task function). "The organism as a whole uses it" is far looser.

Consequently, perhaps the correct, hard-won conclusion of this homomorphism talk is not about homomorphism at all: the rigor in Shea's account was deposited in the use condition, and (Burnston and Ryan, 2026) quietly withdrew it thereby replacing algorithmic exploitation (widely used in their so-called encoding view) with organismic use while leaving the structural relation language in place to look as though structure is doing the work. The pressure point isn't "define your homomorphism", but that "your inherited framework put all its rigor in use, and you adopted the one reading of use that spends it.". A helpful reference here: in the Mind & Language symposium on Shea's book, Gallistel raises questions about homomorphism and correlational information...

The actual discussion/support of algorithmic vs organismic usage will be in section [5]. But here I intend to point to one logic weakness of the paper. 


---

#### [1.x] A sidenote about neural information
Not necessarily in Shanon's entropy information sense (the Brain, in theory BrainInspired podcast). Also, how do we verify that it's actually used by the brain? 

Usually, from the information theory perspective, how are the research conducted? By finding out some correlations?

---

#### [1.x+1] A sidenote about correlation

Does correlation matrix capture the goemetric information? 



---

### [2] Representation as an encoding

The usual analogy we hear about is that a representation encodes the structural patterns of the external/internal stimuli, the environment, or the world around us. This is naturally an encoder/decoder or information-theoretic view. Very intuitive and explanatory, satisfying with the conditions in the definition of representation above (carry structures, normativity, used by downstream processes (as decoders)). What might potentially go wrong? 





---

### [3] Neural properties that muddle a clear representation, or do they?

It has been claimed that "in general, neural activities do not correspond to one particular environmental or experimental variable. They show mixed selectivity and mixed variance; they are multiplexed and context-sensitive."(Burnston and Ryan, 2026) Could we then conclude that there's no specific information retained in the neural activities? That would be so strong a claim. 

But why would this directly refute encoding models, favoring a latent structure framework? 

(Burnston and Ryan, 2026) propose that "representations are due to the interaction of the latent structure of a brain system with its environment." 


---


### [4] Latent world under the surface: dynamics and "manifold"


#### [4.x] What does latent represent?

Since a representation has many facets and easily confused with many other notions (see [1.1]), we need to state very clearly what representations describe. For example, many times bundling correlation with structural correspondence/homomorphism under one label, "structural relationship," allows interpreting and framing it both ways: invoke the expensive thing to sound like they've got real structure-preservation, then retreat to the cheap thing if anyone demands they actually demonstrate it. So crucially,  which one neural latent structures are supposed to instantiate, or what hangs on the difference. Such undefended slide between the strong and weak readings bother me many times, a little like motte and bailey: The "bailey" is the valuable, exposed claim thay actually want (rich structure preservation). The "motte" is the cheap, easily defended fallback (mere correlation). They advance the bailey when unchallenged and scurry back to the motte when attacked, then re-emerge as if the motte vindicated the bailey. 

But on the other hand, such disjunction is to some extent intentionally made, since the correlation-or-structural-correspondence disjunction is inherited verbatim from Shea, where keeping the structural relation liberal is a deliberate design choice and the rigor is offloaded onto the use/exploitation condition, as we covered in [1.2].

Also, from [1.2], (Burnston and Ryan, 2026) make a specific empirical bid that latent structures mirror task structure, which is reaching for the strong homomorphism end, the bailey. But their entire Section 4 (mixed selectivity, mixed variance, "private variance") is an argument that the states are contaminated, which pushes the actual neural case toward the weak, corrupted correlation end, the motte. So one live charge we could immediately bring out is no necessarily that "you bundled two things illegitimately" but that "you oscillate across Shea's legitimate disjunction opportunistically, a sort of rhetorical weight on geometric mirroring, but only evidential retreat to mere (and degraded) correlation when your own physiology bites." Notice it points at the same seam as everything else we've found earlier: the work (Burnston and Ryan, 2026) is supposed to be done by structure, but the structure can't bear it, so it gets quietly done (or not done) by use. So there is the strong/weak slide, but it's not the bundling of concepts together under the name representation rather than Burnston and Ryan, 2026's traversal of an inherited bundle.

So, does the latent structure actually describe correlation? Or really some geometric structure? 



---


### [5] Representation: encoding or latent

Now, with the above discussions we could probably discuss with more bases on whether representation is actually an encoding scheme or a latent structure together with context-dependent mixture (as Burnston and Ryan, 2026 claim).

#### [5.1] Algorithmic vs organismic usage

On p.12: "this does not require that some downstream behavioral system reads the relevant variables from the representation. Simply constraining behavior or providing a signal that biases a behavioral-motor decision is sufficient." That's the no-reader condition with teeth. And it does eventually do dialectical work: thei whole section 4.2 of Burnston and Ryan, 2026 attacks on encoding turns on the decoder/consumer needing illicit contextual knowledge, and the organismic view dodges that by denying there's a knowing consumer at all. 

Summary of support from [3]. 

However, the deeper potential objection I have for that is that: Grant them the differentia. Is "whole-system use" vs "downstream consumer" a difference in mechanism or only in description? Their own cashout is "a signal that biases a behavioral motor decision." But a signal biasing downstream motor circuitry just is a signal causally influencing a downstream structure, which is what "a consumer receiving an input" denotes. The boundary between "the organism uses the state to bias behavior" and "the motor system consumes the state as input" looks like a choice of grain, not a fact about the wiring.

[Elaborate more here]


---

#### [5.2] Normativity and misrepresentation

In the beginning passages of [1] we already established that stripping normativity out and there's no answer to "why is this representation and not just causal flow?". However, when Burnston and Ryan, 2026 approach the differences of encoding vs latent view of representation, there are a few logic fallacies and confusions that bite on their statements. 

On the encoding view, misrepresentation is just a tokening that occurs outside the conditions that fix its content (For example, you recognized person A as B under dim light). On the latent view, their misrepresentation story (p.12 of Burnston and Ryan, 2026) has two modes: (i) the system mislocates the stimulus value within the structure, or (ii) the system fails to deploy the right subspace for the context. Now look at mode (i). "The activation landed at the wrong location on the axis" is the very same event the encoding theorist calls "the wrong message was tokened." Relabeling the state space as a manifold rather than a message doesn't change the normative fact, and both say this: the world is X, the system's state corresponds to not X, and behavior fails accordingly. So for the core notion of misrepresentation-as-mismatch, the accounts are simply just notational variants. The latent view inherits exactly the same normativity, because normativity attaches to the correspondence-and-its-failure, and both posit a correspondence whose failure is misrepresentation.

In fact we can sharpen this into a charge: their whole paper grants that latent structure still gives us a structural correspondence between activity locations and world values. Normativity rides on that correspondence. So of course it's the same on both views: they kept the thing normativity attaches to. To claim a different notion of normativity, they'd need to have changed what representation fundamentally is, and they didn't; they changed where the structure lives (learned connectivity vs. an encoded message), not whether there's a structure to be right or wrong about.

To be fair though, mode (ii)—wrong-subspace—is not available to the encoding view in the same form, and the authors clearly think this is their normativity payoff (it's why the engram cases are all context-misrepresentation: same fear engram, freezing in the small box is "right," rearing in the big box is the system having deployed structure inappropriate to context). On the strict encoding picture, each unit just emits its message in response to its input; there is no further fact of "the system selected the wrong interpretive frame for an otherwise-correctly-tokened state." Encoding normativity is one-level, just message matches world or doesn't. Latent normativity is, they want to claim, two-level: you can get the local value right but the context-indexing wrong.
Consequently, for first-order mismatch (mode i), normativity is identical and they shouldn't pretend otherwise. The genuine candidate-difference is mode (ii), context-misrepresentation.

However, mode (ii) is exactly where the earlier threads come back to bite them. Two problems, both ones we already built:

First, recall in their section 4.2's deepest objection against encoding: that smuggling "the receiver knows which inputs to attend to, given context" into a subcomponent is illicit, because homuncular decomposition is supposed to explain context-sensitive knowledge, not presuppose it. But mode (ii) misrepresentation requires the system to select the appropriate subspace for the context. That selection is precisely a piece of context-recognition. So they've reintroduced, at the system level, the very context-sensitive competence they called illicit when the encoding theorist placed it in a subcomponent. But they could claim that "ours is organismic, not a knowing homunculus", and now we're back to the earlier problem : is "the system deploys the right subspace" a different mechanism from "a downstream selector gates the right channel," or only a different grain of description? If it's only description, then mode ii normativity is encoding normativity wearing a manifold costume, directly echoing [5.1].

Second—this is the back-door re-entry your project has been tracking—their positive normativity now quietly depends on context-recognition as a normatively-assessable function. They said early (p.2) that normativity is one of the two ingredients, then spend the paper acting as though structure does the work. But mode (ii) reveals normativity sitting on a function ("recognize the context, deploy the matching subspace") they never gave content-conditions for. What makes a given subspace the appropriate one for a context? On Shea's framework the answer would have to come from the use/exploitation condition—and we've established they've swapped exploitation for the looser "organismic use." So the appropriateness standard that mode-(ii) normativity needs is underwritten by the one condition they weakened. The normativity doesn't differ from encoding in kind; where it does reach beyond encoding (context-selection), it's resting on an ungrounded notion of "appropriate subspace."

---

#### [5.3] Context and task dependence, one more time

Now perhaps we have more in-depth understanding of task dependence.

For the encoding view, it's usually interpreted that each subcomponent along the chain of processing lines only take inputs and exports functionally specific outputs as its representation for the next subsystem. By this breaking down of complex cognitive processes, each compoent does not necessarily need to "understand/interpret" the ultimate task goal or context. 


---

### [6] Representation and behavior

The authors of (Krakauer, Ghazanfar, Gomez-Marin, MacIver & Poeppel's 2017 Neuron perspective, "Neuroscience Needs Behavior: Correcting a Reductionist Bias.")  push back against a tacit reductionist program that has come to dominate neuroscience — the belief that if you get powerful enough tools (optogenetics, connectomics, genetic targeting, large-scale recording) and establish causal necessity/sufficiency relationships between circuits and behavior, understanding will follow. Their claim: causal-mechanistic explanation at the neural level is not sufficient to explain how the brain generates behavior. What's needed alongside it is careful theoretical and experimental decomposition of behavior itself. Their slogan for the division of labor: behavioral work provides understanding, neural interventions test causality, and behavioral work is epistemologically prior.

#### [6.1] The key problem: from correlation and causality to explanation

Why is causality intervention not enough? Well, two structural problems undercut the "just manipulate the circuit" view. First, degeneracy and multiple realizability: the same behavior can arise from different circuits, and the same circuit can produce different behaviors, so inspecting lower level properties alone can't fix the mapping to behavior. Second, we usually don't even know which level of organization is the relevant one for a given behavior. They lean on Marr's three levels (computational/"why," algorithmic/"what," implementation/"how") and his metaphor that studying neurons to understand perception is like studying feathers to understand flight: you can't infer the algorithm from the hardware nearly as well as you can from analyzing the problem being solved.

The key problem: 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/neural_representation/krakauer_2017_fig1.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="70%" %}
    </div>
</div>
<div class="caption">
    Figure 1 from  (Krakauer, Ghazanfar, Gomez-Marin, MacIver & Poeppel's 2017 Neuron perspective) paper: Complex mapping relations between neural activities and behaviors
</div>


One interesting question they mentioned in the paper is that we are trying to understand from the implementation of processes (the processors, i.e., the neural systmes, hardware, or the "how") to infer the underlying processing principle (neural computation), which has been muddling and far from easy to solve. One simple analogy on feathers:

Consequently, Krakauer et al. proposed to carefully include (apriori) behavior into the experimental framework. 

In front of an evolving trend of neural technologies (that enable neural population recordings, or perturbations methods such as optogenetics), large-scale neural data and the modeling methods that try to describe them may not ncessarily euqal a conceptual framework for explanation and interpretation. Here Krakauer proposes to go beyond correlation and causation, for a more pluralistic narrative of brain-behavior relationships. And this specifically requires more detailed and carefully planned exploration, stipulation, and control of behaivors in experiments. 

Conceptual machinery that (Krakauer, Ghazanfar, Gomez-Marin, MacIver & Poeppel's 2017 Neuron perspective) invoke: 1) A few recurring ideas: the _substitution bias_ (Kahneman): technique-driven neuroscience quietly swaps a hard question for an easier one. 2) Filler terms:  verbs like "mediates," "underlies," "encodes," "modulates" that dress up a bare correlation or causal claim as if it were an explanation. 3) The mereological fallacy which ascribes to individual neurons psychological properties that only belong to the whole organism, with the mirror-neuron "action understanding" literature as the example (neurons get credited with "understanding" though no behavioral test of understanding was run). 4) Finally emergence / downward causation, which points at components doing different things than the organized whole, so causal claims often only make sense across levels simultaneously (an anaology they draw is that ion channels don't beat, but heart cells do).


---

#### [6.2] The presciption and David Marr's 3 levels
The solution that (Krakauer, Ghazanfar, Gomez-Marin, MacIver & Poeppel's 2017 Neuron perspective) gave is what they call a more __pluralistic neuroscience__: multiple legitimate modes of explanation, not the assumption that implementation level description will "naively emerge" into algorithmic understanding. Well-designed, ethologically grounded behavioral experiments (they lean on Tinbergen here: behavior as an evolved entity, VR as a tool only usable once you understand the natural behavior) can stand alone and must generally come first, so that neural experiments are designed against real algorithmic hypotheses rather than fishing in high-dimensional data.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/neural_representation/krakauer_2017_fig1.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="70%" %}
    </div>
</div>
<div class="caption">
    Figure 2 from  (Krakauer, Ghazanfar, Gomez-Marin, MacIver & Poeppel's 2017 Neuron perspective) paper:
    David Marr's 3 levels of understanding of complex systems:
</div>

Explanation of Marr's 3-level analysis should be self-explanatory from the above figure. 

This 3 level analysis points at a sharp fact, which Krakauer et al. repetitively emphasize throughout: being able to describe or causally perturb sometihng does not amount to an explanation or understanding. As quoted from Marr (1982/2010 Vision): "...trying to understnad perception by understanding neurons is like trying to understand a bird's flight by studying only feathers. It just cannot be done". This is a very strong standpoint, and I do think there's much degree of freedom open to debate. For example, you could say that a proper analogy migt be the end effectors (muscles and glands, for example) instead of neurons themselves. However, this does not obfuscate the critical point that striving to understand the underlying principles/patterns (level 2, what/rules) from the implementation (level 3, how/physical) is much more difficult than from the fundamental computation perspective (level 1, why/problem). 

Consequently, the key issue is that from the implementation level how much of details should we neglect and collapse? This also relates to the perhaps broader question on what is life and mind. Carbon-based materials might not be the necessary condition, the same reasons that neurons as physiological functioning units might also not be necessarily required. You could say that the word implementation contains, on its own, many levels of conditions: materials, hierarchies, compositions, etc., some of which might be redundant for the specific conditions we are referring to. For example, Krakauer gave out the example of chess: "Understanding the game does not depend on knowing anything about the material out of which the board or chesss pieces are made". For the problem of neural computation that we care about, the materials does not matter for sure. However, this reinforces the idea that the word "implementation" smears over many different subcategories which are only collapsable under different given contexts. 

---

#### [6.3] The "intrinsic" nature of neural patterns

Giusti's clique topology paper emphasizes the possibility of using clique topology to extract stable neural features intirnsically (irrespective of the stimuli/task structure), opposite view of Krakauer 2017 Perspective above. 

The Giusti's paper wonders that without knowing the neural coding properties (tuning curves and receptive field, etc. like the place field), would we still be able to extract neural patterns intrinsically. 

But first of all, let's think through this: how would knowing the coding properties help?

If we know the coding properties, we would know the map from stimulus to firing rate for each cell, and therefore we can read off each neuron's preferred stimulus value (its place field center, for example). This does two things:

1) It hands us the represented stimulus space and a coordinate for every neuron. Geometric organization is then not something we have to infer — we can just embed the N neurons as N points in some vector space and confirm directly that they live on some low-dimensional structure.

2) It gives us the falloff relationship explicitly. For example, we know that place cells "have correlations that decrease with distance between represented ... locations" and that this "is easy to see by correlating neural responses directly to the relevant stimuli" (Giusti). So with the labels in hand, detecting geometry reduces to a supervised check: correlate responses against the known stimulus and recover each tuning curve.

Without knowledge of coding properties we have only the correlation matrix, and there's no stimulus labels, no field centers, no idea which variable is even being represented. So a priori it's unclear if we can recover the geometry from correlations alone. Their claim (Giusti clique topology paper, restated in the Discussion, page 5) is that geometric organization "can be detected from pairwise correlations alone, without any a priori knowledge about the nature of receptive fields." Of course knowing the coding properties would let us assign each neuron a coordinate in the represented space and verify the manifold directly perhaps, but Giusti argues for doing it unlabeled, from the correlation ordering alone, and invariant to an unknown monotone nonlinear function.

An interesting finding from the Giusti's 2015 clique topology paper is that: 

The PF/scrambled-PF experiment establishes that place-field geometry is *sufficient* to reproduce the geometric Betti signature and that bare common global drive is not (scrambled fields push $\bar{\beta}_2, \bar{\beta}_3$ out of the geometric regime), and (within navigation tasks, relative to the global-drive-only null) that the spatial coherence of the fields is *necessary*; but this necessity is scoped not absolute, and both models are synthetic Poisson processes driven only by trajectory, so the intrinsic dynamics source is deleted by construction before the comparison begins. That deletion matters because navigation geometry is *over-determined*: the nonspatial results (wheel, REM) later prove intrinsic network geometry exists, and since the network doesn't stop being itself during navigation, both the static spatial map and the intrinsic dynamics are simultaneously present in real navigation spike trains. Consequently, the experiment cannot show the signature is *caused by* the map rather than the dynamics, and the honest claim is "place-field geometry suffices to reproduce, and dominates at fine timescales," not "navigation geometry reflects place-field geometry." 

More interestingly, the confound sharpens once we notice $F_i(x)$ is itself estimated from navigation data already containing intrinsic dynamics (theta/assembly sequences are spatially organized), so the field is partly the time-averaged spatial *footprint* of those dynamics; permuting it destroys all spatially coherent structure regardless of origin (the intrinsic spatial shadow, not the intrinsic dynamics which were never in the model). What genuinely dissociates the two sources is the fine-timescale result (Fig. S12–13): navigation geometry persists at fine $\tau$ while wheel/REM geometry deteriorates, because position changes slowly and spatial correlations survive short windows whereas the intrinsic component does not. Consequently, the spatial contribution is real and timescale robust, but at $1$ s both components are live and the origin claim stays underdetermined. 

Something to notice is that although Giusti paper demonstrated the intrinsicality of neural patterns of place cells, it depends on the timescale they pick. Finer timescales break the neural patterns which render it different from when actually during the task. 

---

#### [6.4] "Reflection" of task/stimuli structure? A three layer dissection

When would the neural activities reflect the task/stimuli structure? In what form? 

__Is it a faithful representation? Do we need to look at the high dimensionla neuron space? Is the so-called neural manifold the correct to think about? Is brain actually using that for doing computation?__ 

In Giusti's paper they claim that intrinc geometry patterns could be recovered from neural data alone using their clique topology method. 

There's actually 3 levels of "geometry" (I actually am very against using this word "geometry", the same as "manifold", as it provides no further clarity while specifying no further mathematical details): 1] task/stimuli structure; 2] neural state (high dimensions or latent space) patterns; 3] Correlation matrix structure (clique topology, SPD matrices). 

Let's start with level 3], perhaps the easiest one: 

What the clique topology method actually targets is most cleanlyt stated in the Supplementary Text (p. 16), where they frame the whole enterprise as two questions: 1) Is the matrix a monotonic transformation of a random or a geometric matrix? 2) Can we distinguish these without knowing a potential monotonic nonlinear function applied to it? Consequently, the output of clique topology is a _model classification_, not a __reconstruction__. It tells us which null our matrix's order complex statistically resembles. It does not hand us the represented "manifold", its coordinates, or even (reliably) its dimension. The authors are explicit that "the precise dimension is sensitive to noise and currently difficult to estimate" (p. 3), so what we get intrinsically from $C_{ij}$ is not the task geometry.

Notice that in the paper when Giusti mentioned geometric matrices, the authors are using that in a specific sense: that the order complex of $C_{ij}$ is statistically consistent with being a monotone transform of a negative Euclidean distance matrix of $N$ points sampled in some $\mathbb{R}^d$. That is a much weaker certificate than saying that hte neurons encodes the stimuli (like the 2D environment box). 

However, Giusti et.al. demonstrated that wheel running and REM sleep also show the geometric signature during REM sleep where there is no task/stimulus geometry at all. What clique topology detects cannot be the task geometry: the authors read this as a property of the intrinsic hippocampal circuit ("not merely a byproduct of spatially structured inputs"). The geometric signature and the task geometry coincide during tasks like navigation, but the method is certifying "Euclidean-like organization," not just the task.

To be fair, such method does capture task geometry in the place field example. During navigation, place fields tile a 2D box and correlation falls off with distance between field centers (Giusti's ref. 3), so $C_{ij} \approx f(||c_i − c_j||)$ with the $c_i$ being positions in the 2D box. The authors showed that the PF / scrambled-PF models isolate this: the spatial coherence of the fields is what produces the geometric signature, and destroying it kills $\bar{\beta}_2$, $\bar{\beta}_3$. Consequently, the source of the structure is (position-coding) task geometry. 

In summary, the geometry the neurons represent (during navigation) is the task geometry, but what $C_{ij}$ yields intrinsically is a dimension-agnostic statistical certificate of Euclidean consistency, distinguishable from both randomness and certain nongeometric structure — but not the task manifold itself.

Some other examples on how neural patterns reflect task geometry: For example, orientation-tuned neurons (Hubel DH, Wiesel TN (1959) Receptive fields of single neurones in the cat’s striate cortex. J Physiol 148:574–591.) and hippocampal place cells (O’Keefe J, Dostrovsky J (1971) The hippocampus as a spatial map. Brain Res 34(1):171–175.) demonstrate correlations which decrease as the corresponding represented variables increase (orientation angles or position locations). How to detect this? This loops back to [6.1], and a simple strategy is just to measure the correlation between neural activities and the corresponding task stimuli.

Let me be explicit and precise on the place cells example:

1. Place cells have spatially localized receptive fields. A place cell fires when the animal's position is inside its place field, i.e. its preferred region of the environment, so each neuron is effectively tagged with a location as its field center or simply a point in a let's say 2D box.

2. Correlation falls off monotonically with distance between field centers. Two cells whose fields are close together co-fire often (the animal passes through both nearly together); cells with distant fields rarely co-fire. In formula we could write $C_{ij} \approx f(||c_i − c_j||)$ with $c_i$ the field centers and $f$ monotone decreasing.

3. That is exactly the definition of a geometric matrix. Giusti's geometric matrix is $C_{ij} = f(−||p_i − p_j||)$: a monotone function of negative Euclidean distances among points in $\mathbb{R}^d$. And because clique topology is invariant under the monotone f, we don't even need to know $f$ because the ordering is what matters.

Notice that here there's no claim or employment of somatotopy (neurons close in anatomical distance share larger covariance). 

To be fair, the whole worry motivating the paper is whether we can detect geometric organization intrinsically, from correlations alone, without using the stimulus labels. Place cells during navigation are the one case where we already know the answer must be "yes, geometric," from decades of receptive-field work. So if the label-free method returned "random" or "nongeometric" here, the clique topology method would be broken. Getting the geometric signature confirms the tool detects what it's supposed to, on ground truth. They actually say as much in the setup: geometric structure "is expected due to the existence of spatially localized receptive fields... but has not previously been detected intrinsically using only the pattern of correlations" (p. 3, Giusti).


---

#### [6.5] Euclidean vs Non-euclidean geometry

Giusti's clique topology originally worked with the geometric matrices obtained from uniform sampling from a Euclidean unit ball. Howeve, notice that this method cannot distinguish Euclidean vs non-Euclidean method, because it only relies on the metric property (triangle inequality). 

To be precise, the triangle inequality is a metric axiom, not a Euclidean one. It holds in hyperbolic space, on any Riemannian manifold, in any metric space. So the mechanism in the paper,  near transitivity coupling the edge ordering, cliques filling holes, cycles dying young, fires for any metric geometry where correlation decreases with distance. Therefore the geometric-vs-random detection still works for hyperbolic or other non-Euclidean data. Clique topology would flag hyperbolic neural data as "geometric," not "random."

Of course there're many genuine metric geometries that aren't Euclidean: hyperbolic, spherical, geodesic distance on any Riemannian manifold. These still obey the triangle inequality (it's a theorem for geodesic distance, not a Euclidean special feature). So they don't give us a triangle-violating counterexample (there're two notions of metric coing into play at the same time and that's why it appears confusing).

What is Euclidean-specific is the reference distribution, not the tool. Giusti's "geometric" null is narrowly defined: $N$ points sampled uniformly from the unit cube $[0,1]^d \subset R^d$, $C_{ij} = -||p_i−p_j||$ with the Euclidean norm. The specific Betti curve shapes and the dimension stratification they report are properties of that sampling family. Hyperbolic space has exponential volume growth rather than polynomial, so uniform sampling there produces quantitatively different distance orderings and hence different Betti curves. So:

* Out of the box, Giusti cannot distinguish Euclidean from hyperbolic — both sit in the broad "geometric" bucket, well below random.
* To characterize which geometry, we would swap the forward model: sample from $\mathbb{H}^d$ (with a chosen curvature), push through the identical clique-topology and Betti-curve pipeline, and compare the data's Betti curves against a family of geometric nulls rather than the single Euclidean one.

That swap is exactly ZhouZhang (Sharpee) 2023's extension (clique topology + hyperbolic forward model for Betti-curve model-comparison). Giusti gives the invariant readout and the Euclidean anchor; the non-Euclidean question is answered by adding candidate-geometry forward models on top of the same persistent-homology substrate. 




---

### [7] The world model and generation



