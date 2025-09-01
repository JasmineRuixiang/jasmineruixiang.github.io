---
layout: post
title: Learning and constraints (in progress)
date: 2025-08-25 22:09:34
description: Neural constraints, both spatial and temporal
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
This blog covers two papers which focuses on exploring constraints during learning in monkey's behavior from different perspectives. The first one {% cite Sadtler2014 %} approaches the constraint as a spatial restriction of neural activities residing upon some low dimensional neural manifold, while the second {% cite Oby2025 %} deliberately perturbs the motion sequence, the temporal order movement to explore whether the monkeys are capabable of adapting neural dynamics. In terms of methods, these two papers share much in common in terms of latent space calculation, and in order not to make this blog unbearably long, I will elaborate more on the first paper's algorithms, while glossing over many details in the second. In total, through juxtaposing these studies, hopefully we could glean some integrated thoughts on the constraints of neural dynamics form both the spatial and temporal perspectives. 

Both spatial (neural manifold view) and temporal (dynamical system view) on constraints of neural activities. 

# Spatial constraints (Sadtler et.al. 2014)
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
&= I - \Lambda^T(\Psi + \Lambda \Lambda^T)^{-1}\Lambda
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

and $$\eta_{OM}$$ is a $$q \times q$$ permutation matrix ($$q$$ is the number of neural units). The underlying logic is that in the __neural space__ for $$u_t$$, the monkeys might not be able to adapt to conteract the perturbation brought by permutation, since $$\eta_{OM}$$ directly acts upon $$u_t$$. 

### Select perturbed mappings
The authors also devised a clever plan to dictate the specific permutation matrices to choose (for a permutation matrix with size $$k \times k$$, there're $$k!$$ numbers of them) in three steps, with the central goal that the perturbed mapping would not be too difficult nor easy to learn: 

#### Step 1: Find the candidate set
For within-manifold perturbations, all $$10!$$ possible permutation matrices are treated as the candidate set. For outside-manifold perturbations, th strategy differs for two monkeys. For one monkey only permutations of nueral units with largetst modulation depths are selected. For the other monkey, the solution is to randomly put all units with the highest modulation depths into 10 groups of $$m$$ each (the rest with low modulation forms an 11th group). The outside-manifold perturbation is formed by permutating these 10 groups instead of each unit (thus $$10!$$ in total matching that of within-manifold perturbation). 

#### Step 2: Open-loop velocities prediction per  perturbation
The second step hinges on estimating the open-loop velocities for each candidate permutation. Specifically, it approximates the decoded cursor kinematics if the monkeys did not learn to adapt:

$$
\begin{equation}
x_{OL}^i = M_{2, P}u_{B}^i
\end{equation}
$$

where $$u_{B}^i$$ is the mean z-scored spike counts across all trials on the $$i^{th}$$ target($$8$$ in total), and $$P$$ represents either $$OM$$ or $$WM$$. The method here echoes the dissection made explicity in equation(5), where the current step prediction $$\hat{x}_t$$ is a linear combination of prediction from last step $$\hat{x}_{t-1}$$ and neural activities at the present $$M_2 u_t$$. 

#### Step 3: Determine potential perturbations
Finally, to determine a perturbation the authors compared the open-loop velocities under the perturbed mapping with those under the intuitive mapping for each target. These velocities should only differ in an acceptable range so the monkeys would not find it too simple nor difficult to learn. The authors quantified this metric by defining a range over differences in velocity angles and magnitude, and chose perturbations that fall in this specified range for all targets. 


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

For defining the candidate set of potential outside-manifold perturbation for the second monkey,  while the group looping strategy is a clever way to equalize possible permutation matrices with within-manifold perturbation ($$10!$$), I'm not completely persuaded by the logic behind it. When explaning the logic for the second monkey outside-manifold perturbation, the authors stated that the within-maifold perturbation only permutes the neural space with $$10$$ dimensions, while the outside-manifold perturbation on average deal with 39 dimensions (number of permuted units). Thus the monkey would have to search for more space for outside-manifold perturbation. I think the subtlety here lies in the fact that within-manifold perturbation is not performed on the level of the ambient neural space, but on the latent 10-dimensional space which is extracted out of the original neural space and each basis vector of the latent space is a __linear combination__ of the neural units. Its is not explicitly clear to me whether/how these two spaces should be compared solely based on the dimensonality it carries. 

Additionally, though I do appreciate the flavor of group assignment (there's even a flavor of group action here; anyway, permutation matrices form a permutation group), I am not sure if each of the $$10!$$ permutations on groups of neural units is "equivalent" to a permutation among 10 latent axes. 

Another question is outside-manifold perturbation is not necessarily guaranteed to be outside the manifold?

### Required changes in preferred directions
There's one interesting method that I don't have enough time to delve into, which is the calculation of changes in preferred directions for each neural unit. This is calculated to make sure that learning two perturbation types would require similar effort for the monkeys to adapt (interested readers might refer to Figure 3b in the paper). The authors began by discussing comparing the columns of $$M_2, \; M_{WM, or OM}$$, which reflects how each neural unit impacts the decoded cursor kinematics. This strategy is not adopted because for the second monkey $$M_2$$ and $$M_{OM}$$ share many columns for the un-permuted columns, making it an unfair comparison. However, it's informative if we associate this view of $$M_2 u_t$$ by assiging each column of $$M_2$$ with a coordinate in $$u_t$$ with that mentioned in the principal angles section where rows of $$M_2$$ represnet an axis upon which $$u_t$$ is projected. These two perspectives which interpret linear matrix multiplication as either a __transformation__ (of basis vectors in space) or a __projection__ which will be further illustrated in an upcoming post. 

Then, the authors came up with another technique. They assumed that 

> 1] under perturbation the monkeys would still manage to keep the same cursor velocity in the intutive mapping, 
>
> 2] The perturbed firing rates should be as close as possible to those in the intuitive mapping

