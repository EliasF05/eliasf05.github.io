---
layout: default
title: Reconstruction of Quantized Time Series
description: Bachelor's Thesis
--- 

<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

**[Home](index.md)** | **[Research & Projects](projects.md)** | **[CV](cv.md)** | **[Contact](contact.md)**
---

Below is a quantized time series - meaning that at each time point/sampling interval, its current value was rounded. This project was about estimating the original, unquantized time series, using only the quantized time series that you see below.<br><br>
Click once to overlay the original time series, then click again to overlay the estimate.

<div align="center">
  <div id="thesis-graphic-container" style="position: relative; width: 80%; cursor: pointer; display: inline-block;">
    <!-- State 0: Quantized Only -->
    <img id="graphic-state-0" src="before.jpg" alt="Quantized time series" style="width: 100%; display: block;">
    
    <!-- State 1: Quantized + Original (Hidden by default) -->
    <img id="graphic-state-1" src="before_peak.jpg" alt="Quantized with original overlay" style="width: 100%; position: absolute; top: 0; left: 0; display: none;">
    
    <!-- State 2: Quantized + Original + Estimate (Hidden by default) -->
    <img id="graphic-state-2" src="after.jpg" alt="Quantized with original and estimated overlay" style="width: 100%; position: absolute; top: 0; left: 0; display: none;">
  </div>
  
  <p id="graphic-caption" style="font-style: italic; color: #555; margin-top: 8px;">
    Showing: Quantized Time Series (Click image to overlay original data)
  </p>
</div>

<script>
  (function() {
    var container = document.getElementById('thesis-graphic-container');
    var caption = document.getElementById('graphic-caption');
    var states = [
      document.getElementById('graphic-state-0'),
      document.getElementById('graphic-state-1'),
      document.getElementById('graphic-state-2')
    ];
    
    var captions = [
      "Showing: Quantized Time Series (Click image to overlay original data)",
      "Showing: Original Time Series Overlay (Click again to overlay the estimate)",
      "Showing: Final Estimate Overlay (Click to reset)"
    ];
    
    var currentState = 0;
    
    container.addEventListener('click', function() {
      // Hide the current state layer (if it's an overlay layer)
      // Or manage visibility strictly:
      currentState = (currentState + 1) % 3;
      
      if (currentState === 0) {
        states[1].style.display = 'none';
        states[2].style.display = 'none';
      } else if (currentState === 1) {
        states[1].style.display = 'block';
        states[2].style.display = 'none';
      } else if (currentState === 2) {
        states[1].style.display = 'none'; // hide middle if 'after.jpg' includes both
        states[2].style.display = 'block';
      }
      
      caption.textContent = captions[currentState];
    });
  })();
</script>
