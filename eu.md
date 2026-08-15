---
layout: default
title: Towards better Estimators of Epistemic Uncertainty
description: Personal Project - Targeting Workshop Submission
--- 

<script defer src="https://cdn.jsdelivr.net/npm/mathjax@4/tex-chtml.js"></script>

**[Home](index.md)** | **[Research & Projects](projects.md)** | **[CV](cv.md)** | **[Contact](contact.md)**
---
This blog is 100% human-written. <br><br>
## Introduction & Background 
In machine learning, predictive uncertainty arises in two ways: Imperfect knowledge about the data-generating process ("Epistemic Uncertainty"), and irreducible noise in the data-generating process ("Aleatoric Uncertainty"). <br><br> In the classification setting, we use a function $$\mathcal{l}(\hat{\theta}, y)$$ to score the loss associated with the predicted probabilities for each class, $$\hat{\theta}$$, given the one-hot encoding of the true class, $$y$$. The aleatoric uncertainty can be defined as the expected loss under our predicted class probabilities which come from the learned model posterior $$Q$$\[1\]: 
$$\begin{equation} AU(Q) = \mathbb{E}_{\hat{\theta} \sim Q}\left[\mathbb{E}_{y \sim \hat{\theta}}[\mathcal{l}(\hat{\theta}, y)]\right]. \end{equation}$$
In words, aleatoric uncertainty quantifies the inherent uncertainty in the distribution over classes, under the model posterior[^1]. For example, if we use the negative log-likelihood $$\mathcal{l}(\hat{\theta}, y) = -\log(\hat{\theta}^Ty)$$, $$AU(Q)$$ is the expected entropy of $$\hat{\theta}$$.<br><br>
We can get an unbiased estimator for the aleatoric uncertainty using Monte Carlo simulation:
$$\begin{equation}
\widehat{AU(Q)} = \frac{1}{N}\sum_{i=1}^N S(\theta^{(i)}),
\end{equation}$$
where $$S(\cdot)$$ denotes, as our running example, the Shannon entropy. <br><br> 
The epistemic uncertainty now is defined as the expected additional loss incurred by the use of the averaging estimator $$\bar{\theta}$$[^2]:
$$\begin{equation} EU(Q) = \mathbb{E}_{Q}\left[\mathbb{E}_\hat{\theta}[\mathcal{l}(\bar{\theta}, y)] \right]- AU(Q) \end{equation}$$
In the case of our running example, where $$\mathcal{l}$$ is the negative log-likelihood, $$EU(Q)$$ is the expected Kullback-Leibler divergence of $$\hat{\theta}$$ from $$\bar{\theta}$$,
$$\begin{equation} \mathbb{E}_{Q}\left[D_{KL}(\hat{\theta}||\bar{\theta}) \right]\end{equation}.$$
Again, we will have to resort to a sampling-based approximation. For example, let us consider deep ensembles: Integration over the exact posterior is intractable, but we can sample it by training the same deep neural network architecture with different parameter initializations \[3\]. We obtain samples of the trained parameters $$\theta^{(1)}, \theta{(2)}, ..., \theta^{(M)}$$. However, assuming we want to use all of our samples for prediction, we cannot easily obtain an unbiased estimator of $$EU(Q)$$, due to the presence of $$\bar{\theta}$$ inside the expectation. This leads to two questions which this project aims to address:
1. Is $$\bar{\theta}$$, as used in our definition of epistemic uncertainty, $$\int_{Q} \theta d\theta$$ or $$\frac{1}{M}\sum_{i=1}^M \theta^{(i)}$$?
2. Once we've answered 1., how should we estimate $$EU(Q)$$?<br>

## Answering Question 1 <br>
To begin, let us examine the impact of our choice for $$\bar{\theta}$$. As a toy example, we can have $$Q$$ be $$U[\frac{1}{2}, \frac{3}{4}]$$. In this case, the below figure shows the discrepancy between the two induced estimation targets.
<div align="center">
  <div id="thesis-graphic-container" style="position: relative; cursor: pointer; display: inline-block;">
    <img id="graphic-state-0" src="DefinitionsDiscrepancy.jpg" alt="Definitions discrepancy" style="width: 50%; display: block;">
  </div>
</div>
As we would expect, the finite ensemble-based target approaches the exact averaging estimator-based target from above: The expected KL-Divergence from the true mean of $$\theta$$ is smaller than the expected KL-Divergence from the sample mean, though the two will coincide asymptotically. <br><br>Which curve should we attempt to estimate? Here, we attempt to estimate the red curve, because we concern ourselves with predictive uncertainty: We want to compute a measure of uncertainty associated with the predictions coming from the model that we will actually use for prediction, not the model that we would use if we learned black magic to integrate over neural network training[^3]. 

## Answering Question 2 <br>
We are now ready to define the target for estimation
$$\begin{equation} \mathbb{E}_{\hat{\theta}, \theta^{(1)}, \theta^{(2)}, ..., \theta^{(M)} \sim Q} \left[D_{KL}(\hat{\theta}||\frac{1}{M}\sum_{i=1}^M \theta^{(i)}) \right]\end{equation}$$

Currently, standard practice is the following, in-sample estimator:
$$\begin{equation} \frac{1}{M}\sum_{i=1}^M D_{KL(\theta^{(i)}||\frac{1}{M}\sum_{i=1}^M \theta^{(i)})} \end{equation}$$
Clearly, this estimator will underestimate the target. In particular, its bias is given by:
$$\begin{align} \&\mathbb{E}\bigg[D_{KL}(h_1(x)||\frac{1}{M}\sum_{j=1}^Mh_j(x))-D_{KL}(h_{M+1}(x)||\frac{1}{M}\sum_{j=1}^M h_{j}(x))\bigg] \\
= \&\mathbb{E}\bigg[\sum_{k=1}^K \big ( (h_{M+1}(x)_k-h_1(x)_k)\log (\sum_{j=1}^M h_j(x)_k) \big)\bigg] \end{align}$$

[^1]: This definition of aleatoric uncertainty has been criticized because it will be affected because of the learner's inability to learn a Dirac delta distribution for $$Q$$ from a finite amount of samples. Most often, the true $$\theta$$ will have a lower entropy than the average $$\theta$$ under $$Q$$, and we will therefore generally overestimate the true amount of irreducible noise in the data-generating process, as has been empirically demonstrated in \[2\]. However, the use of this definition remains standard practice.
[^2]: This too has been criticized, and this too remains standard practice.
[^3]: Bayesian neural networks can provide this black magic sometimes, but suffer from limitations such as even larger computational cost than deep ensembles.
