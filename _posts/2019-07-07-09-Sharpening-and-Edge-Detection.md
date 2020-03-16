## Sharpening and Edge Detection

Reference: [Sharpening and Edge Detection](http://fourier.eng.hmc.edu/e161/lectures/gradient/index.html)

### 1. Sharpening

High-pass filtering can be carried out by **subtracting** the *low-pass filtered image* from its *original* version, which can be considered as all-pass filtered by a delta function kernel: 

$$
W_{\textrm{ap}}=\left[ \begin{array}{ccc} 0 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{array} \right]
$$
When an image is convolved with this delta function it is not changed:
$$
W_{\textrm{ap}} * I_\textrm{o} = I_\textrm{o}
$$
As convolution is a linear operation, we have
$$
I_{\textrm{hp}} = I_{\textrm{ap}}-I_{\textrm{lp}}=W_{\textrm{ap}} * I_\textrm{o}-W_{\textrm{lp}} * I_\textrm{o} = (W_{\textrm{ap}}-W_{\textrm{lp}}) * I_\textrm{o} = W_{\textrm{hp}} * I_\textrm{o}
$$
where $W_{\textrm{hp}}=W_{\textrm{ap}}-W_{\textrm{lp}}$ is the high-pass kernel corresponding to the low-pass kernel $W_{\textrm{lp}}$. Equivalently the frequency domain filtering is shown in the figure:

<img src='./pictures/09-HighLowPassFilter.png' width=60%>

<img src='./pictures/09-HighLowPassFilter1.png' width=60%>

We can therefore obtain a **high-pass filtering kernel** corresponding to each of the *low-pass filter kernels* by subtracting the low-pass kernel from the all-pass kernel. The resulting kernels are various forms of high-pass filtering kernels, also called the **Laplace operators**.
$$
W_{\textrm{hp}} = W_{\textrm{ap}} - W_{\textrm{lp}} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{bmatrix} - \frac16 \begin{bmatrix} 0 & 1 & 0 \\ 1 & 2 & 1 \\ 0 & 1 & 0 \end{bmatrix} = \frac16 \begin{bmatrix} 0 & -1 & 0 \\ -1 & 4 & -1 \\ 0 & -1 & 0 \end{bmatrix}
$$

$$
W_{\textrm{hp}} = W_{\textrm{ap}} - W_{\textrm{lp}} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{bmatrix} - \frac19 \begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{bmatrix} = \frac19 \begin{bmatrix} -1 & -1 & -1 \\ -1 & 8 & -1 \\ -1 & -1 & -1 \end{bmatrix}
$$

$$
W_{\textrm{hp}} = W_{\textrm{ap}} - W_{\textrm{lp}} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{bmatrix} - \frac1{16} \begin{bmatrix} 1 & 2 & 1 \\ 2 & 4 & 2 \\ 1 & 2 & 1 \end{bmatrix} = \frac1{16} \begin{bmatrix} -1 & -2 & -1 \\ -2 & 12 & -2 \\ -1 & -2 & -1 \end{bmatrix}
$$

Note that the *sum of all elements* of the resulting high-pass filter is always <font color='red'>**zero**</font>. When such a high-pass kernel is convolved with a region of an image where all pixels have same gray level (constant or DC component), the result is zero, i.e., the zero spatial frequency component is totally suppressed by the high-pass filter.

Similar **band-pass filters** can be obtained by finding the difference between two *low-pass filters* of **different cut-off frequencies**.

<img src='./pictures/09-low_band_pass_filter.gif' width=65%>

The following figure shows the *original image*, a cat (left), and its *low-pass* (middle) and *high-pass* (right) filtered versions.

<img src='./pictures/09-spatialfiltering.gif' width=70%>

### 2. High-boost filtering

It is often desirable to emphasize **high frequency components** representing the *image details* (by means such as sharpening) without eliminating low frequency components representing the basic form of the signal. In this case, the **high-boost filter** can be used to enhance high frequency component while still keeping the low frequency components: 

$$
I_{\textrm{hb}}=I_\textrm{o}+c I_{\textrm{hp}} = (W_{\textrm{ap}} + c W_{\textrm{hp}}) * I_\textrm{o} = W_{\textrm{hb}} * I_\textrm{o}
$$
where $c$ is a constant and $W_{\textrm{hb}}=c W_{\textrm{ap}}+W_{\textrm{hp}}$ is the high boost convolution kernel. For example:
$$
W_{\textrm{hb}} = W_{\textrm{ap}} + c W_{\textrm{hp}} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{bmatrix} + c \begin{bmatrix} 0 & -1 & 0 \\ -1 & 4 & -1 \\ 0 & -1 & 0 \end{bmatrix} = \begin{bmatrix} 0 & -c & 0 \\ -c & 4c+1 & -c \\ 0 & -c & 0 \end{bmatrix}
$$

$$
W_{\textrm{hb}} = W_{\textrm{ap}} + c W_{\textrm{hp}} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{bmatrix} + c \begin{bmatrix} -1 & -1 & -1 \\ -1 & 8 & -1 \\ -1 & -1 & -1 \end{bmatrix} = \begin{bmatrix} -c & -c & -c \\ -c & 8c+1 & -c \\ -c & -c & -c \end{bmatrix}
$$

<img src='./pictures/09-high_boost.gif' width=40%>

The example below shows the effect of high-boost filtering obtained by the above high-boost convolution kernel with $c=2$. The image on the left is the original image, the one on the right is high-boost filtered. Note that the low spatial frequency components (global, large black background and bright areas) are suppressed while the high spatial frequency components (the texture of the fur and the whiskers) are enhanced. After a linear stretch, the image on the right is obtained.

<center class='half'><img src='./pictures/09-cat.gif' width=25%><img src='./pictures/09-cat_highboost_1.gif' width=25%></center>
### 3. The Gradient Operator

The **Gradient** (also called the *Hamilton* operator) is a vector operator for any $N$-dimensional scalar function $f(x_1,\cdots, x_N)=f({\bf x})$, where ${\bf x}=[x_1,\cdots,x_N]^T$ is an $N$-D vector variable. For example, when $N=3$, $f({\bf x})$ may represent temperature, concentration, or pressure in the 3-D space. The gradient of this $N$-D function is a vector composed of $N$ components for the $N$ partial derivatives:

$$
{\bf g}({\bf x})=\bigtriangledown f({\bf x})=\frac{d}{d {\bf x}} f(\bf x) = \left[ \frac{\partial f({\bf x})}{\partial x_1},\cdots, \frac{\partial f({\bf x})}{\partial x_N} \right]^T
$$

- The direction $\angle {\bf g}$ of the gradient vector ${\bf g}$ is the direction in the $N$-D space along which the function $f({\bf x})$ increases most rapidly.
- The magnitude $\vert{\bf g}\vert$ of the gradient ${\bf g}$ is the rate of the increment.

In image processing we only consider 2-D field:
$$
\bigtriangledown = \begin{bmatrix} \frac{\partial}{\partial x} \\ \frac{\partial}{\partial y} \end{bmatrix}
$$
When applied to a 2-D function $f(x,y)$, this operator produces a vector function:
$$
{\bf g}=\bigtriangledown f(x,y) = \begin{bmatrix} \frac{\partial}{\partial x} \\ \frac{\partial}{\partial y} \end{bmatrix} f(x,y) = \begin{bmatrix} f_x \\ f_y \end{bmatrix}
$$
where $f_x=\partial f/\partial x$ and $f_y=\partial f/\partial y$. The direction and magnitude of ${\bf g}$ are respectively
$$
\angle {\bf g}=\tan^{-1} (f_y/f_x),\;\;\;\;\;\;\;\vert\vert{\bf g}\vert\vert=\sqrt{f_x^2+f_y^2}
$$
Now we show that $f(x,y)$ increases most rapidly along the direction of  ${\bf g}=\vec{g}(x,y)$ and the rate of increment is equal to the magnitude of ${\bf g}=\vec{g}(x,y)$.

<img src='./pictures/09-gradient_direction.gif' width=30%>

Consider the *directional derivative* of $f(x,y)$ along an arbitrary direction $r$: 

$$
\frac{d}{dr}f(x,y) = \frac{\partial f}{\partial x}\frac{dx}{dr} + \frac{\partial f}{\partial y}\frac{dy}{dr} = f_x \cos \theta + f_y \sin\theta
$$
This directional derivative is a function of $\theta$, defined as the angle between directions $r$ and the positive direction of $x$. To find the direction along which $df/dr$ is maximized, we let
$$
\frac{d}{d\theta} \frac{df(x,y)}{dr} = \frac{d}{d\theta} (f_x \cos \theta + f_y \sin\theta) = -f_x \sin\theta +f_y \cos \theta = 0
$$
Solving this for $\theta$, we get
$$
f_x \sin\theta=f_y \cos \theta
$$
i.e.,
$$
\theta =\tan^{-1} \left(\frac{f_y}{f_x}\right)
$$
which is indeed the direction $\angle {\bf g}$ of ${\bf g}=\vec{g}(x,y)$.

From $\tan  \theta=f_y/f_x$, we can also get
$$
\sin\theta=\frac{f_y}{\sqrt{f_x^2+f_y^2}},\;\;\;\; \cos \theta=\frac{f_x}{\sqrt{f_x^2+f_y^2}}
$$
Substituting these into the expression of $df/dr$, we obtain its maximum magnitude,
$$
\left. \frac{d}{dr}f(x,y) \right\vert _{\max} = \frac{f_x^2+f_y^2}{\sqrt{f_x^2+f_y^2}}=\sqrt{f_x^2+f_y^2}
$$
which is the magnitude of  $\vec{g}(x,y)$.

### 4. Digital Gradient

For discrete digital images, the derivative in gradient operation 

$$
D_x[f(x)]=\frac{d}{dx}f(x)=\lim_{\Delta x \rightarrow 0} \frac{f(x+\Delta x)-f(x)}{\Delta x}
$$
becomes the difference
$$
D_n[f[n]]=f[n+1]-f[n],\;\;\;\;\mbox{or}\;\;\;\;\frac{f[n+1]-f[n-1]}{2}
$$
Two steps for finding discrete gradient of a digital image:

1. Find the **difference**: in the two directions:
   $$
   g_m[m,n]=D_m[f[m,n]]=f[m+1,n]-f[m,n]
   $$

   $$
   g_n[m,n]=D_n[f[m,n]]=f[m,n+1]-f[m,n]
   $$

2. Find the **magnitude** and **direction** of the gradient vector:
   $$
   \Vert g[m,n]\Vert=\sqrt{g^2_m[m,n]+g^2_n[m,n]},\;\; \;\; \mbox{or approximately} \;\;\;\; \Vert g[m,n] \Vert = \Vert g_m \Vert + \Vert g_n \Vert
   $$

   $$
   \angle g[m,n]=\tan^{-1} \left(\frac{g_n[m,n]}{g_m[m,n]}\right)
   $$

The differences in two directions $g_m$ and $g_n$ can be obtained by convolution with the following kernels:

- **Roberts**
  $$
  \begin{bmatrix} -1 & 1 \\ 0 & 0 \end{bmatrix}, \quad \begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}
  $$
  or
  $$
  \begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}, \quad \begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}
  $$