and these two assumptions transform into a constrained optimization problem: Find $$u_p^{i}$$ such that its Euclidean distance with $$u_B^{i}$$ is minimized when $$M_2u_B^i = M_{2, P}u_p^i$$:

> $$u_p^{i, *} = \arg\min_{u_p^i} ||u_p^i - u_B^i||_2$$
>
> s.t. $$M_2u_B^i = M_{2, P}u_p^i$$

This can be solved in closed-form with Lagrange multipliers (for rigorosity, the inverse below should be replaced with the Moore-Penrose Pseudoinverse, unless $$M_{2, P}$$ is full-rank, which I'm not sure I could theoretically make that claim). Due to limited space, I'll not leave the proof to another blog on linear transformation, which I also illustrate its relationship with CCA and another paper (Juan Gallego, trajectory alignment):

$$
\begin{equation}
u_p^i = u_B^i + M_{2, P}^T(M_{2, P}M_{2, P}^T)^{-1}(M_2 - M_{2, P})u_B^i
\end{equation}
$$

where $$u_B^i$$ is the mean normalized spike count vector across all trials for each target $$i$$ in the basline blocks. 

Then the authors fit a standard cosine tuning model for each unit $$k$$ with all targets:

$$
\begin{equation}
u_B^i(k) = m_k \cdot cos(\theta_i - \theta_B(k)) + b_k
\end{equation}
$$

where for each neural unit $$k$$, its preferred direction is encoded as $$\theta_B(k)$$, $$m_k$$ the depth of modulation, $$b_k$$ the model offset, $$\theta_i$$ the direction of the $$ith$$ target. Apply the same calculation for $$u_P^i$$ to obtain the preferred direction $$\theta_P(k)$$ for each unit $$k$$ under the perturbaed mapping. Finally, the preferred direction changes (for each neural unit) is calculated as:

$$
\begin{equation}
\mid \theta_P(k) - \theta_B(k) \mid
\end{equation}
$$

### Selection of intrinsic dimensionality
Usually we do not have a coherent and systematic way to detemrine the optimal intrinsic dimensionality. Here for factor analysis, based on its explicit probabilistic inference structure, the authors could easily compute the likelihood for cross-validated data. 

### Measurement of cumulative shared variance
Based on equations (13), the original covariance of $$u$$ is decomposed (with minor substitutions) into a shared component $$\Lambda \Lambda^T$$ and an independent component $$\Psi$$. In order to calculate the amount of shared variance along orthogonal directions within the manifold (notice this is a linear manifold), consequently, the authors calculated the eigenvalues of $$\Lambda \Lambda^T$$ which present the shared variances, each corresponds to an orthonormalized latent dimension. This is similar to Churchland 2012 the last blog...


## Conclusions
The neural manifold reflects the inherent connectivity which constrains (in a short term) the potentially learnable patterns. Consequently, the neural connectivity network structure dictates possible neural patterns and corresponding behavior repertoire the animals are capable of performing. 

This paper strengthens my belief in the legit usability of the low dimensional structure among neural population, and more crucially the value of perturbation methods to causally verify the neural manifold. Specifically, the extraction of latent factors, other than directly mapping neural activties to cursor kinematics, not only adds more interpretability to the framework, but also provies a readily distinguishable strategy of within/outside-manifold perturbations. This reminds me of many other models with latent factors in between: (xxx, xxx, xxx, xxx). 



# Dynamical constraints (Oby et.al. 2025)
This paper {% cite Oby2025 %} presented some surprising facts about neural dynamics. One key question the authors are interested is whether the sequential representation or computation that neural population could perform is temporally locked. The critical assumption is that if neural dynamics does reflect the underlying connectivity, then it should be robust and difficult to alter. More specifically, they tested if such neural population patterns could be produced but in a __reversed__ time ordering and the results show otherwise: even when the animals were presented with different visual feedback or strong incentive to change the time order (for example, reversing the time course) of the neural activities, the temporal evolution of the neural dynamics is still robust and difficult to violate (thought I think the claim might be too strong. See more in the discussions).   

## Different views of the high dimensional neural space


<div class="row mt-3">

  <!-- Left column: text -->
  <div class="col-sm-7">
    <p>
      Note that unlike many previous BCI works, neural activities are mapped to curosr __positions__ directly instead of velocities. The modeling framework is similar to {% cite Sadtler2014 %}, in that high dimensional neural activities <strong>90D</strong> are mapped into <strong>10D</strong> latents by <strong>GPFA</strong> (instead of simply <strong>FA</strong> in {% cite Sadtler2014 %}), and then build corresponding linear maps from <strong>10D</strong> latent factors to <strong>2D</strong> cursor positions using different maps. Since each map is a linear projection of latent factors to <strong>2D</strong> space, it is geoemtrically equivalant to observing the high dimensional signals from a specified angle. The key ingredient of this paper is that the authors found if with some linear <strong>2D</strong> mapping/projection, the neural trajectories are readily flexible and reversible, whereas some other views robustly exhibited no significant change even if the monkeys were inspired to alter the neural trajectories.
    </p>

    <p>
      There're two mappings the authors emphasizd, one is called "movement-intention" (MoveInt) mapping, under which the monkeys could intuitively control the cursor to move between two diametrically placed targets <strong>A</strong> and <strong>B</strong>. Indeed, under MoveInt, the neural trajectories (equivalent to cursor motion here) could go back and forth between <strong>A</strong> and <strong>B</strong> with significant overlapping. This might lead readers to believe that the neural dyanmcis are also reversible. 
    </p>
  </div>

  <!-- Right column: image -->
  <div class="col-sm-5 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Oby_Fig_2.png" class="img-fluid rounded z-depth-1" zoomable=true
        %}
    <div class="caption">
      Adapted from Fig. 2 in {% cite Oby2025 %}. a: decoding pipeline. b: Overlapping neural/cursor trajectories under multipl orientations. From this it seems that time courses of neural dynamics are flexible for different orientations (thus including reversing the temporal order). 
    </div>
  </div>
</div>

However, under another 2D mapping selected as maximizing the separations ($$SepMax$$) between $$A$$ to $$B$$ trajectories v.s. $$B$$ to $$A$$ trajectories, the neural trajectories overlapping by $$MoveInt$$ projection are clearly distinguished. This is shown below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Oby_Fig_3.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="75%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig. 3 in {% cite Oby2025 %}. See a. and b. where A-B and B-A trajectories are distinct. Panels c. and d. show results from quantitative metric (discriminability <strong>d'</strong> between midpoints of trajectories signify separation of trajectories).  
</div>

### MoveInt vs SepMax projections
Both $$MoveInt$$ and $$SepMax$$ projections are calculated by fitting a simple linear transformation to the extracted $$10D$$ latents $$\hat{z}_t$$ to acquire $$2D$$ cursor positions $$\hat{x}_t$$, which is used to displace the cursor in closed-loop control:

$$
\begin{equation}
\hat{x}_t = W\hat{z}_t + c
\end{equation}
$$

except they have different weight matrices and bias $$W$$ and $$c$$.

For $$MoveInt$$ projection, $$\hat{x}_t$$ is selected as the label velocity vectors, and thus $$W_{MI} \in \mathbb{R}^{2 \times 10}$$ and $$c_{MI} \in \mathbb{R}^{2 \times 1}$$ are obtained by the correpsonding linear regression:

