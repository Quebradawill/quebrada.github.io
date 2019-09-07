---
layout: post
title: Computer Image Processing and Analysis (E161)  - Harvey Mudd College
date: 2019-03-15
categories: Image-Processing
tags: Low-Level-Image-Processing
status: publish
type: post
published: true
author: Quebradawill
---

## Computer Image Processing and Analysis (E161)  - Harvey Mudd College

From: [http://fourier.eng.hmc.edu/e161/lectures/](http://fourier.eng.hmc.edu/e161/lectures/)

### 1. Introduction

### 2. Visual Perception

### 3. Sampling Theory

#### 3.1 Fourier expansion and Fourier transform 

**Fourier expansion**

Let $$x_T (t)$$ represent a continuous periodic function


$$
x_T (t + mT) = x_T(t), \quad (m = 0, \pm1, \pm2, \cdots )
$$


with its period equal to $$T$$. Its <font color="red">Fourier expansion</font> is


$$
x_T(t) = \sum_{n=-\infty}^{\infty} X(n) e^{j 2\pi n f_0 t}
$$


where $$f_0 = 1/T$$ and $$X(n), (n = 0, \pm 1, \pm 2, \cdots)$$ are the *expansion coefficients* defined as


$$
X (n) = \frac1T \int_{-T/2}^{+T/2} x_T(t) e^{- j 2\pi n f_0 t} dt
$$


These coefficients can also be considered as the discrete, non-periodic spectrum of $$x_T (t)$$


$$
X(f) = \sum_{n=-\infty}^{\infty} X(n) \delta (f - n f_0)
$$


where $$f_0 = 1/T$$ is the interval between two neighboring frequency components.

**Fourier transform of Non-periodic, continuous function**

The Fourier of a non-periodic, continuous function $$x(t)$$ is


$$
x(t)=\int_{-\infty}^{\infty} X(f) e^{j2\pi ft} df
$$


where $$X(f)$$ is the non-periodic continuous spectrum


$$
X(f)=\int_{-\infty}^{\infty} x(t) e^{-j2\pi ft} dt
$$


These two equations are a Fourier transform pair (inverse and forward).

**The convolution theorem**

The convolution of two functions $$x(t)$$ and $$y(t)$$ is defined as 


$$
x(t)*y(t) \stackrel{\triangle}{=} \int_{-\infty}^{\infty} x(\tau) y(t-\tau) d\tau = \int_{-\infty}^{\infty} y(\tau) x(t-\tau) d\tau
$$


It can be shown that the spectrum of $$x(t)*y(t)$$ is the product of their spectra $$X(f)$$ and $$Y(f)$$:


$$
\int_{-\infty}^{\infty} [x(t)*y(t)] e^{-j2\pi ft} dt = X(f) Y(f)
$$


Also, the spectrum of the product of the two functions is the convolution of their spectra:


$$
\int_{-\infty}^{\infty} [x(t) \, y(t)] e^{-j2\pi ft} dt = X(f)*Y(f)
$$

### 4. Digital Images

### 5. Convolution Theory

The <font color="red">convolution</font> of two continuous signals $$x(t)$$ and $$h(t)$$ is defined as 


$$
y(t)=h(t)*x(t) \stackrel{\triangle}{=} \int_{-\infty}^{\infty} x(\tau) h(t-\tau) d\tau = \int_{-\infty}^{\infty} h(\tau) x(t-\tau) d\tau = x(t)*h(t)
$$


i.e., convolution is commutative. Also convolution is associative: 


$$
h*(g*x)=(h*g)*x
$$


Typically, $$y(t)$$ is the output of a system characterized by its impulse response function $$h(t)$$ with input $$x(t)$$.

<img src="https://github.com/quebrada/quebrada.github.io/blob/master/pictures/20190315-ConvolutionEx1.gif?raw=true" align="center" width="70%">

Convolution in discrete form is 


$$
y[n]=\sum_{m=-\infty}^{\infty} x[n-m] \; h[m] =\sum_{m=-\infty}^{\infty} h[n-m]\;x[m]
$$


If $$h[m]$$ is finite, e.g., 


$$
h[m] = \left\{ \begin{array}{ll} h[m] & \vert m\vert\le k \\ 0 & \vert m\vert>k \end{array} \right.
$$


the convolution becomes 


$$
y[n]=\sum_{m=-k}^{k} x[n-m] \; h[m]
$$


If the system in question were a causal system in time domain


$$
h[n)=h[n]u[n],\;\;\;\;\mbox{i.e.},\;\;\;\;h[n]=0\;\;\;\mbox{ if $n<0$}
$$


the above would become


$$
y[n]=\sum_{m=0}^{k} x[n-m] \; h[m]
$$


However, in image processing, we often consider convolution in spatial domain where causality does not apply.

### 6. Image Resizing

### 7. Graylevel Transform

### 8. Smoothing and Noise Reduction

### 9. Sharpening and Edge Detection

### 10. Edge Detection Methods

### 11. Vector Space and Orthorgonal Transforms

### 12. Fourier Transform

### 13. Walsh-Hadamard Transform

### 14. Discrete Cosine Transform

### 15. Haar Transform

### 16. Principal Component Transform

### 17. Sigular Value Decomposition

### 18. Wavelet Transform

### 19. Filter Banks

### 20. Color Perception

### 21. Color Image Processing

### 22. Motion Restoration

### 23. Image Compression

### 24. Hough Transform

### 25. Mathemtical Morphology

### 26. Fourier Descripter

### 27. Template Match

### 28. Pattern Classification

### 29. Bayesian Inference and Expectation Maximization

### 30. Neural Networks

### 31. Back Propagation Network

### 32. Support Vector Machines

### 33. Kernel PCA

### 34. Independent Component Analysis

### 35. Gaussian Process

### 36. Review of Linear Algebra

### 37. Review of Probability I (univariate)

### 38. Review of Probability II (multivariate)

