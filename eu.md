---
layout: default
title: Towards better Estimators of Epistemic Uncertainty
description: Personal Project - Targeting Workshop Submission
--- 

<script defer src="https://cdn.jsdelivr.net/npm/mathjax@4/tex-chtml.js"></script>

**[Home](index.md)** | **[Research & Projects](projects.md)** | **[CV](cv.md)** | **[Contact](contact.md)**
---
This blog is 100% human-written. <br><br>
In machine learning, uncertainty arises in two different ways: Imperfect knowledge about the data-generating process ("Epistemic Uncertainty"), and irreducible noise in the data-generating process ("Aleatoric Uncertainty"). <br><br> In the classification setting, we use a function $$\mathcal{l}(\hat{\theta}, y)$$ to score the loss associated with the predicted probabilities for each class, $$\hat{\theta}$$, given the one-hot encoding of the true class, $$y$$. The aleatoric uncertainty can be defined as the expected loss under our predicted class probabilities which come from the learned model posterior $$Q$$\[1\]: 
$$\begin{equation} AU(Q) = \mathbb{E}_{\hat{\theta} \sim Q}\left[\mathbb{E}_{y \sim \hat{\theta}}[\mathcal{l}(\hat{\theta}, y)]\right]. \end{equation}$$
In words, aleatoric uncertainty quantifies the inherent uncertainty in the distribution over classes, under the model posterior[^1]. For example, if we use the negative log-likelihood $$\mathcal{l}(\hat{\theta}, y) = -\log(\hat{\theta}^Ty)$$, $$AU(Q)$$ is the expected entropy of $$\hat{\theta}$$.<br><br>
We can get an unbiased estimator for the aleatoric uncertainty using Monte Carlo simulation:
$$\begin{equation}
\widehat{AU(Q)} = \frac{1}{N}\sum_{i=1}^N S(\theta^{(i)}),
\end{equation}$$
where $$S(\cdot)$$ denotes, as our running example, the Shannon entropy. <br><br> 
The epistemic uncertainty now is defined as the expected loss incurred by the use of the averaging estimator $$\bar{\theta}$$[^2]:
$$\begin{equation} EU(Q) = \mathbb{E}_{Q}\left[\mathbb{E}_\hat{\theta}[\mathcal{l}(\hat{\theta)] \right]- AU(Q) \end{equation}$$
In the case of our running example, where $$\mathcal{l}$$ is the negative log-likelihood, $$EU(Q)$$ is the expected Kullback-Leibler divergence from $$\bar{\theta}$$ to $$\hat{\theta}$$:

[^1]: This definition of aleatoric uncertainty has been criticized because it will be affected by the learner's inability to assign $$0$$ probability mass to $$\theta$$'s which have a different entropy than the true $$\theta$$. Most often, the true $$\theta$$ will have a comparably low entropy and we will therefore generally overestimate the true amount of irreducible noise in the data-generating process, as has been empirically demonstrated in [2]. However, the use of this definition remains standard practice.
[^2]: This too has been criticized, and this too remains standard practice.