$$
\begin{align}
W_{MI} = XZ^{T}(ZZ^T)^{-1} \\
c_{MI} = -W_{MI}(\frac{1}{n}\sum_{t=1}^{n}\hat{z}_t)
\end{align}
$$

where $$Z = [\hat{z}_1, \hat{z}_2, ..., \hat{z}_n] \in \mathbb{R}^{10 \times n}$$ represents the collection of $$GPFA$$ latent states, and $$X = [x_1, x_2, ..., x_n] \in \mathbb{R}^{2 \times n}$$ indicates the intended cursor position. $$n$$ is the total number of timesteps during calibration. 

For $$SepMax$$ projection, the goal is to identify subspace upon which cursor trajectories showcased maximal separation during the two-target task from target pair $$A$$ to $$B$$ and $$B$$ to $$A$$. The projections is acquired by satisfying the following 3 objectives: 1] maximizing separation between midpoints of both ways ($$\bar{z}_{AB}$$ and $$\bar{z}_{BA}$$); 2] minimizing trial-to-trial variance at midpoints ($$\Sigma_{AB}$$ and $$\Sigma_{BA}$$); and 3] maximizing projections of $$\bar{z}_{A}$$ and $$\bar{z}_{B}$$. 

The algorithm for $$SepMax$$ projection could be summarized in the following:

> 1] Compute trial-averaged starting points $$\bar{z}_{A}, \; \bar{z}_{B}$$ from $$A$$ to $$B$$ and $$B$$ to $$A$$ trials, respectively, 
>
> 2] Calculate midpoint $$m = \frac{\bar{z}_{A} + \bar{z}_{B}}{2}$$
> 
> 3] For a given trial, project the trajectory $$\hat{z}_t$$ onto the axis connecting $$\bar{z}_{A}$$ and $$\bar{z}_{B}$$. Define $$\hat{z}_{t_{c}}$$ as the trial midpoint, where $$t_{c}$$ is the timepoint where the projection is closest to $$m$$, 
>
> 4] Define $$\bar{z}_{AB}$$ as the trial-averaged midpoint over $$\hat{z}_{t_{c}}$$ from $$A$$ to $$B$$ trajectories, similarly $$\bar{z}_{BA}$$ from $$B$$ to $$A$$, 
>
> 5] Simiar to 4], calculate covariance $$\Sigma_{AB}$$ over $$\hat{z}_{t_{c}}$$ for $$A$$ to $$B$$ trials, and simiarly $$\Sigma_{BA}$$ for $$B$$ to $$A$$ trials, 
>
> 6] Finally, to obtain the ideal $$2D$$ projection and find two orthonormal vectors collected in $$P_{SM} = [p_1, p_2] \in \mathbb{R}^{10 \times 2}$$, solve the optimization problem below:
> 
> $$ J = -w_{mid}p_{1}^T(\bar{z}_{AB} - \bar{z}_{BA}) + w_{var}p_{1}^T(\Sigma_{AB} + \Sigma_{BA})p_1 - w_{start}p_{2}^T(\bar{z}_B - \bar{z}_A)$$
>
> 7] To align the space from $$P_{SM}$$ with animals' workspace: 
> $$W_{SM} = AP_{SM}^T, \;, c_{SM} = -AP_{SM}^Tm$$
> 
> where
> 
> $$A = R_{\theta}OS$$

Note that $$\bar{z}_{A}, \bar{z}_{B}, m, \bar{z}_{AB}, \bar{z}_{BA} \in \mathbb{R}^{10 \times 1}$$, $$W_{SM} \in \mathbb{R}^{2 \times 10}, A \in \mathbb{R}^{2 \times 2}, c_{SM} \in \mathbb{R}^{2 \times 1}$$ and $$w_{mid}, w_{var}, w_{start}$$ are all weighting hyperparameters. The above objective function is composed of 3 separate aims which match exactly the those mentioned in above. Consequently, along $$p_1$$ trajectories of different directions are maximally separated, while targets are distinguished on an orthogonal $$p_2$$ axis.

