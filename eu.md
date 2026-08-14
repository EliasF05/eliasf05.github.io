---
layout: default
title: Towards better Estimators of Epistemic Uncertainty
description: Personal Project
--- 

<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

**[Home](index.md)** | **[Research & Projects](projects.md)** | **[CV](cv.md)** | **[Contact](contact.md)**
---
In machine learning, uncertainty arises in two different ways: Imperfect knowledge about the data-generating process ("Epistemic Uncertainty"), and irreducible noise in the data-generating process ("Aleatoric Uncertainty"). <br><br> In the classification setting, we use a function $$\mathcal{l}(\hat{\theta}, y)$$ to score the loss associated with the predicted probabilities for each class, $$\hat{\theta}$$, given the one-hot encoding of the true class, $$y$$. The aleatoric uncertainty can be defined as the expected loss under our predicted class probabilities which come from the learned model posterior $$Q$$\[1\]: 
$$AU(Q) = \mathbb{E}_{\hat{\theta} \sim Q}\left[\mathbb{E}_{y \sim \hat{\theta}}[\mathcal{l}(\hat{\theta}, y)]\right]$$
In words, aleatoric uncertainty quantifies the inherent uncertainty in the distribution over classes. For example, if we use the negative log-likelihood, $$\mathcal{l}(\hat{\theta}, y) = -\log(\hat{\theta}^Ty)$$, $$AU(Q)$$ is the expected entropy of $$\hat{\theta}$$.<br><br>
We can get an unbiased estimator for the aleatoric uncertainty using Monte Carlo simulation:
$$\widehat{AU(Q}} = \frac{1}{N}\sum_{i=1}^N S(\widehat{\theta^{(i)}}),$$
where $$S(\cdot)$$ denotes, as our running example, the Shannon Entropy.
