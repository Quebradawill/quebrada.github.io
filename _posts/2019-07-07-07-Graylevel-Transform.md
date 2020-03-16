## Image Enhancement by Contrast Transform

Reference: [Image Enhancement by Contrast Transform](http://fourier.eng.hmc.edu/e161/lectures/contrast_transform/index.html)

### 1. Gray level mapping

#### 1.1 Histogram

In a typical 8-bit image, there are $L=2^8=256$ discrete gray scale levels from 0 to $2^8-1=255=L-1$. The histogram of an image represents the density probability distribution of the pixel values in the image over the entire gray scale range.

#### 1.2 Gray level mapping

The appearance (brightness, contrast, etc.) of an image can be modified according to various needs by a gray level mapping function specified by the user: 

$$
y = f(x)
$$
where $x=x[m,n]$ is a pixel in the input image and $y=y[m,n]$ is the corresponding pixel in the output image. This mapping function can be specified in different ways, such as a piecewise linear function, or based on the histogram of the input image.

#### 1.3 Programming issues

The above mapping functions can be carried out for each of the pixels in the image. However, this is not the most efficient way computationally. A better way for implementing the function mapping is to use a *lookup table* which stores the pre-computed mapping for each of the $L$ gray levels. The gray level of a pixel in the input image is used as the address to the table and the content of the table entry is used as the gray level of the corresponding pixel of the output image. By using the lookup table, the mapping function only needs to be carried out $L$ times, instead of $M\times N$ ($>> L$) times (size of the image).

<img src='./pictures/07-lookup_table.gif' width=60%>

#### 1.4 Common mapping functions

- **Thresholding**:

  As a special case of piecewise linear mapping, thresholding is a simple way to do image segmentation, in particular, when the histogram of the image is bimodal with two peaks separated by a valley, typically corresponding to some object in the image and the background. A thresholding mapping maps all pixel values below a specified threshold to zero and all above to 255.

  <img src='./pictures/07-Thresholding.png' width=60%>

- **Negative image**:
  $$
  y=f(x)=255-x
  $$
  <img src='./pictures/07-NegativeMapping.png' width=60%>

- **Min-max linear stretch**:

  <img src='./pictures/07-gray_mapping_stretch.gif' width=50%>
  $$
  y=f(x)=\left\{ \begin{array}{ll} 0 & x < \min \\ \frac{x-\min}{\max - \min} \; 255 & \min \leq x \leq \max \\ 255 & x > \max \end{array} \right.
  $$
  This is a piecewise linear mapping between the input and output images of three linear segments with slopes 0 for $x<\min$, $255/(\max-\min)>1$ for $\min \le x \le \max$, and 0 for $x>\max$. The greater than 1 slope in the middle range stretches the dynamic range of the image to use all gray levels available in the display.

  <img src='./pictures/07-LinearStretch.png' width=60%>

- **Linear stretch based on histogram**:

  If in the image there are only a small number of pixels close to minimum gray level 0 and the maximum gray level $L-1=255$, and the gray level of most of the pixels are concentrated in the middle range (gray) of the histogram, the above linear stretch method based on the minimum and maximum gray levels has very limited effect (as the slope $(L-1)/(\max-\min)$ is very close to 1).

  In this case we can push a small percentage (e.g., $3\%$, $5\%$) of gray levels at the two ends of the histogram toward 0 and $L-1$.

  <img src='./pictures/07-gray_mapping_cutstretch.gif' width=50%>

  <img src='./pictures/07-LinearStretch1.png' width=60%>

- **Piecewise linear mapping**:

  A mapping function can be specified by a set of $n$ break-points  $(x_i,y_j), i=1,2,\cdots,n$, with neighboring points connected by straight lines, such as shown here:

  <img src='./pictures/07-pwlnr.gif' width=35%>

  For example, to increase the contrast of the image of Paolina, we can linearly stretch the gray scales of the image so that the darkest and brightest gray levels are mapped to 0 and 255, respectively.

### 2. Histogram Equalization

To transform the gray levels of the image so that the histogram of the resulting image is equalized to become a constant: 

$$
h[i]= \mbox{constant},\;\;\;\;\;\;0\le i\le L-1
$$
The purposes:

- to equally make use of all available gray levels in the dynamic range;
- for further histogram specification.

We first assume the pixel values are continuous in the range of $0<x<1$, and the mapping function $y=f(x)$ maps $x$ to $y$ also in the same range. We also assume all pixels within the gray scale interval $dx$ of the input image are mapped to the corresponding range $dy$ of the output image. As the number of pixels being mapped remains unchanged, we have 
$$
p(y)dy=p(x)dx
$$
<img src='./pictures/07-equalize.gif' width=50%>

For the histogram of the output image to be equalized, it needs to be constant 1, i.e., $p(y)=1$, so that 

$$
\int_0^1 p(x)\;dx=\int_0^1 dy=1
$$
We therefore have: 
$$
p(y)dy=dy=p(x)dx,\;\;\;\mbox{or}\;\;\frac{dy}{dx}=p(x)
$$
Integrating both sides, we get the mapping function $y=f(x)$ for the histogram equalization:
$$
y=f(x)=\int_0^x p(u) du=P(x)-P(0)=P(x)
$$
where $P(x)$ is the cumulative distribution of the gray levels of the input image:
$$
P(x)=\int_0^x p(u) du, \;\;\;\;\;P(0)=0,\;\;\;\;\;\;P(1)=1
$$
This histogram equalization mapping can be intuitively interpreted by the following:

- If $p(x)$ is low, $P(x)$ has a shallow slope, $dy$ will be narrow, causing $p(y)$ to be high, to keep $p(y)dy=p(x)dx$;
- If $p(x)$ is high, $P(x)$ has a steep slope, $dy$ will be wide, causing $p(y)$ to be low, to keep $p(y)dy=p(x)dx$.

<img src='./pictures/07-equalize1.gif' width=50%>

For discrete gray levels, the gray level of the input $x$ takes one of the $L$ discrete values: $0\le x < L$, and the continuous mapping function becomes discrete:

$$
y'[j]=H_x[j]\stackrel{\triangle}{=}\sum_{i=0}^j h_x[i]
$$
where $h_x[i]$ and $H_x[j]$ are respectively the density and cumulative histogram of the input image $x$:
$$
h_x[i]=\frac{n_i}{\sum_{i=0}^{L-1} n_i}=\frac{n_i}{N},\;\;\; h_x[j] = \sum_{i=0}^j h_x[i],\;\;\;\;\;\;H_x[L-1]=\sum_{i=0}^{L-1} h_x[i]=1
$$
and $n_i$ is the number of pixels with gray level $i$, and $N$ is the total number of pixels.

The resulting function $y'$ in the range $0 \le y' \le 1$ needs to be converted to the gray levels $0 \le y <L$ by either of the two ways:

1. $y_1=\lfloor (L-1)\;y' +0.5 \rfloor$
2. $y_2=\lfloor (L-1) \;\frac{y'-y'_{\min}}{1-y'_{\min}}+0.5 \rfloor $

where $\lfloor w \rfloor$ is the floor of a real number $w$ (the largest integer smaller than $w$), and adding $0.5$ is for proper rounding. Note that both conversions map $y'_{max}=1$ to the highest gray level $L-1$, but the second conversion also maps $y'_{min}$ to 0 to stretch the gray levels of the output image to occupy the entire dynamic range $0 \le y <L$; i.e., the second method does gray scale stretch as well as histogram equalization.

**Example**:

Assume the images have $N=64 \times 64=4096$ pixels in $L=8$ gray levels. The following table shows the equalization process corresponding to the two conversion methods above:

<img src='./pictures/07-img75.png' width=70%>

<img src='./pictures/07-equalize2.gif' width=65%>

In the following example, the histogram of a given image is equalized. Although the resulting histogram may not look constant, but the cumulative histogram is a exact linear ramp indicating that the density histogram is indeed equalized. The density histogram is not guaranteed to be a constant because the pixels of the same gray level cannot be separated to satisfy a constant distribution.

<img src='./pictures/07-Paolina_256_hist.gif' width=60%>

<img src='./pictures/07-Paolina_256_hist_eq.gif' width=60%>

<img src='./pictures/07-HistogramEQ.gif' width=100%>

### 3. Histogram Specification

Here we want to convert the image so that it has a particular histogram that can be arbitrarily specified. Such a mapping function can be found in three steps:

1. Equalize the histogram of the input image
2. Equalize the specified histogram
3. Relate the two equalized histograms

We first equalize the histogram $p_x$ of the input image $x$: 
$$
y=f(x)=\int_0^x p_x(u) du
$$
We then equalize the desired histogram $p_z$ of the output image $z$: 

$$
y'=g(z)=\int_0^z p_z(u) du
$$
The inverse of the above transform is 

$$
z=g^{-1}(y')
$$
As the two intermediate images $y$ and $y'$ both have the same equalized histogram, they are actually the same image, i.e., $y=y'$, and the overall transform from the given image $x$ to the desired image $z$ can be found to be: 
$$
z=g^{-1}(y')=g^{-1}(y)=g^{-1}(f(x))
$$
However, as the image gray levels are discrete, the continuous mapping obtained above can only be approximated. The discrete histograms $h_y[i]$ and $h_{y'}[i]$ are not necessarily identical. We therefore need to relate each gray level in $x$ with $h_x[i]\ne 0$ to a gray level in $z$ with $h_z[j]\ne 0$, so that the mapping from $y$ to $y'$ can be established.

<img src='./pictures/07-hist_specify.gif' width=50%>

Here are the specific steps of the algorithm:

- **Step 1**: Find histogram of input image $h_x$, and find its cumulative $H_x$, the histogram equalization mapping function:
  $$
  H_x[j]=\sum_{i=0}^j h_x[i]
  $$

- **Step 2**: Specify the desired histogram $h_z$, and find its cumulative $H_z$, the histogram equalization mapping function:
  $$
  H_z[j]=\sum_{i=0}^j h_z[i]
  $$

- **Step 3**: Relate the two mapping above to build a lookup table for the over all mapping. Specifically, for each input level $i$, find an output level $j$ so that $H_z[j]$ best matches $H_x[i]$:
  $$
  \vert H_x[i]-H_z[j] \vert = min_k \vert H_x[i]-H_z[k] \vert
  $$
  and then we setup a lookup entry $\textrm{lookup}[i]=j$.

**Example**:

The histogram of the given image and the histogram desired are shown below:

<img src='./pictures/07-hist_specify_1.gif' width=60%>

- **Step 1**: Equalize $p_x$ to get mapping $y=f(x)$ (save as previous example).

  | $x_i$ | $n_j$ | $h_x$ | $h=H_x$ |
  | :---: | :---: | :---: | :-----: |
  |   0   |  790  | 0.19  |  0.19   |
  |   1   | 1023  | 0.25  |  0.44   |
  |   2   |  850  | 0.21  |  0.65   |
  |   3   |  656  | 0.16  |  0.81   |
  |   4   |  329  | 0.08  |  0.89   |
  |   5   |  245  | 0.06  |  0.95   |
  |   6   |  122  | 0.03  |  0.98   |
  |   7   |  81   | 0.02  |  1.00   |

- **Step 2**: Equalize $p_z$ to get mapping $y'=g(z)$.

  | $z_i$ | $p_z$ | $y'=H_z$ |
  | :---: | :---: | :------: |
  |   0   |  0.0  |   0.0    |
  |   1   |  0.0  |   0.0    |
  |   2   |  0.0  |   0.0    |
  |   3   | 0.15  |   0.15   |
  |   4   | 0.20  |   0.35   |
  |   5   | 0.30  |   0.65   |
  |   6   | 0.20  |   0.85   |
  |   7   | 0.15  |   1.00   |

- **Step 3**: Obtain overall mapping,  $x \rightarrow y \rightarrow y' \rightarrow z $

  | $x_i = i$ | $y_j = H_x$ | $y'_j = H_z$ | $z_j = j$ |
  | :-------: | :---------: | :----------: | :-------: |
  |     0     |    0.19     |     0.0      |     3     |
  |     1     |    0.44     |     0.0      |     4     |
  |     2     |    0.65     |     0.0      |     5     |
  |     3     |    0.81     |     0.15     |     6     |
  |     4     |    0.89     |     0.35     |     6     |
  |     5     |    0.95     |     0.65     |     7     |
  |     6     |    0.98     |     0.85     |     7     |
  |     7     |    1.00     |     1.00     |     7     |

  Here is the look-up table:

  | $i$  |  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |
  | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
  | $j$  |  3   |  4   |  5   |  6   |  6   |  7   |  7   |  7   |

  This is the histogram of the resulting image:

  <img src='./pictures/07-hist_specify_2.gif' width=45%>

In the following example, the desired histogram is a triangle with linear increase in the lower half of the the gray level range, and linear decrease in the upper half. Again the cumulative histogram shows indeed the density histogram is such a triangle.

<img src='./pictures/07-HistogramEqSp.jpg' width=90%>