The goal for step 7] is that after projection the starting points of $$A$$ to $$B$$ and $$B$$ to $$A$$ align with the two targets $$A$$ and $$B$$ in animals' workspace. $$A$$ is comprised with 3 transformations:

> 1) Scaling the axes of $$P_{SM}$$ with a diagonal matrix $$S \in \mathbb{R}^{2 \times 2}$$ such that distance between $$\bar{z}_A$$ and $$\bar{z}_B$$ is identical to $$A$$ and $$B$$ in $$MoveInt$$ projection, 
>
> 2) Optionally flip the projection along the $$A - B$$ axis with $$O \in \mathbb{R}^{2 \times 2}$$, such that the controlled cursor movements align with the expected orientation,
>
> 3) Rotation with $$R_{\theta}$$ so the projection aligns with workspace targets $$A$$ and $$B$$. 

Notice that the above postprocessing is not an isometry, in that a scaling is also applied ($$P_{SM}). 

After identifying the existence of irreversible neural trajectories, the authors continued to explore how robust time course evolution is, with 3 experiments that built upon the previous ones which increasingly motivated the monkeys to adapt neural dynamics.  

## Task 1: Visual feedback task
Firstly, the authors gave monkeys the visual feedback of neural trajectory mappings by $$SepMax$$ projection (not $$MoveInt$$). Based on the underlying inherent tendency to streighten cursor trajectories, the authors intended to observe if the monkeys would indeed made such adjustments, which would then demonstrate that temporal evolution of neural patterns is flexible. However, the authors found that the cursor trajectories were robust and were independent of which visual feedback the monkeys received. This further corroborates the claim that the monkeys did not have volitional control over certain time order of neural trajectories. The experiment paradigm and results are summarized below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Oby_Fig_4.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="80%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig. 4 in {% cite Oby2025 %}. Given visual feedback from the <strong>SepMax</strong> projection (b.), the authors still do not observe straightening of cursor trajectories for <strong>SepMax</strong> projection (c.). In fact, projections in either linear subspace are robust whether which visual feedback is presented (d.). 
</div>

One key remaining question is whether the robustness of constraint exists only in the $$SepMax$$ projection subspace, or if it's a common phenomena also observable in other dimensions. Consequently, the authors applied random $$2D$$ projections of the $$10D$$ latents and conducted flow field analysis (I did not have space to go into details, but flow field analysis was employed to quantify the discrimination of trajectories). 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Oby_Ext_Fig_4.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="85%" %}
    </div>
</div>
<div class="caption">
    Adapted from Extended Fig.4 in {% cite Oby2025 %}. Latent factors are projected into <strong>2D</strong> space by random projection matrices and the flow field is calculated. This is repeated for 400 times and the corresponding comparisons of flow fields in the corresponding projections  under different feedbacks (<strong>MoveInt</strong> or <strong>SepMax</strong>) are shown in cyan distribution as "other feedback" in g. and h. Note that d. is an example comparison of flow fields for <strong>SepMax</strong> projection. In order to interpret the other-feedback distributions the authors also devised two control groups where no change or maximal change in flow fields is expected. For no-change distribution, subsets of trials from the same feedback is used to calculate flow fields, called "fixed feedback". For maximal change, two distributions are devised: one is reversing the direction of the flow field so they overlap and are maximally different (called "time-reversed", e.), and the other with altered starting targets (called "alternate-target", f.). The difference in flow field is min-max normalized by fixed feedback and time reversed feedback, respectively, the smaller the less difference among flow field comparisons. g. indicates that other feedback distribution is not significantly different from fixed feedback. h. shows that state space highly overlaps between fixed feedback, other feedback, andn time reversal. Taking stock, the other feedback distributions show low flow difference under and hight flow overlapping under different feedbacks, suggesting that the neural trajectories are indeed robust in the <strong>10D</strong> latent space. 
</div>

