## Low-pass Filtering and Smoothing

Reference: [Low-pass Filtering and Smoothing](http://fourier.eng.hmc.edu/e161/lectures/smooth_sharpen/index.html)

### 1. Spatial averaging (low-pass filtering)

The challenge of suppressing and removing such noise is to distinguish between this type of isolated and out-of-range noise and other legitimate signal components such as the details, edges, lines, small features, etc. in the image, as both correspond to high frequency components in the image data, so that we can preserve the former while suppressing the latter.

One way to remove such noise is to replace each pixel by the **average** of the pixels in a small neighborhood so that the noise of out-of-range gray levels are reduced. Equivalently, this *averaging operation* in spatial domain corresponds to *low-pass filtering* in the spatial frequency domain, by which the high-frequency components are removed.

The **averaging operation** is a weighted sum of the pixels in a small neighborhood, typically of *odd* size in each dimension, i.e.,  $(2k+1)\times (2k+1)$ such as $3 \times 3$, $5 \times 5$, $7 \times 7$, although even-sized neighborhood can also be used. Such operation is typically implemented as a *convolution* with a symmetric kernel $w[m,n]=w[-m,-n]$ of certain shapes and values:
$$
\begin{align} y[m,n] & = \sum \sum_{-k\le i,j \le k} w[i,j]\;x[m-i,n-j] \\ & =\sum \sum_{-k\le i,j \le k} w[-i,-j]\;x[m+i,n+j] \\ & = \sum \sum_{-k\le i,j \le k} w[i,j]\;x[m+i,n+j] \end{align}
$$
where $w[i,j]=w[-i,-j]$ is a symmetric kernel of a limited size (typically odd). Some common kernels are shown below:
$$
w_1 = \frac14 \begin{bmatrix} 1 & 1 \\ 1 & 1 \end{bmatrix}
$$

$$
w_2 = \frac16 \begin{bmatrix} 0 & 1 & 0 \\ 1 & 2 & 1 \\ 0 & 1 & 0 \end{bmatrix}
$$

$$
w_3 = \frac19 \begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{bmatrix}
$$

$$
w_4 = \frac1{10} \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 1 \\ 1 & 1 & 1 \end{bmatrix}
$$

$$
w_5 = \frac1{16} \begin{bmatrix} 1 & 2 & 1 \\ 2 & 4 & 2 \\ 1 & 2 & 1 \end{bmatrix}
$$

The weighting of these convolution kernels is based on the assumption of **spatial locality**, i.e., the closer two pixels are, the more they are correlated. Consequently, closer neighbors of a pixel should be weighted more than those that are farther away.

These convolution kernels can be considered as the approximations of some continuous 2D functions such as a rectangular function 

$$
\mbox{rect} (x,y) = \left\{ \begin{array}{ll} 1 & \mbox{if $\vert x \vert < a$ and $\vert y \vert<b$} \\ 0 & \mbox{else} \end{array} \right.
$$
and a Gaussian function
$$
\mbox{Gauss}(x,y)=e^{-\frac{x^2+y^2}{2\sigma^2}}
$$
Convolution with such functions in spatial domain $(x,y)$ is equivalent to the filtering in spatial frequency domain $(f_x,f_y)$ with the Fourier transforms of the kernel functions:
$$
\mbox{Rect}(f_x,f_y)=\frac{\sin(2\pi a f_x)}{\pi f_x}\frac{\sin(2\pi a f_y)}{\pi f_y}
$$
<img src='./pictures/08-sinc_2d.gif' width=60%>

and
$$
\mbox{Gauss}(f_x,f_y)=\frac{1}{2\pi\sigma^2}e^{-\frac{(f_x^2+f_y^2)}{2\sigma^2}}
$$
<img src='./pictures/08-gaussian_2d.gif' width=60%>

In general the Gaussian function is a preferred method for the following reasons:

- Rotational symmetric with no bias on any particular direction;
- Closer neighbors contribute more (larger weights) than those farther away;
- The width of the function can be easily controlled;
- Fourier transform of Gaussian is also Gaussian so that the filtering in frequency domain is smooth.

The algorithms below represent various efforts made to distinguish noise components from legitimate signal components.

#### 1.1 Weymouth-Overton's algorithm

This algorithm is based on the idea that in the averaging process, greater weight should be assigned to pixels that are

- closer to the pixel in question (in the center of the neighborhood) in distance, and
- more similar to the pixel in gray scale value.

as such a pixel is more relevant and its value is more reliable. Other pixels that are more different from the center pixel in both distance and gray scale value should be weighted less. Correspondingly the weight has two factors,  $w_d$ for the *distance* and $w_v$ for the *pixel value*:

- Define weights for each pixel $x[i,j]$ in the neighborhood $W$ of the pixel $x[m,n]$ under consideration. The *distance weight* is defined as:
  $$
  w_d[i,j]=\frac{1}{1+\sqrt{(m-i)^2+(n-j)^2}}
  $$
  and the *value weight* is defined as:
  $$
  w_v[i,j]=\frac{1}{1+\left[x[i,j]-x[m,n]\right]^\alpha}
  $$
  where $\alpha$ is a user specified parameter. The overall weight for pixel  $x[i,j]$ is defined as
  $$
  w[i,j]=w_d[i,j]\; w_v[i,j]
  $$
  Pixels closer to $x[m,n]$ in both distance and gray scale value will be assigned larger weights.

- Replace $x[m,n]$ by
  $$
  y[m,n]=\frac{\sum_{(i,j)\in W} w[i,j] x[i,j]}{\sum_{(i,j)\in W} w[i,j] }
  $$
  where the denominator is for normalization.

#### 1.2 Statistical thresholding

First find the mean $\mu$ and the variance $\sigma^2$ of all pixels in the neighborhood $W$ of a given pixel $x[m,n]$ of the input image:
$$
\begin{align} \mu & =\frac{1}{N} \sum_{(i,j) \in W} x[i,j] \\\sigma^2 & =\frac{1}{N} \sum_{(i,j) \in W} [x[i,j]-\mu]^2 \end{align}
$$
where $N$ is the number of pixels in the neighborhood $W$.

Then find the corresponding pixel for the output image:
$$
y[m,n]=\left\{ \begin{array}{ll} x[m,n] & \mbox{if $\vert x[m,n] - \mu\vert < \sigma T $} \\ \mu & \mbox{else}
\end{array} \right.
$$
where $T$ is a user specified threshold value.

#### 1.3 Nagao's algorithm

- Define a set of $K$ neighborhoods around the pixel $x[m,n]$ under consideration, for example, the eight $3 \times 3$ neighborhoods in N, NE, E, SE, S, SW, W, and NW directions. The pixel $x[m,n]$ should be included as a corner in each of these neighborhoods.

- Find the mean $\mu_i$ and variance $\sigma_i^2$ for each of the $K$ neighborhoods.

- Replace pixel $x[m,n]$ by the mean value of the neighborhood having the smallest  $\sigma^2$ and therefore most reliable:
  $$
  y[m,n]=\mu_i, \;\;\;\;\;\mbox{iff}\;\;\;\;\;
  \sigma_i^2 = \min\{\sigma_1^2,\cdots,\sigma_K^2\}
  $$
  <img src='./pictures/08-Nagao_example.gif' width=60%>

In this example, the pixel in question $x[m,n]=5$ is replaced by the mean 7.1 from the northeast neighborhood with minimum variance $105$.

### 2. Median Filter

Neighborhood averaging can suppress isolated out-of-range noise, but the *side effect* is that it also blurs sudden changes such as line features, sharp edges, and other image details all corresponding to high spatial frequencies.

The *median filter* is an effective method that can, to some extent, distinguish out-of-range isolated noise from legitimate image features such as edges and lines. Specifically, the **median filter** replaces a pixel by the median, instead of the average, of all pixels in a neighborhood $W$

$$
y[m,n] = \mbox{median} \{ x[i,j], (i,j) \in W \}
$$
where $W$ represents a neighborhood defined by the user, centered around location $[m,n]$ in the image.

**Example**:

This figure shows the comparison of the smoothing results of a **1D signal** (<font color='blue'>blue</font>, 1st column) by an **average filter** (<font color='red'>red</font>, 2nd column) and by a **median filter** (<font color='red'>red</font>, 3rd column) both of size five.

It is clear that *the average filter always blurs edges without completely suppressing noise*, while *the median filter can preserve sharp edges while completely suppressing isolated out-of-range noise, based on the size of the feature*. If its size is less than half of the filter size, indicating the pixel is likely to be an out-of-range noise, it is replace by the median. However, if the size is greater than half of the window, large enough to represent some legitimate feature in the image, it is preserved.

<img src='./pictures/08-median_filter.gif' width=60%>