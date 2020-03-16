---
layout: post
title: HMC - E161 - 10 - Edge Detection Methods
date: 2019-07-07
categories: Image-processing
tags: HMC-Image-Processing
status: publish
type: post
published: True
author: Quebradawill
---

## Edge Detection Methods

Reference: [Edge Detection Methods](http://fourier.eng.hmc.edu/e161/lectures/canny/index.html)

### 1. Canny Edge Detection

Canny edge detection is a multi-step algorithm that can detect edges with noise suppressed at the same time.

1. **Smooth the image** with a *Gaussian filter* to reduce noise and unwanted details and textures.
   $$
   g(m,n)=G_{\sigma}(m,n)*f(m,n)
   $$
   where
   $$
   G_{\sigma}=\frac{1}{\sqrt{2\pi\sigma^2}} \exp \left(-\frac{m^2+n^2}{2\sigma^2}\right)
   $$

2. **Compute gradient** of $g(m,n)$ using any of the gradient operators (*Roberts*, *Sobel*, *Prewitt*, etc.) to get:
   $$
   M(n,n)=\sqrt{g_m^2(m,n)+g_n^2(m,n)}
   $$
   and
   $$
   \theta(m,n) = \tan^{-1} \left( \frac{g_n(m,n)}{g_m(m,n)} \right)
   $$

3. **Threshold $M$**:
   $$
   M_T(m,n)=\left\{ \begin{array}{ll} M(m,n) & \mbox{if $M(m,n)>T$} \\ 0 & \mbox{otherwise} \end{array} \right.
   $$
   where $T$ is so chosen that all edge elements are kept while most of the noise is suppressed.

4. **Suppress non-maxima pixels** in the edges in $M_T$ obtained above to thin the edge ridges (as the edges might have been broadened in step 1). To do so, check to see whether each non-zero $M_T(m,n)$ is greater than its two neighbors along the gradient direction $\theta(m,n)$. If so, keep $M_T(m,n)$ unchanged, otherwise, set it to 0.

5. **Threshold the previous result** by two different thresholds $\tau_1$ and $\tau_2$ (where $\tau_1<\tau_2$) to obtain two binary images $T_1$ and $T_2$. Note that $T_2$ with greater $\tau_2$ has less noise and fewer false edges but greater gaps between edge segments, when compared to $T_1$ with smaller $\tau_1$.

6. **Link edge segments** in $T_2$ to form continuous edges. To do so, trace each segment in $T_2$ to its end and then search its neighbors in $T_1$ to find any edge segment in $T_1$ to bridge the gap until reaching another edge segment in $T_2$.

**Example 1:**

<img src='./pictures/09-panda.gif' width=30%>

Edge detection by gradient operators (*Roberts*, *Sobel* and *Prewitt*):

<img src='./pictures/09-panda_gradient.gif' width=80%>

Edge detection by LoG and DoG:

<img src='./pictures/09-panda_LoG_DoG.gif' width=55%>

Edge detection by Canny method ($\sigma=1,2,3$, $\tau_1=0.3$, $\tau_2=0.7$):

<img src='./pictures/09-panda_canny.gif' width=80%>

**Example 2:**

<img src='./pictures/09-fuzzy_edge_demo.gif' width=25%>

Edge detection results by Sobel, Prewitt gradient operators, by DoG method and by Canny's method ($\sigma=5$, $\tau_1=0.8$, $\tau_2=0.95$):

<center class='half'><img src='./pictures/09-edge_Sobel.gif' width=25%><img src='./pictures/09-edge_Prewitt.gif' width=25%><img src='./pictures/09-edge_DoG.gif' width=25%><img src='./pictures/09-edge_canny.gif' width=25%></center>
### 2. Edge detection with image pyramid

Why can humans perceive edges seemingly effortlessly, while it is usually quite difficult for an edge detection algorithm to find them? One reason is that human vision has access to the *global information* while the gradient operator only has *local information* contained in a $3 \times 3$ or $5 \times 5$ neighborhood. The difference is probably best appreciated by thinking of a mouse in a maze and a bird looking down from the sky.

Here is a method that *combines both global and local information* to detect edges while suppressing noise (textures, etc.) at the same time.

1. **Build an image pyramid.** Assume the size of the given image is $N$ by $N$ where $N=2^n$. The *image pyramid* is a hierarchical structure composed of $n$ levels of the same image of different resolutions. At the bottom of the pyramid is the given full size image. Each set of $2 \times 2=4$ neighboring pixels is replaced by their <font color='blue'>average</font> as the pixel value of the image at the next level. This process, which reduces the image size by half, is repeated $n=\log_2 N$ times until finally an image of only 1 pixel (average of entire image) is generated as the top of the pyramid.

   <img src='./pictures/10-image_pyramid.gif' width=50%>

2. **Find a proper level in the pyramid** where noise is sufficiently suppressed while major edges are still represented. For each edge element, a pixel with its gradient greater than a threshold, search its 4 children in the next level to identify finer edge elements. Do this recursively until finding edges with fine details but without interference of noise.

### 3. Gaussian-Laplacian Pyramid Image Coding

- **Down-sampling**:

  - Convolution with Gaussian (low-pass):
    $$
    f'_{l+1}=g * f_l
    $$
    All images so obtained by Gaussian (low-passed) filtering form a *Gaussian pyramid*.

  - Difference image (band-pass):
    $$
    h_{l+1}=f'_l-f'_{l+1}
    $$
    Note that image $h_{l+1}=f'_l-f'_{l+1}$ is the difference between two images convolved by gaussian kernels of difference sizes, and is a bandpass filtered image. All images so obtained form a *Laplacian pyramid*.

  - Down-sampling of $f'_{l+1}$:
    $$
    f_{l+1}[n]=f'_{l+1}[2n]
    $$

  <img src='./pictures/10-Laplacian_pyramid_1.gif' width=60%>

  <img src='./pictures/10-down_sampling.gif' width=60%>

- **Feature Extraction**

- **Image Compression**

- **Up-sampling**:

  - Interlacing of zeros (rows and columns)
    $$
    f'_l[n]=\left\{ \begin{array}{ll} f_l[n/2] & n=0,2,4,\cdots \\ 0 & \mbox{otherwise} \end{array} \right.
    $$

  - Convolution with Gaussian (low-pass):
    $$
    f_{l-1}=g*f'_l+h_l
    $$

<img src='./pictures/10-Laplacian_pyramid_2.gif' width=60%>
<img src='./pictures/10-Image_Pyramid.gif' width=50%>
<img src='./pictures/10-laplaciangaussian.gif' width=70%>
<img src='./pictures/10-laplaciangaussian1.gif' width=70%>
<img src='./pictures/10-pyramid1.gif' width=80%>
<img src='./pictures/10-pyramid2.gif' width=80%>