Note that the above figure only demonstrates that neural trajectories are robust and highly consistent/__constrained__ under different feedbacks in many different dimensions, viewed from distinct perspectives/projections. This does not necessarily translate to the unversality of separability of trajectories in different subspaces other than $$SepMax$$ projection. It is the former, the inability to alter neural trajectories under whatever visual feedback in some projected space, that the authors refer to as the __constraint__. It is this constraint that the authors found could be extended to many other dimensions (other than just $$SepMax$$ projection) by the distribution from random projections (other feedback) shown above. 

## Task 2: IT task

<div class="row mt-3">

  <div class="col-sm-7 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Oby_Fig_6.png" class="img-fluid rounded z-depth-1" zoomable=true
        %}
    <div class="caption">
      Adapted from Fig.6 in {% cite Oby2025 %}. Looking at the flow field generated by the previous two-target trials (a.), the authors explored if the monkeys could move back against the flow field (b.), to achieve an "intermediate target" (IT) on the midway (c.). Black lines: single-trial trajectories, thick lines being the trial average. For quantification, initial angles of trajectories were calculated under multiple comparison groups. d.: trajectory to IT (black) v.s. direct path against the flow field (black dashed), compared to trajectory to the blue target (red) v.s.direct path (black dashed), showed as % difference in e. 0% indicates the cursor movement is along the flow field, whereas 100% implies that monkeys could move against the flow field. The authors also performed similar analyses with no-change (f. and g.: initial angles between direct path to IT and early trials (red), with direct path to IT and late trials(dashed red)) and full-change control (h. and i.: initial angles between directh path to IT with different targets (here blue and aqua))
    </div>
  </div>

  <div class="col-sm-5">
    <p>
        In the second task, the authors explicitly motivated the monkeys to present the neural activities (by the cursor movement) in a time-reversed ordering by motivating them to move against an empirically derived flow field (from past cursor trajectories) towards an "intermediate target" (<strong>IT</strong>) along the path from <strong>A</strong> to <strong>B</strong>. The authors observed that in order to go from target <strong>B</strong> to <strong>IT</strong>, the monkeys did not adopt the time-reversed pathway of <strong>A</strong> to <strong>B</strong>, but followed, at least initially, the path specified by the flow field from <strong>B</strong> to <strong>A</strong> and then reverted back towards <strong>IT</strong>. Consequently, the monkeys failed to reproduce the existing neural activities patterns in the reversed time-ordering, and they did not (initially) violated the flow field. This is quantified by initial angles of trajectories across multiple control/comparison groups ('no-change': early vs. late trials, where there is no expectation of difference; 'full-change': difference of initial angles among trials of different end targets (same starting target) in the <strong>MoveInt</strong> projection), and the authors discovered that the initial angles during the IT task is not significantly different from the 'no-change' condition, but not with 'full-change' condition. This demonstrates that the time course of neural population is robust and not easily modifiable. 
    </p>
  </div>

</div>


## Task 3: Instructed path task
In the third task, in order to further motivate the monkeys to reverse neural trajectories, the authors appied an "instructed path task" where a visualb boundary was applied to the time-reversed trajectory. However, the monkeys only minimally modified the cursor trajectories as the bounday was reduced in size, and the trajectories followed the flow field instead of violating it while showcasing the reverting/hooking phenomena in the $$IT$$ task. This again demonstrates that the temporal evolution of neural activity patterns is robust and thus it is robustly constrained. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Oby_Fig_7.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="85%" %}
    </div>
</div>
<div class="caption">
    Adapted from Fig.7 in {% cite Oby2025 %}. The monkeys were required to move the cursor to the IT (as in Task 2) within the visual boundary (a.), with the boundary decreasing in size to motivate monkeys to change trajectories (b., failed trials shown in red). c.: five boundary sizes with different distances from the boundary to the target(dashed line), and trial-averaged trajectories for successful trials only for each boundary. d. and e.: Comparison of the initial angle between direct IT path (black dashed) and the trial-averaged trajectory of all trials (black), and that between direct IT path (black dashed) and trial-averaged trajectories in the two-target taks (task 1, dark red). The distribution of percent difference is similar to the no-change distribution (dark red triangle and line) seen in Fig.6. indicating that even with visual boundaries the cursor trajectories are still robust. 
</div>


