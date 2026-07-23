---
layout: default
title: Reconstruction of Quantized Time Series
description: Bachelor's Thesis
--- 

<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

**[Home](index.md)** | **[Projects](projects.md)** | **[CV](cv.md)** | **[Contact](contact.md)**
---

This article is 100% human-written.<br><br>
**Problem Statement** <br>
Time series are sometimes quantized, for example in order to save memory. For coarse quantization, this means that we lose quite a lot of information - or does it?
<p align="center">
  <img src="quantizedARExampleBlog.jpg" alt="Reconstruction Plot" width="80%">
</p>
Attempting to reconstruct the blue curve using only the orange curve seems like an ill-defined problem, and in general, it is. But practically, we might be able to do quite well.<br><br>
**Reconstruction Bias** <br>
Analogously to the need of an inductive bias in machine learning, reconstruction requires a bias too: Given the quantized signal, there is an infinite number of possible reconstructions, so we need to specify a preference for some of them over others. We can do this by assuming a model class for the original time series, for example an autoregressive model:\begin{align}x_t = \sum_{i=1}^p \phi_i x_{t-i}+w_t, \quad w_t \sim \mathcal{N}(0, \sigma^2)\end{align}
Marginalizing over all possible autoregressive models will surely be intractable, so we need to use a point estimate of $$p$$, $$\{\phi_i\}$$, and $$\sigma^2$$. We can then use this point estimate to estimate $$x_{1:T}$$ from $$y_{1:T}$$ using state estimation methods. For now, it will be useful to assume that we have an estimate of $$p$$, $$\{\phi_i\}$$, and $$\sigma^2$$, and focus on the state estimation step. We will see how we can turn the ability to do state estimation into a parameter estimation algorithm afterwards. <br><br> 

**State Estimation**<br>
State estimation is the task of inferring the hidden, unobserved state $$x_t$$ of a system (e.g. the original values), using observations $$y_{1:T}$$ (e.g. the quantized values). For certain applications, it might be the case that we need to perform state estimation in real-time, meaning that $$T=t$$, as observations from future timepoints are not yet available. Real-time state estimation is often called *filtering*. The general case, where $$T\geq t$$, is called *smoothing*.<br><br>

It turns out that 


