---
layout: post
title: HMC - E161 - 05 - Digital Convolution
date: 2019-07-07
categories: Image-processing
tags: HMC-Image-Processing
status: publish
type: post
published: True
author: Quebradawill
---

## Digital Convolution

Reference: [Digital Convolution](http://fourier.eng.hmc.edu/e161/lectures/convolution/index.html)

The **convolution** of two continuous signals $x(t)$ and $h(t)$ is defined as
$$
y(t)=h(t)*x(t) \stackrel{\triangle}{=} \int_{-\infty}^{\infty} x(\tau) h(t-\tau) d\tau = \int_{-\infty}^{\infty} h(\tau) x(t-\tau) d\tau = x(t)*h(t)
$$
i.e., convolution is *commutative*, also convolution is *associative*: 
$$
h*(g*x)=(h*g)*x
$$
Typically, $y(t)$ is the output of a system characterized by its impulse response function $h(t)$ with input $x(t)$.

![](/pictures/20190707-05-ConvolutionEx1.gif width=70%)

Convolution in *discrete* form is
$$
y[n]=\sum_{m=-\infty}^{\infty} x[n-m] \; h[m] =\sum_{m=-\infty}^{\infty} h[n-m]\;x[m]
$$
If $h[m]$ is finite, e.g., 
$$
h[m] = \left\{ \begin{array}{ll} h[m] & \vert m\vert\le k \\  0 & \vert m\vert>k \end{array} \right.
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

If $h[m]=h[-m]$ is symmetric (almost always true in image processing), then replacing $m$ by $-m$ we get 

$$
y[n]=\sum_{m=-k}^{k} x[n+m]\;h[-m]=\sum_{m=-k}^{k} x[n+m] \; h[m]
$$
We see that now the *convolution* is the same as the *correlation* of the two functions.

If the input $x[m]$ is finite (always true in reality), i.e., 
$$
x[m] = \left\{ \begin{array}{ll} x[m] & 0 \le m <N \\ 0 & \mbox{otherwise} \end{array} \right.
$$
its index $n+m$ in the convolution has to satisfy the following for $x$ to be in the valid non-zero range: 
$$
0 \le n+m \le N-1
$$
or correspondingly, the index $n$ of the output $y[n]$ has to satisfy: 
$$
-m \le n \le N-m-1
$$
When the variable index $m$ in the convolution is equal to $k$, the index of output $y[n]$ reaches its lower bound $n=-k$; when $m=-k$, the index of $y[n]$ reaches its upper bound $n=N+k-1$. In other words, there are $N+2k$ valid (non-zero) elements in the output: 
$$
y[n],\;\;\;\;\;\;(-k \le n \le N+k-1)
$$
Digital convolution can be best understood graphically (where the index of $y[n]$ is rearranged) as shown below:

![](/pictures/20190707-05-digital_convolution.gif width=70%)

Assume the size of the input signal $x[n]$ is $N$ ($n=0,\cdots,N-1$) and the size of $h$ is $M=2k+1$ (usually an odd number), then the size of the resulting convolution $y=x*h$ is $N+M-1=N+2k$. However, as it is usually desirable for the output $y$ to have the same size as the input $x$, we can drop $k$ components at each end of $y$. When the size of $h$ is even, we can drop $k$ components at one end and $k-1$ from the other of $y$.

In particular, if the elements of the kernel are all the same (an *average* operator or a *low-pass* filter), then we can speed up the convolution process while sliding the kernel over the input signal by taking care of only the two ends of the kernel.

In image processing, all of the discussions above for one-dimensional convolution are generalized into two dimensions, and $h$ is called a **convolution kernel**, or **mask**.

![](/pictures/20190707-05-digital_convolution_2D.gif width=65%)