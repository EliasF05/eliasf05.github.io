---
layout: default
title: Reconstruction of Quantized Time Series
description: Bachelor's Thesis
--- 

**[Home](index.md)** | **[Projects](projects.md)** | **[CV](cv.md)** | **[Contact](contact.md)**
---

Time series are sometimes quantized, for example in order to save memory. For coarse quantization, this means that we lose quite a lot of information - or does it?
Attempting to reconstruct the blue curve using only the orange curve seems like an ill-defined problem, and in general, it is. But practically, we might be able to do quite well.  \
Analogously to the need of an inductive bias in machine learning, reconstruction requires a bias too: Given the quantized signal, there is an infinite number of possible reconstructions, and we need to specify a preference for some of them over others. We can do this by assuming a model class for the original time series, for example an autoregressive model:


