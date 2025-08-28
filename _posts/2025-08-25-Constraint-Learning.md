---
layout: post
title: Learning and constraints (in progress)
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


# Neural constraints on learning (Sadtler et.al. 2014)
## Experiment setup
For {% cite Sadtler2014 %}, two male Rhesus macaques were trained to perform closed-loop BCI cursor task (Radial 8). Around 85-91 neural units (threshold-crossings) were recorded. The experiment pipeline is demonstrated below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Fig_1a.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="70%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig. 1a in {% cite Sadtler2014 %}. Note that on the right hand side the authors already presented where to place the two kinds of perturbation. The color scheme (green, yellow, and black) is consistent throughout figures in this paper.
</div>


## Decoding paradigm
### Dimensionality reduction technique
The control space is just 2D because the decoder output is cursor velocities in $$\mathbb{R}^2$$, illustrated as black line in Fig.2 black line (Note: it's actually a 2D plane, but here for simplicity shown as a black line ($$\mathbb{R}^1$$)). 

They used Factor Analysis ({cite (Factor-analysis methods for higher-performance neural prostheses) }{cite (Gaussian Process Factor analysis)}; I'll write a blog on GPFA later) to extract what they called the "intrinsic manifold", which captures the co-modulation patterns among the recorded neural population. This is shown as the underlying yellow plane in Fig.2 (might be confusing, but it's not the 2D control space). Note that at the time of publication, neural manifold was not yet in a popular trend, so the authors briefly characterized the term "intrinsic manifold" with the following illustration:

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

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Fig_4.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="70%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig.4 in {% cite Sadtler2014 %}.
</div>

The factor analysis method works in the following way (I'll keep the same notation as the paper). Let's assume the high dimensional neural signal (here the z-scored spike counts) acquired every 45ms time bin is denoted as $$u \in \mathbb{R}^{q \times 1}$$ (naturally, $$q$$ neural units), and $$z \in \mathbb{R}^{10}$$ the latent variable. Factor analysis assumes the observed neural activity is related to the unobservable latent variables under a Gaussian distribution:

$$
\begin{equation}
u \mid z \sim N(\Lambda z + \mu, \psi)
\end{equation}
$$

where the latent vector is assumed to come from 

$$
\begin{equation}
z \sim N(0, I)
\end{equation}
$$

Here the covariance matrix $$\psi$$ is diagonal. Consequently, the intrinsic manifold is defined on the span of the columns of $$\Lambda$$, and each column of $$\Lambda$$ represents a latent dimension where $$z$$ encodes the corresponding projections/coordinates. All three parameters $$\Lambda, \mu, \psi$$ are estimated from __Expectation-Maximization (EM)__ method (I'll also write a blog about this later, especially how it as a classical inferenc engine is closely related to Evidence Lower Bound (__ELBO__), a populat loss/objective function for modern-day generative models based on DNN like VAE and Diffusion). 


### Intuitive mapping
The intuitive mapping is selected by fitting a modified Kalman Filter ({cite Wu W. Gao Y., Bayesian population decoding of motor cortical activity using a Kalman filter}). Specifically, for each __z-scored__ spike count $$z_t$$, after obtaining the posterior mean $$\hat{z}_{t} = E[z_t \mid u_t]$$ and __z-scoring__ each dimension (these z-scorings are important, which will be stressed a multiple times later), the authors started with the common linear dynamical system (LDS) assumption of Kalman Filter:

$$
\begin{align}
x_t \mid x_{t-1} &\sim N(Ax_{t-1} + b, Q) \\
\hat{z}_t \mid x_t &\sim N(Cx_t + d, R)
\end{align}
$$

The parameters $$A,b,Q,C,d,R$$ are obtained by maximum likelihood estimation, where $$x_t$$ is the estimate of monkey's intended velocity (label for the data). Since the spike counts and the latent factors were both __z-scored__ and the calibration kinematics were centered, $$\mu = d = b = 0$$. 

Consequently, by filtering the goal is to estimate $$\hat{x}_{t} = E[x_t \mid \hat{z}_{1}, \;, ... \;, \hat{z}_{t}]$$. The authors directly gave out the formula below to express $$\hat{x}_t$$ in terms of the final decoded velocity at the previous step $$\hat{x}_{t-1}$$ and the current z-scored spike count $$u_{t}$$: 

$$
\begin{equation}
\hat{x}_t = M_1 \hat{x}_{t-1} + M_2 u_t
\end{equation}
$$

$$
\begin{equation}
M_1 = A - KCA
\end{equation}
$$

$$
\begin{equation}
M_2 = K\Sigma_{z}\beta
\end{equation}
$$

$$
\begin{equation}
\beta = \Lambda^T(\Lambda \Lambda^T + \Psi)^{-1}
\end{equation}
$$

where $$K$$ is the steady-state Kalman gain matrix. As part of the process of z-scoring the latent factors, $$\Sigma_z$$ is a __diagonal__ matrix whose diagonal element ($$p, p$$) refers to the inverse of standard deviation of the $$pth$$ factor. Since both spike counts and latent factors are __z-scored__, the perturbed mappings (see in the next section) "would not require a neural unit to fire outside of its observed spike count range".

The above formula might sound confusing, so I present below a detailed derivation. It's not so complicated but readers who are not interested in derivation feel free to skip it. 

#### Derivation of the iterative filtering equation
I'll derive the above formula (5 - 8) in the following 3 steps. In the end, this is nothing brand new and elusive. 

> Step 1: Obtain the posterior of $$z \mid u$$
>
> Step 2: z-score the latents
>
> Step 3: Apply Kalman filter
>

##### Step 1: Linear Gaussian system 
I'll start with a well-known fact about linear Gaussian system (this derivation is also the core of Gaussian Process and Kalman Filter; Stop for a second and marvel again at the all-encompassing power of Gaussian distribution). Assume two random vectors $$z \in \mathbb{R}^m$$ and $$x \in \mathbb{R}^n$$ which follow the Gaussian distribution:

$$
\begin{equation}
p(z) = N(z \mid \mu_z, \Sigma_z)
\end{equation}
$$

$$
\begin{equation}
p(x \mid z) = N(x \mid Az + b, \Omega)
\end{equation}
$$

The above illustrates a __linear Gaussian system__. Note that $$A \in \mathbb{R}^{n \times m}$$. Consequently, the correponsding joint distribution $$p(z, x) = p(z)p(x \mid z)$$ is also a Gaussian with an $$(m + n)$$ dimensional random vector:

$$
\begin{equation}
p(z, x) = N(
\begin{bmatrix}
z \\
x
\end{bmatrix} \mid
\tilde{\mu}, \tilde{\Sigma})
\end{equation}
$$

where

$$
\begin{align}

\tilde{\mu} &= 
\begin{bmatrix}
\mu_z \\
A\mu_z + b
\end{bmatrix} \\

\tilde{\Sigma} &= 
\begin{bmatrix}
\Sigma_z & \Sigma_z A^T \\
A\Sigma_z & A\Sigma_z A^T + \Omega
\end{bmatrix}

\end{align}
$$


The above could be easily derived from matching the corresponding moments, so I will not show in full details. From this joint Gaussian, we could thus easily continue to write out the posterior distribution:

$$
\begin{equation}
p(z \mid x) = N(z \mid \mu', \Sigma')
\end{equation}
$$

where

$$
\begin{equation}
\mu' = \mu_z + \Sigma_z A^T(A\Sigma_z A^T + \Omega)^{-1}(x - (A \mu_z + b))
\end{equation}
$$

$$
\begin{equation}
\Sigma' = \Sigma_z - \Sigma_z A^T(A\Sigma_z A^T + \Omega)^{-1}A\Sigma_z
\end{equation}
$$

The above posterior is known as __Bayes' rule for Gaussians__. It states that if both the prior $$p(z)$$ and the likelihood $$p(x \mid z)$$ are Gaussian, so is the posterior $$p(z \mid x)$$ (equivalently, Gaussian prior is a __conjugate prior__ of Gaussian likelihood or Gaussians are __closed under updating__, {% cite pml2Book %} P29).  One interesting fact is that although the posterior mean is a linear function of $$x$$, the posterior covariance is entirely independent of $$x$$. This is a peculiar property of Gaussian distribution (Interested readers please see more explanations in {% cite pml2Book %} sections 2.3.1.3, 2.3.2.1-2, and 8.2). Finally, keen readers might already perceive the equation (15,16) prelude the form of the Kalman Filter posterior update equations. 

From the above posterior Gaussian form, by plugging in the notations specified in (1-2) with $$z = z_t, \; x = u_t, \; \mu_z = 0, \; \Sigma_z = I \;, A = \Lambda, \; b = \mu \;, \Omega = \Psi$$ we obtain the following:

$$
\begin{equation}
p(z_t \mid u_t) = N(z_t \mid \mu_{post}, \Sigma_{post})
\end{equation}
$$

where

$$
\begin{align}
\mu_{post} &= 0 + I \Lambda^T(\Psi + \Lambda I \Lambda^T)^{-1}(u_t - (\Lambda 0 + \mu)) \\
&= \Lambda^T(\Psi + \Lambda \Lambda^T)^{-1}u_t
\end{align}
$$

and 

$$
\begin{align}
\Sigma_{post} &= I - I\Lambda^T(\Psi + \Lambda I \Lambda^T)^{-1}\Lambda I \\
\Sigma_{post} &= I - \Lambda^T(\Psi + \Lambda \Lambda^T)^{-1}\Lambda
\end{align}
$$

I deliberately used a different set of notations for the posterior linear Gaussian system, so if you are interested please do this little derivation on your own. Since $$\hat{z}_t = E[z_t \mid u_t]$$, from above we know that 

$$
\begin{equation}
\hat{z}_t = \Lambda(\Psi + \Lambda \Lambda^T)^{-1}u_t
\end{equation}
$$

##### Step 2: Perform z-scoring
The second step is to z-score the posterior mean $$\hat{z}_t$$, which, here, is dividing each position of this vector by the corresponding standard deviation:

$$
\begin{equation}
\hat{z}_{t, z-scored} = 
\begin{bmatrix}
\hat{z}_{t}^{1} / \sigma_{1} \\
\hat{z}_{t}^{2} / \sigma_{2} \\
\vdots \\
\hat{z}_{t}^{p} / \sigma_{p}
\end{bmatrix} = 

\begin{bmatrix}
\frac{1}{\sigma_1} & 0 & 0 &\cdots & 0 \\
0 & \frac{1}{\sigma_2} & 0 & \cdots & 0 \\
\vdots & & \ddots & & \vdots \\
0 & 0 & 0 & \cdots & \frac{1}{\sigma_{p}}
\end{bmatrix} \hat{z}_t = 
\Sigma_z \hat{z}_t

\end{equation}
$$

For simplicity, for the below I'll replace $$\hat{z}_{t, z-scored}$$ with $$\hat{z}_t$$.

##### Step 3: Apply Kalman Filter
As in Step 1, let me right down the filtering equation for a pure Kalman Filter based on the Gaussian linear assumption made in (9-10) (For the following I'll drop off $$b, d$$ since they are 0). The notations below might be a little messy, but I want to keep it rigorous. The hat on $$x, P$$ refer to estimates not ground truth, and $$t \mid t-1$$ indicates prior estimation while $$t \mid t$$ indicates posterior estimation at time point $$t$$.

$$
\begin{align}
\hat{x}_{t|t-1} &= A\hat{x}_{t-1|t-1} \\
\hat{P}_{t|t-1} &= A\hat{P}_{t-1|t-1}A^T + Q \\
K_t &= \hat{P}_{t|t-1}C^T(C\hat{P}_{t|t-1}C^T + R)^{-1}\\
\hat{x}_{t|t} &= \hat{x}_{t|t-1} + K_t(\hat{z}_{t} - C\hat{x}_{t|t-1}) \\
\hat{P}_{t|t} &= (I - K_tC)\hat{P}_{t|t-1}
\end{align}
$$

Again, we play the trick of substitution, starting with (27):

$$
\begin{align}
\hat{x}_{t\mid t} &= \hat{x}_{t \mid t-1} + K_t(\hat{z}_{t} - C\hat{x}_{t \mid t-1})  \\
&= A\hat{x}_{t-1 \mid t-1} + K_t(\hat{z}_t - CA\hat{x}_{t-1 \mid t-1}) \\
&= (A - K_tCA)\hat{x}_{t-1 \mid t-1} + K_t \hat{z}_t \\
&= (A - K_tCA)\hat{x}_{t-1 \mid t-1} + K_t\Sigma_z( \Lambda^T(\Psi + \Lambda \Lambda^T)^{-1}u_t)  \\
&= (A - K_tCA)\hat{x}_{t-1 \mid t-1} + K_t\Sigma_z \Lambda^T(\Psi + \Lambda \Lambda^T)^{-1}u_t 
\end{align}
$$

Finally, if we define $$M_1 = A - KCA$$, $$M_2 = K\Sigma_z \beta$$, $$\beta = \Lambda^T(\Psi + \Lambda \Lambda^T)^{-1}$$ (formula (6-8)), we'd claim what we saw in (5):

$$
\hat{x}_t = M_1 \hat{x}_{t-1} + M_2 u_t
$$

What I wrote as $$\hat{x}_{t \mid t}$$ is the posterior prediction which the authors denoted $$\hat{x}_{t}$$ for simplicity (same logic for $$t-1$$). The only subtlety that remains here is that in the above derivation I used the dynamic Kalman Gain $$K_t$$, calculated for every time point $t$. In the paper the authos utilized steady Kalman Gain (that's why there's no subscript $$t$$) but that's basically iterate the above filtering equations many times with the given set of model parameters ($$A,Q,C,R$$) until $$K_t$$ converges to some matrix $$K = \lim_{t \rightarrow \infty} K_t$$, and then use that matrix for all time steps. Basically, you could just replace $$K$$ with $$K_t$$ without changing the backbone of the inference structure. 

Before we jump into the perturbation method, the formula (5) does inform us the central philosophy of Kalman Filter: it integrates model prediction by its specified linear dynamics and the observation together, to arrive at an optimal (I'll not dive deep into what optimality represents here) posterior inference. Extracting out the linear relationship between prediction at timestep $$t$$ and the previous step $$t-1$$ together with the observation input would help understanding the perturbation method below.

## Perturbation method
Then the core methodology of this study is to change the BCI mapping so that the altered control space would be lying either within or outside of the insintric manifold. The paper does present some confusion as to how intuitive mapping and control space would be distinguished. My interpretation is that the control space refers to the ideal potential neural subspace for which to control the cursor optimally. Since within a short time neural connectivity is kept unaltered, the true intrinsic manifold is approximately invariant and thus the required potential neural subspace might not be reachable. By default the control space/intuitive mapping lies within the intrinsic manifold (that's why it's called "intuitive", because that's is what the neural network system has learned to achieve). 

The neuronal connectivity statistics is referred to as the natural co-modulation patterns.

In short, a within-manifold perturbation only reoriented the control space such that it still resides in the intrinsic manifold (shown in Fig.3 red line). This does not require monkeys to readapt neuronal connectivity patterns to achieve such new control space. It only altered the function from the intrinsic manifold to cursor kinematics. On the other hand, an outside-manifold perturbation alters the control space allowing it to live off the intrinsic manifold (Fig.3 blue line). Notice that if such outside-manifold perturbation pushes the control space along the orthogonal subspace that passes through the original control space, then the mapping from the neural comodulation patterns to cursor kinematics is preserved (basically just project the altered control space to the intrinsic manifold, then would recover the original control space). However, the underlying comodulation/covariation patterns among the neural population are altered. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Fig_1c.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="45%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig.1c in {% cite Sadtler2014 %}.
</div>

After the perturbation, the authors observed if the monkeys could eventually learn to readapt to the new mapping, to achieve great cursor control performance. For within-manifold perturbation, the monkeys only need to learn to associate cursor kinemaitcs to a new set of neural comodulation patterns (still within reach because lying in the same intrinsic manifold). However, for outside-manifold perturbation, they had to generate new co-modulation patterns in order to reach outside of the existing intrinsic manifold. Consequently, the authors predicted that within-manifold perturbation is easier to learn compared to outside-manifold perturbation. The authors did find results to back up this claim and since they are not the main focus of this blog, I'll refer interested readers to the original paper to take a look (FIgure 2). 


### Perturbation as permutation
Other than that, I do want to dive deep into how such within/outside-manifold perturbations were implemented. Specifically, 
from the derivation section readers should be already familiar with equations (5-8), the intuitive mapping. The perturbed mappings are the corresponding modified versions.

The within-manifold perturbation still manages to maintain the relationship between neural units to latent factors, but perturb that between latents and cursor kinematics: $$\hat{z}_{t}$$ is permuted before going into the Kalman inference (multiplying with the Kalman Gain)(Figure 1 red). Since permutation is simply re-orientation, the within-manifold perturbation is equivalent to re-orientating the coordinate system within the manifold (the manifold is preserved because the row space of $$\Lambda^T$$ is preserved (equation (7))). Mathematically, 

$$
\begin{align}
\hat{x}_t = M_1 \hat{x}_{t-1} + M_{2, WM}u_t \\
M_{2, WM} = K\eta_{WM}\Sigma_z\beta 
\end{align}
$$

and $$\eta_{WM}$$ is a $$10 \times 10$$ permutation matrix (10 because of the latent dimensionality)

For outside-manifold perturbation, it changes the relationship between the neural units and latent factors by permuting $$u_t$$ before passing it into factor analysis (Figure 1 blue). Specifically, 

$$
\begin{align}
\hat{x}_t = M_1 \hat{x}_{t-1} + M_{2, OM}u_t \\
M_{2, OM} = K\Sigma_z\beta \eta_{OM}
\end{align}
$$

and $$\eta_{OM}$$ is a $$q \times q$$ permutation matrix. The underlying logic is that in the __neural space__ for $$u_t$$, the monkeys might not be able to adapt to conteract the perturbation brought by permutation, since $$\eta_{OM}$$ directly acts upon $$u_t$$. 

### Select perturbed mappings
The authors also devised a clever plan to dictate the specific permutation matrices to choose (for a permutation matrix with size $$k \times k$$, there're $$k!$$ numbers of them) in three steps, with the central goal that the perturbed mapping would not be too difficult nor easy to learn:

#### Step 1: Find the candidate set

#### Step 2: Open-loop velocities prediction per  perturbation
#### Step 3: Determine candidate perturbations



## Quantifiable metric
### Amount of the learning
To quantify the potential amount of learning under two perturbation kinds, the authors resorted primarily to two performance metric: (the change of) relative acquisition time and relative success rate across perturbation blocks. Specifically, as shown below, the black dot represents the intuitive mapping, while the red and blue dots indicate the imediate performance just after corresponding perturbations. Red and blue asterisks represent the best performance during the within the perturbation sessions. The dashed line indicates the maximum learning vector $$L_{max}$$ (note that it starts on the red dot), and thus the aount of learning ($$A_i \in \mathbb{R}$$) is quantified as the length of the projection of the raw learning vector onto the maximum learning vector, normalized by the length of the maximum learning vector:

$$
A_i = \frac{L_{raw, i} \cdot L_{max}}{\|L_{max}\|^2}
$$

where $$i \in \{red, blue\}$$. Pictorially, it's illustrated as below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Fig_2cd.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="75%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig.2c and Fig.2d in {% cite Sadtler2014 %}.
</div>

Note that the asterisks in the above represent the time point/bin corresponding to __maximal__ amount of learning. In real case, for each time bin the authors would pinpoint the end points for the learning vectors (for calculations in details, please refer to the METHODS section of the paper), and compute the amount of learning correspondingly (the red and blue learning vectors might end in different positions with a diferent set of relative acquisition time and success rate up to that time bin).

The amount of learning for all sessions was presented above in the right pannel. Notice that a value of 1 indicates "complete" learning of the new relationship between the required neural co-modulation and cursor kinematics, reverting to the performance level of the intuitive control, while 0 indicates no learning. The authors did observe that there's significant amount of learning for within-manifold perturbations than outside-manifold perturbation. To see changes in success rates and acquisition time during perturbation blocks, instead of a single metric $$L$$ as shown above, the authors also plotted them separately as below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Ext_Fig_2.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="80%" %}
    </div>
</div>
<div class="caption">
    Adapted from Extended Fig.2 in {% cite Sadtler2014 %}.
</div>

### After-effects
A second metric Sadtler et. al. employed is observe how the monkeys performed as they reintroduce the intuitive mapping, or the so-called after-effects after washout of the perturbed mapping. Specifically, the after-effect is measured as the amount of performance imparement (tentative: acquisition time, success rate) at the beginning at the wash-out block (like how impairement was measured at the beginning of a perturbation block). A large wash-out effect indicates that the monkeys have learned and adapted to the perturbed mapping. For within-manifold perturbation, the authors did observe brief impaired performance but not so for outside-manifold perturbation, indicating that learning did occur during the former. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Ext_Fig_3.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="70%" %}
    </div>
</div>
<div class="caption">
    Adapted from Extended Fig.3 in {% cite Sadtler2014 %}.
</div>

### Principal/canonical angles
To quantify the comparisons between the intuitive and the perturbed mappings, Sadtler et.al. also calculated the principal angles between two linear subspaces. Notice that by formula () above, both subspaces are spanned by the rows of $$M_2$$ ($$M_{2, WM}$$ for within-manifold perturbation, and $$M_{2, OM}$$ for outside-manifold perturbation). Consuquently, the two principal angles specify the maximum and minimum angles of separation between the intuitive and the perturbed control spaces. Notice that since the spike counts are z-scored, the control spaces also center at the origin.  

Sidenote: How do principal angles relate with principal eigenvalues and principal curvatures?

To give a short summary of principal angles calculation:

Note the role of SVD decompsition. 


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

The dissection of control into an estimation of intrinsic manifold (Factor Analysis) and then build an intuitive mapping (decoder, Kalman Filter) to relate the latent factors to cursor kinematics is different from directly mapping neural activities to movement. Such dissection becomes a common theme for the past two decades, with the emphasis on latent manifold structure beecomes increasingly popular. More than a theoretical refinement, practically speaking, such dissection also allows the authors to perform two different types of perturbation.

Talk more here and also relate this to the nonlinear manifold paper 2025. 


## Conclusions
The neural manifold reflects the inherent connectivity which constrains (in a short term) the potentially learnable patterns. Consequently, the neural connectivity network structure dictates possible neural patterns and corresponding behavior repertoire the animals are capable of performing. 

This paper strengthens my belief in the legit usability of the low dimensional structure among neural population, and more crucially the value of perturbation methods to causally verify the neural manifold. 


# Batista 2025

## Discussions


## Conclusions


# Discussions
These two studies offer powerful information that dimensionality reduction could be not just a visualization tool, but a causal summary of the underlying neural connectivity and anatomical constraints, which correlates to the neural computations that neural population could implement. 

Associating Sadtler 2014 with Churchland 2012, there's a common convergence on using matrices to explore transformations. This again reinforces my idea that perhaps group theory needs to be rigorously introduced into neursocience for ... (also associate with Barack and Kraukauer "Two views on the cognitive brain", which relates __computation__ to __transformation of representations__ to explain cognitive phenomena)

# Conclusions