- **Sobel** ($3 \times 3$)
  $$
  \begin{bmatrix} -1 & 0 & 1 \\ -2 & 0 & 2 \\ -1 & 0 & 1 \end{bmatrix}, \quad \begin{bmatrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ 1 & 2 & 1 \end{bmatrix}
  $$

- **Prewitt** ($3 \times 3$)
  $$
  \begin{bmatrix} -1 & 0 & 1 \\ -1 & 0 & 1 \\ -1 & 0 & 1 \end{bmatrix}, \quad \begin{bmatrix} -1 & -1 & -1 \\ 0 & 0 & 0 \\ 1 & 1 & 1 \end{bmatrix}
  $$

- **Prewitt** ($4 \times 4$)
  $$
  \begin{bmatrix} -3 & -1 & 1 & 3 \\ -3 & -1 & 1 & 3 \\ -3 & -1 & 1 & 3 \\ -3 & -1 & 1 & 3 \end{bmatrix}, \quad \begin{bmatrix} -3 & -3 & -3 & -3 \\ -1 & -1 & -1 & -1 \\ 1 & 1 & 1 & 1 \\ 3 & 3 & 3 & 3 \end{bmatrix}
  $$

Note *Sobel* and *Prewitt* operators first find the averages of one direction and then find the difference of these averages in the another direction.

### 5. Compass Gradient Operations

The following compass operators can detect edges of a particular direction:
$$
\begin{bmatrix} -1 & 0 & 1 \\ -1 & 0 & 1 \\ -1 & 0 & 1 \end{bmatrix}, \quad \begin{bmatrix} 0 & 1 & 1 \\ -1 & 0 & 1 \\ -1 & -1 & 0 \end{bmatrix},  \quad \begin{bmatrix} 1 & 1 & 1 \\ 0 & 0 & 0 \\ -1 & -1 & -1 \end{bmatrix},  \quad \begin{bmatrix} 1 & 1 & 0 \\ 1 & 0 & -1 \\ 0 & -1 & -1 \end{bmatrix} \\ 
\begin{bmatrix} 1 & 0 & -1 \\ 1 & 0 & -1 \\ 1 & 0 & -1 \end{bmatrix}, \quad \begin{bmatrix} 0 & -1 & -1 \\ 1 & 0 & -1 \\ 1 & 1 & 0 \end{bmatrix},  \quad \begin{bmatrix} -1 & -1 & -1 \\ 0 & 0 & 0 \\ 1 & 1 & 1 \end{bmatrix},  \quad \begin{bmatrix} -1 & -1 & 0 \\ -1 & 0 & 1 \\ 0 & 1 & 1 \end{bmatrix}
$$
Other compass operators
$$
\begin{bmatrix} 1 & 1 & 1 \\ 1 & -2 & 1 \\ -1 & -1 & -1 \end{bmatrix}, \quad \begin{bmatrix} 5 & 5 & 5 \\ -3 & 0 & -3 \\ -3 & -3 & -3 \end{bmatrix},  \quad \begin{bmatrix} 1 & 2 & 1 \\ 0 & 0 & 0 \\ -1 & -2 & -1 \end{bmatrix}
$$
For all convolution kernels above, the sum of all elements is <font color='red'>**zero**</font>, i.e., the output from a homogeneous image region is zero. If the orientation of the edge is not needed, we run these compass operators in all directions and find if the maximum of them is greater than a threshold value.

<img src='./pictures/09-rubic.jpg' width=25%>

<img src='./pictures/09-edge_detection_4_dir.gif' width=100%>

*Higher angular resolution* can be achieved by **increase the mask size**. The two kernels below are for $30^{\circ}$ and $60^{\circ}$ degrees, respectively:
$$
\begin{bmatrix} 1 & 1 & 1 & 1 & 1 \\ -0.32 & 0.78 & 1 & 1 & 1 \\ -1 & -0.92 & 0 & 0.92 & 1 \\ -1 & -1 & -1 & -0.78 & 0.32 \\ -1 & -1 & -1 & -1 & -1 \end{bmatrix}, \quad \begin{bmatrix} -1 & 0.32 & 1 & 1 & 1 \\ -1 & -0.78 & 0.92 & 1 & 1 \\ -1 & -1 & 0 & 1 & 1 \\ -1 & -1 & -0.92 & 0.78 & 1 \\ -1 & -1 & -1 & -0.32 & 1 \end{bmatrix}
$$
The *gradient image* $g[m,n]$ obtained by applying the gradient operator to the original image $x[m,n]$ can be used in various ways to **enhance the details of the image**. For example, gradient image can be used to high-boost to <font color='blue'>emphasize the details in the image while still keeping the rest as background</font>:

- $$
  y[m,n] = c_1 x[m,n] + c_2 g[m,n]
  $$

  where $c_1$ and $c_2$ are two weights specified by the user.

- $$
  y[m,n]=\left\{ \begin{array}{ll} g[m,n] & \mbox{if $g[m,n] \ge T$} \\ x[m,n] & \mbox{else} \end{array} \right.
  $$

- $$
  y[m,n]=\left\{ \begin{array}{ll} L_g & \mbox{if $g[m,n] \ge T$} \\ x[m,n] & \mbox{else} \end{array} \right.
  $$

  Here $T$ is some specified threshold, and $L_g$ is a constant.

Alternatively, we can <font color='blue'>emphasize the details while suppressing the background</font>:

- $$
  y[m,n]=\left\{ \begin{array}{ll} g[m,n] & \mbox{if $g[m,n] \ge T$} \\L_b & \mbox{else} \end{array} \right.
  $$

- $$
  y[m,n]=\left\{ \begin{array}{ll} L_g & \mbox{if $g[m,n] \ge T$} \\ L_b & \mbox{else} \end{array} \right.
  $$

  Here $L_g$ and $L_b$ are two gray levels assigned to the details and the background.

### 6. Edge Detection

In most applications *edges* and *lines* in images carry more useful information than other types of features such as texture, color, etc. Edges and lines may be caused by various reasons:

<img src='./pictures/09-edges_lines_example.gif' width=50%>

<img src='./pictures/09-edges_lines_1d.gif' width=50%>

*Edge detection* is one of the most essential tasks in image processing. As edges and lines are drastic changes in gray level over a small spatial distance, they correspond to high spatial frequency components in the image signal. The **main challenge** is to distinguish edges from other small features in the image such as textures and especially the noise, so that all edges are detected while the noise is suppressed.

The *gradient operator* is sensitive to local gray level changes and is therefore a convenient tool to detect edges. If the magnitude $\vert g[m,n]\vert$ of the gradient image is larger than a threshold value, the pixel $[m,n]$ can be considered as on an edge. Moreover, the orientation of the edge can be obtained from the direction $\angle g[m,n]$ of the gradient vector. Some simple examples of 1-D case is shown below:

<center class='half'><img src='./pictures/09-gradient_edge_detection_1d.gif' width=40%><img src='./pictures/09-gradient_line_detection_1d.gif' width=40%></center>

<img src='./pictures/09-forest.gif' width=30%>

Edge detection by gradient operators (*Roberts*, *Sobel* and *Prewitt*):

<img src='./pictures/09-forest_gradient.gif' width=70%>

However, when these gradient operators are used as edge detectors, their performances are very poor. Basically, *they can not distinguish edges from textures and/or noise*. The Prewitt gradient filter (3 by 3 and 5 by 5) is used to obtain the edges in the following image containing fuzzy edges:

<img src='./pictures/09-fuzzy_edge.gif' width=30%>

The difficulty of edge detection can be best appreciated by looking at the profile of the image shown on the top part of the image.

<img src='./pictures/09-fuzzy_edge_demo.gif' width=30%>

<center class='half'><img src='./pictures/09-fuzzy_edge_gradient_1.gif' width=30%><img src='./pictures/09-fuzzy_edge_gradient_2.gif' width=30%></center>

### 7. The Laplace Operator

The **Laplace operator** is a scalar operator defined as the *dot product* (*inner product*) of two gradient vector operators: 

$$
\bigtriangleup = \bigtriangledown \cdot \bigtriangledown = \bigtriangledown^2 = \left[ \frac{\partial}{\partial x_1}, \cdots, \frac{\partial}{\partial x_N} \right] \begin{bmatrix} \frac{\partial}{\partial x_1} \\ \vdots \\ \frac{\partial}{\partial x_N} \end{bmatrix} = \sum_{n=1}^N \frac{\partial^2}{\partial x_n^2}
$$
In $N=2$ dimensional space, we have:
$$
\bigtriangleup =\bigtriangledown \cdot \bigtriangledown = \bigtriangledown^2 = \left[ \frac{\partial}{\partial x} \;\; \frac{\partial}{\partial y} \right] \begin{bmatrix} \frac{\partial}{\partial x} \\ \frac{\partial}{\partial y} \end{bmatrix} = \frac{\partial^2}{\partial x^2}+\frac{\partial^2}{\partial y^2}
$$
When applied to a 2-D function $f(x,y)$, this operator produces a scalar function:
$$
\bigtriangleup f(x,y) = \frac{\partial^2 f}{\partial x^2}+\frac{\partial^2 f}{\partial y^2}
$$
In discrete case, the second order differentiation becomes second order difference. In 1-D case, if the first order difference is defined as
$$
\bigtriangledown f[n]=f[n+1]-f[n]
$$
then the second order difference is
$$
\begin{align} \bigtriangleup f[n] & = \bigtriangledown\;(\bigtriangledown f[n]) \\ & = \bigtriangledown f[n]-\bigtriangledown f[n-1] \\ & = (f[n+1]-f[n])-(f[n]-f[n-1]) \\ & = f[n+1]-2f[n]+f[n-1] \end{align}
$$
Note that $\bigtriangleup f[n]$ is so defined that it is <font color='red'>**symmetric**</font> to the center element $f[n]$. The Laplace operation can be carried out by 1-D convolution with a kernel $[1, -2, 1]$.

In 2-D case, Laplace operator is the sum of two second order differences in both dimensions:
$$
\begin{align} \bigtriangleup f[m,n] & =	\bigtriangleup_m[f[m,n]]+\bigtriangleup_n[f[m,n]] \\ & = f[m+1,n]-2f[m,n]+f[m-1,n]+f[m,n+1]-2f[m,n]+f[m,n-1] \\ & = f[m+1,n]+f[m-1,n]+f[m,n+1]+f[m,n-1]-4f[m,n] \end{align}
$$
 This operation can be carried out by 2-D convolution kernel:
$$
\begin{bmatrix} 0 & 1 & 0 \\ 1 & -4 & 1 \\ 0 & 1 & 0 \end{bmatrix}
$$
Other Laplace kernels can be used:
$$
\begin{bmatrix} 1 & 1 & 1 \\ 1 & -8 & 1 \\ 1 & 1 & 1 \end{bmatrix}
$$
We see that these **Laplace kernels** are actually the same as the *high-pass filtering kernels* discussed before.

**Gradient** operation is an effective detector for *sharp edges* where the pixel gray levels change over space very rapidly. But when the gray levels <font color='red'>change slowly</font> from dark to bright (<font color='red'>red</font> in the figure below), the gradient operation will produce a very <font color='green'>wide edge</font> (<font color='green'>green</font> in the figure). It is helpful in this case to consider using the **Laplace** operation. The <font color='blue'>second order derivative</font> of the wide edge (<font color='blue'>blue</font> in the figure) will have a *zero crossing* in the middle of edge. Therefore the location of the edge can be obtained by detecting the zero-crossings of the second order difference of the image.

<img src='./pictures/09-fat_edge.gif' width=70%>

<img src='./pictures/09-fat_edge_negate.gif' width=70%>

One dimensional example:

<img src='./pictures/09-fat_edge_detection_1d.gif' width=70%>

In the two dimensional example, the image is on the left, the two Laplace kernels generate two similar results with zero-crossings on the right:

<img src='./pictures/09-fat_edge_detection_2d.gif' width=60%>

If in the neighborhood ($3 \times 3$, $5 \times 5$, $7 \times 7$, etc.) of a given pixel there exist both polarities, i.e., pixel values greater than and smaller than 0, then the pixel is a zero-crossing. (Note that the pixel in question does not have to be zero.) Specifically, we find the maximum and minimum among all pixels in the neighborhood of a pixel under consideration. <font color='blue'>If the maximum is greater than zero and the minimum is smaller than zero, the pixel is a zero-crossing</font>.

Due to the existence of *random noise* some false zero-crossing may be detected. In this case we check whether the difference between the maximum and the minimum is greater than a <font color='red'>threshold value</font>. If so the pixel is on an edge, otherwise the zero-crossing is assumed to be caused by noise and suppressed.

### 8. Laplacian of Gaussian (LoG)

As *Laplace* operator may detect edges as well as noise (isolated, out-of-range), it may be desirable to *smooth* the image first by a convolution with a Gaussian kernel of width $\sigma$

$$
G_{\sigma}(x,y)=\frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{x^2+y^2}{2\sigma^2}\right)
$$
to suppress the noise before using Laplace for edge detection:
$$
\bigtriangleup[G_{\sigma}(x,y) * f(x,y)]=[\bigtriangleup G_{\sigma}(x,y)] * f(x,y)=LoG*f(x,y)
$$
The first equal sign is due to the fact that
$$
\frac{d}{dt}[h(t)*f(t)]=\frac{d}{dt} \int f(\tau) h(t-\tau)d\tau = \int f(\tau) \frac{d}{dt} h(t - \tau) d \tau = f(t) * \frac{d}{dt} h(t)
$$
So we can obtain the <font color='red'>**Laplacian of Gaussian**</font> $\bigtriangleup G_{\sigma}(x,y)$ first and then convolve it with the input image. To do so, first consider
$$
\frac{\partial}{\partial x} G_{\sigma}(x,y) = \frac{\partial}{\partial x} \left( \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left( - \frac{x^2 + y^2}{2 \sigma^2} \right) \right) = - \frac{x}{\sigma^2} \cdot \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left( - \frac{x^2 + y^2}{2 \sigma^2} \right)
$$
and
$$
\begin{align}\frac{\partial^2}{\partial x^2} G_{\sigma}(x,y) & = - \frac{1}{\sigma^2} \cdot \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left( - \frac{x^2 + y^2}{2 \sigma^2} \right) - \frac{x}{\sigma^2} \left(- \frac{x}{\sigma^2} \cdot  \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left( - \frac{x^2 + y^2}{2 \sigma^2} \right) \right) \\ & = \frac{x^2 - \sigma^2}{\sigma^4} \cdot \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left( - \frac{x^2 + y^2}{2 \sigma^2} \right) \end{align}
$$
Note that for simplicity we omitted the normalizing coefficient $1/\sqrt{2\pi \sigma^2}$. Similarly we can get
$$
\frac{\partial^2}{\partial y^2} G_{\sigma}(x,y) = \frac{y^2 - \sigma^2}{\sigma^4} \cdot \exp \left( - \frac{x^2 + y^2}{2 \sigma^2} \right)
$$
Now we have *LoG* as an operator or convolution kernel defined as
$$
LoG \stackrel{\triangle}{=} \bigtriangleup G_{\sigma}(x,y) = \frac{\partial^2}{\partial x^2} G_{\sigma}(x,y) + \frac{\partial^2}{\partial y^2} G_{\sigma}(x,y) = \frac{x^2+y^2-2\sigma^2}{\sigma^4} \exp \left( - \frac{x^2 + y^2}{2 \sigma^2} \right)
$$
The Gaussian $G(x,y)$ and its first and second derivatives $G'(x,y)$ and $\bigtriangleup G(x,y)$ are shown here:

<img src='./pictures/09-LoG.gif' width=70%>

<img src='./pictures/09-LoG_plot.gif' width=70%>

This 2-D *LoG* can be approximated by a 5 by 5 convolution kernel such as
$$
\begin{bmatrix} 0 & 0 & 1 & 0 & 0 \\ 0 & 1 & 2 & 1 & 0 \\ 1 & 2 & -16 & 2 & 1 \\ 0 & 1 & 2 & 1 & 0 \\ 0 & 0 & 1 & 0 & 0 \end{bmatrix}
$$
The kernel of any other sizes can be obtained by approximating the continuous expression of *LoG* given above. However, make sure that the sum (or average) of all elements of the kernel has to be <font color='red'>**zero**</font> (similar to the Laplace kernel) so that the convolution result of a homogeneous regions is always zero.

The edges in the image can be obtained by **these steps**:

1. Applying *LoG* to the image
2. Detection of zero-crossings in the image
3. Threshold the zero-crossings to keep only those strong ones (large difference between the positive maximum and the negative minimum)

The last step is needed to suppress the weak zero-crossings most likely caused by noise.

### 9. Difference of Gaussian (DoG)

Similar to *Laplace of Gaussian*, the image is first smoothed by convolution with Gaussian kernel of certain width $\sigma_1$

$$
G_{\sigma_1}(x,y) = \frac{1}{\sqrt{2 \pi \sigma_1^2}} \exp \left(-\frac{x^2+y^2}{2\sigma_1^2} \right)
$$
to get
$$
g_1(x,y)=G_{\sigma_1}(x,y)*f(x,y)
$$
With a different width $\sigma_2$, a second smoothed image can be obtained:
$$
g_2(x,y)=G_{\sigma_2}(x,y)*f(x,y)
$$
We can show that the difference of these two Gaussian smoothed images, called **difference of Gaussian (DoG)**, can be used to detect edges in the image.
$$
g_1(x,y)-g_2(x,y)=G_{\sigma_1}*f(x,y)-G_{\sigma_2}*f(x,y) = (G_{\sigma_1}-G_{\sigma_2})*f(x,y) = \mbox{DoG}*f(x,y)
$$
The DoG as an operator or convolution kernel is defined as
$$
DoG \stackrel{\triangle}{=} G_{\sigma_1} - G_{\sigma_2} = \frac{1}{\sqrt{2 \pi}} \left(\frac{1}{\sigma_1} \exp \left(- \frac{x^2+y^2}{2 \sigma_1^2} \right) - \frac{1}{\sigma_2} \exp \left(- \frac{x^2+y^2}{2 \sigma_2^2} \right) \right)
$$
Both 1-D and 2-D functions of $G_{\sigma_1}(x,y)$ and $G_{\sigma_2}(x,y)$ and their difference are shown below:

<img src='./pictures/09-DoG.gif' width=70%>

<img src='./pictures/09-DoG_plot.gif' width=70%>

As the difference between two differently *low-pass* filtered images, the **DoG** is actually a *band-pass filter,* which removes *high frequency components* representing noise, and also some *low frequency components* representing the homogeneous areas in the image. The frequency components in the passing band are assumed to be associated to the edges in the images.

The discrete convolution kernel for DoG can be obtained by approximating the continuous expression of DoG given above. Again, it is necessary for the sum or average of all elements of the kernel matrix to be <font color='red'>**zero**</font>.

Comparing this plot with the previous one, we see that the DoG curve is very similar to the LoG curve. Also, similar to the case of LoG, the edges in the image can be obtained by **these steps**:

1. Applying DoG to the image
2. Detection of zero-crossings in the image
3. Threshold the zero-crossings to keep only those strong ones (large difference between the positive maximum and the negative minimum)

The last step is needed to suppress the weak zero-crossings most likely caused by noise.