---
layout: post
title: HMC - E161 - 06 - Image Scaling and Rotation
date: 2019-07-07
categories: Image-processing
tags: HMC-Image-Processing
status: publish
type: post
published: True
author: Quebradawill
---

## Image Scaling and Rotation

Reference: [Image Scaling and Rotation](http://fourier.eng.hmc.edu/e161/lectures/resize/index.html)

### 1. Enlargement

The size of a given image can be easily enlarged by an integer scale factor (2, 3, etc.) by repeating each of the pixels in the image. For example, a 2 by 2 image can be doubled by 

$$
f_{2\times 2} = \begin{bmatrix} 1 & 3 \\ 4 & 5 \end{bmatrix} \Longrightarrow f_{4\times 4} = \begin{bmatrix} 1 & 1 & 3 & 3 \\  1 & 1 & 3 & 3 \\  4 & 4 & 5 & 5 \\  4 & 4 & 5 & 5 \end{bmatrix}
$$

Obviously the drawback of this simple method is that it is not flexible in terms of the scaling factor, and the resulting image is likely to look blocky.

This replication can be implemented equivalently by this two-step procedure:

- **Zero interlacing**
  $$
  f_{2\times 2} = \begin{bmatrix} 1 & 3 \\ 4 & 5 \end{bmatrix} \Longrightarrow f_{4\times 4}^{'} = \begin{bmatrix} 1 & 0 & 3 & 0 \\  0 & 0 & 0 & 0 \\  4 & 0 & 5 & 0 \\  0 & 0 & 0 & 0 \end{bmatrix}
  $$

- **Convolution** with kernel

  $$
  H_{2 \times 2} = \begin{bmatrix} 1 & 1 \\ 1 & 1 \end{bmatrix}
  $$
  to get
  $$
  f_{4 \times 4}^{'} * H = f_{4 \times 4}
  $$

An obvious problem of enlargement by replication is that the resulting image looks blocky, which can be avoided by using linear interpolation:

$$
f_{2\times 2} = \begin{bmatrix} 1 & 3 \\ 4 & 5 \end{bmatrix} \Longrightarrow f_{4\times 4} = \begin{bmatrix} 1 & 2 & 3 & 1.5 \\ 2.5 & 3.25 & 4 & 2 \\  4 & 4.5 & 5 & 2.5 \\  2 & 2.25 & 2.5 & 1.25 \end{bmatrix}
$$
This operation is called *bilinear interpolation* (two-dimensional linear interpolation) which can be implemented equivalently by this two-step procedure:

- **Zero interlacing**
  $$
  f_{2\times 2} = \begin{bmatrix} 1 & 3 \\ 4 & 5 \end{bmatrix} \Longrightarrow f_{4\times 4}^{'} = \begin{bmatrix} 1 & 0 & 3 & 0 \\  0 & 0 & 0 & 0 \\  4 & 0 & 5 & 0 \\  0 & 0 & 0 & 0 \end{bmatrix}
  $$

- **Convolution** with kernel
  $$
  H_{3 \times 3} = \frac14 \begin{bmatrix} 1 & 2 & 1 \\ 2 & 4 & 2 \\ 1 & 2 & 1 \end{bmatrix}
  $$
  to get
  $$
  f'_{4 \times 4} * H_{3 \times 3} = f_{4 \times 4}
  $$

Note that the convolution assumes zero pixels outside the image. The resulting image looks smooth instead of blocky.

### 2. Reduction

Image size can be easily reduced by subsampling, e.g., getting rid of every other pixel in each row and column:

$$
f_{4 \times 4} = \begin{bmatrix} 1 & 2 & 4 & 3 \\ 2 & 99 & 3 & 8 \\ 1 & 4 & 3 & 2 \\ 3 & 2 & 1 & 4 \end{bmatrix} \Longrightarrow \begin{bmatrix} 1 & 4 \\ 1 & 3 \end{bmatrix} \; \textrm{or} \; \begin{bmatrix} 2 & 3 \\ 4 & 2 \end{bmatrix} \; \textrm{or} \; \begin{bmatrix} 2 & 3 \\ 3 & 1 \end{bmatrix} \; \textrm{or} \; \begin{bmatrix} 99 & 8 \\ 2 & 4 \end{bmatrix}
$$
In any of the four possible subsampling cases, three fourths of the information contained in the original image is lost. A better way is to find the average of a $2 \times 2$ neighborhood as the resulting pixel:

$$
\begin{bmatrix} 1 & 2 & 4 & 3 \\ 2 & 99 & 3 & 8 \\ 1 & 4 & 3 & 2 \\ 3 & 2 & 1 & 4 \end{bmatrix} \Longrightarrow \begin{bmatrix} 26 & 4.5 \\ 2.5 & 2.5 \end{bmatrix}
$$
Again, this operation can be implemented in a two-step process:

- **Regional averaging** by convolving with
  $$
  H_{2 \times 2} = \frac14 \begin{bmatrix} 1 & 1 \\ 1 & 1 \end{bmatrix}
  $$
  to get
  $$
  f_{4 \times 4} * H_{2 \times 2} = f_{4 \times 4}^{'} = \begin{bmatrix} 26 & 27 & 4.5 & 2.75 \\ 26.5 & 27.23 & 4.0 & 2.5 \\ 2.5 & 2.5 & 2.5 & 1.5 \\ 1.25 & 0.75 & 1.25 & 1.0 \end{bmatrix}
  $$

- **Subsampling** $f'_{4 \times 4}$ to get
  $$
  \begin{bmatrix} 26 & 4.5 \\ 2.5 & 2.5 \end{bmatrix}
  $$

### 3. Arbitrary resizing

It is obviously more desirable to arbitrarily resize a given image (enlarge or reduce the image proportionally or non-proportionally). We first consider converting a one-dimensional $m$-sample input $\{ x_i, (i=0, 1, \cdots, m-1) \}$ into an $n$-sample output $\{ y_j, (j=0, 1, \cdots, n-1) \}$, where $n$ is the desired size of the output, which may be either smaller or greater than $m$, i.e., the scaling factor $n/m$ can be either greater or smaller than 1 (for either enlargement or reduction). The method is essentially a two-step process of **linear interpolation** and **re-sampling**.

![](/pictures/20190707-06-interpolate_0d.gif width=66%)

- **Convert indices**:

  Represent each index $0 \leq k \leq n-1 $ of the output as a floating point number $p$ in the range of $0 \leq i \leq m-1 $ of the input indices:
  $$
  \frac{p}{m-1}=\frac{k}{n-1},\;\;\;\;\;\mbox{i.e.}\;\;\;\; p=k\; \frac{m-1}{n-1}
  $$
  The indices of the two neighboring samples of $p$ can be found as
  $$
  i = \lfloor p \rfloor \;\;\leq\;\; p \;\;\leq \;\;\lceil p \rceil =i+1
  $$
  where $\lfloor p \rfloor$ and $\lceil p \rceil$ represent, respectively, the floor and the ceiling of $p$, i.e., the largest integer smaller than $p$ and the smallest integer larger than $p$.

- **Re-sampling**:

  Find the fraction $0 \leq (u=p-i) \leq 1$ and $d$ as shown in the figure:
  $$
  \frac{d}{u}=\frac{x_{i+1}-x_{i}}{(i+1)-i}=x_{i+1}-x_i,\;\;\;\;\mbox{i.e.,}
  \;\;\;\;\;\;d=u(x_{i+1}-x_i) 
  $$
  Now the $k$-th value $y_k$ of the output can be found as the interpolation between $x_i$ and $x_{i+1}$: 
  $$
  y_k=x_{p}=x_i+d=x_i+u(x_{i+1}-x_i)
  $$

![](/pictures/20190707-06-interpolate_1d.gif width=35%)

This method of linear interpolation can be generalized from 1-D to 2-D bilinear interpolation for image resizing.

- **Convert indices**:

  Similar to the 1D case, we first convert the integer indices $(k, l)$ of each pixel of the output image of the desired size into real coordinates $(p, q)$ in the range of the input image. Then the corresponding fractions $u$ and $v$ in both horizontal and vertical dimensions can be found:
  $$
  i = \lfloor p \rfloor, \;\;\;\;\; i+1=\lceil p \rceil, \;\;\;\;\; u=p-i
  $$

  $$
  j = \lfloor q \rfloor, \;\;\;\;\; j+1=\lceil q \rceil, \;\;\;\;\; v=q-j
  $$

- **Re-sampling**:

  Find value $x[p,q]$ as the bilinear interpolation of its four immediate neighbors in the input image, represented by
  $$
  a=x_{i,j},\;\;\;\;\;\; b=x_{i+1,j},\;\;\;\;\;\; c=x_{i,j+1},\;\;\;\;\;\; d=x_{i+1,j+1}
  $$

![](/pictures/20190707-06-interpolate_2d.gif width=50%)

The bilinear interpolation is carried out in two levels of linear interpolations. We first find the two interpolations of $a$, $b$ and $c$, $d$ along the dimension with index $i$: 

$$
\left\{ \begin{array}{ll} e=a+u(b-a) \\  f=c+u(d-c) \end{array} \right.
$$
and then find $y[k,l]=x[p,q]$ as the linear interpolation of $e$ and $f$ along the dimension with index $j$: 
$$
y[k,l]=x[p,q]=e+v(f-e)=(1-u-v+uv)a+u(1-v)b+v(1-u)c+uvd
$$
Alternatively and equivalently, we could first find the interpolations along the dimension in $j$: 
$$
\left\{ \begin{array}{ll} g=a+v(c-a) \\ h=b+v(d-b) \end{array} \right.
$$
and then find the pixel value of the output as the interpolation along the dimension in $i$: 
$$
y[k,l]=x[p,q]=g+u(h-g)=(1-u-v+uv)a+u(1-v)b+v(1-u)c+uvd
$$
The result is exactly the same as before.

![](/pictures/20190707-06-LennaScale.gif width=70%)

### 4. Arbitrary rotation

We first note that rotating the input image $x$ by an angle $\phi$ is equivalent to rotating the output image $y$ by an angle $-\phi$. For each pixel position  $(k, l)$ of the output $y$ we find the corresponding position in the input $x$: 

$$
\left\{ \begin{array}{l} p=\;\;\;\cos \phi\; k+\sin \phi\; l \\ q=-\sin \phi\; k+\cos \phi\; l \end{array} \right.
$$
This rotation is with respect to the origin of the image, the top left corner of the image. If we want the rotation to be around the center $(x_0=M/2,\;y_0=N/2)$ of an image of size $M\times N$, then
$$
\left\{ \begin{array}{l} p=[\;\;\;\cos \phi\; (k-x_0)+\sin \phi \; (l - y_0) ] + x_0 \\ q = [- \sin \phi \; (k-x_0)+\cos \phi\; (l-y_0)]+y_0 \end{array} \right.
$$
Next we find the interpolation value $x[p,q]$ for each pixel $y[k,l]$ of the output image using the same bilinear interpolation method as in the arbitrary scaling discussed above. Note that some pixels in the image after the rotation may be outside the original image before the rotation, they can be arbitrarily assigned with any color, such as black.

![](/pictures/20190707-06-ImageRotate1.gif width=75%)

![](/pictures/20190707-06-LennaRotate.gif width=60%)