## Discussions
I like how the experiments are built upon each other to add more constraints and motivation for the monkeys to reverse the time course of neural activities. The quantification methods used (flow field analysis, initial angle comparison among trajectories) are great tools to add to future research. Many adjustments and tweaks of experiment sessions to enable further exploration are interesting to read, like for Task 1 the authors also flipped the projection on the screen along the axis connecting two targets, in order to observe if cursor trajectories would also be reflected or not (details not discussed in this blog, illustration shown below; should be self-explanatory). 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/learning_constraint/Oby_Ext_Fig_5.png" class="img-fluid rounded z-depth-1" zoomable=true
        width="85%" %}
    </div>
</div>
<div class="caption">
    Adapted from Extended Fig.5 in {% cite Oby2025 %}.  
</div>


A dynamical system does not simply allow flowing back.

Instead of an abrupt change of the experiment, graduallly apply the changes: not to go to reversa in total in an instant: reminds me of hysterisis. The conclusion seems so strong. 

Would this be a byproduct of the training process itself? ---- if the monkeys were trained with visual feedback of the maximum separation view, would that be different?


### Linear mapping
Linear manifold, linear mapping, orthogonal/null space


### Dynamical systems explanation
The authors claimed that the observed time course activities reflect the underlying connectivity among the neural population. They made associations with network models where at any present, the activities of each node is determined by the previous time behavior of all nodes, the network connectivity, and the external input. This is explicitly reflected by the dynamical systems perspective (let's not go to SDE for now):

$$
\begin{equation}
\dot{x}(t) = f(x(t)) + u(t)
\end{equation}
$$

Discretizing the above would reveal previous time step dependence, and the dynamics is specified by $$f$$ determined by network connectivity. The paper {% cite jpca %} discussed in [my earlier post]({% post_url 2025-08-16-jPCA %}) made this the backbone of modeling. From this perspective, it seems not surprising that neural trajectories do not necessarily need to be reversible. One extreme hypothetical case would be the following:

Consequently, to alter the time course would require substantial adjustment of the connectivity itself to change $$f$$, which in a short time span is not quite likely readily achievable. 

### Constraints on the input
Since the monkeys were assumed to freely change their neural activities input to $$M1$$, the fact that they were no capable of altering the trajectory indicates that changing inputs could only affect the dynamics in a limited way. The authors hypothesized that perhaps the inputs could only affect certain dmensions of neural dynamics (for example, shown by $$MoveInt$$), or it could not overcome the intrinsic dynamics the neural network carries. s

### Hysteresis explanation

### Eventual, but not initial, violation of flow field

### Determinism and the exception
To arrive at a given state $$z(t)$$, $$z(t-1)$$ is pre-determined. The only exception is attractor and limit cycle? 

### Relation with rotational dynamics
The study might remind us of rotational dynamics (cite jPCA paper). Previous study showed that simply by reversing the hand kinematics would not necessarily lead to reversal of the direction of the rotational dynamics (% cite russo2018 %). What's even more striking here is that there's no arm movement component included here, thus further emphasizing that kinematic movement or somatosensory feedback (as shown coupling many monkey studies) is not a necessary condition for engendering the neural dynamics, which nonetheless carries on an inherent property of the underlying neuronal connectivity. 

### Relation with low-tangling
On another hand, the monkeys were motivated to produce neural latents that evolve with high-tangeling, but in fact neural trajectories showed only low tangling. 

### Relation with output-null/potent space
Finally, this does echoe the flavor of output-potent/null subspace (another blog coming soon). Indeed, the distingushed properties of neural dynamics observed in $$MoveInt$$ v.s. $$SepMax$$ projections echoe the observation that different perspectives of viewing neural dynamics might lead to functionally distinguished inerpretations: preparatory activities encoded in the null-space are not read out as they are projected onto the "same" coordinates on the readout output-potent space, but they specify the initial conditions to drive the temporal evolution of neural activities along the output-potent space. 


## Conclusions
The animals' dynamical structure is robust and highly constrained. 

# Discussions
These two studies offer powerful information that dimensionality reduction could be not just a visualization tool, but a causal summary of the underlying neural connectivity and anatomical constraints, which correlates to the neural computations that neural population could implement. 

Associating Sadtler 2014 with Churchland 2012, there's a common convergence on using matrices to explore transformations. This again reinforces my idea that perhaps group theory needs to be rigorously introduced into neursocience for ... (also associate with Barack and Kraukauer "Two views on the cognitive brain", which relates __computation__ to __transformation of representations__ to explain cognitive phenomena)

# Conclusions
