## Digital Images

Reference: [Digital Images](http://fourier.eng.hmc.edu/e161/lectures/digital_image/index.html)

### 1. Image Digitization
#### 1.1 Quantization

The continuous range of light intensity $0\le x \le G$ received by the digital image acquisition system need to be quantized to $L$ gray levels (e.g., $L=2^8=256$). The numbers of gray levels of the following eight images are respectively 256, 128, 64, 32, 16, 8, 4, and 2, respectively.

<img src='./pictures/04-GlevelRes.gif' width=80%>

##### 1.1.1 Uniform distribution

Define $L+1$ boundaries 
$$
\begin{equation}t_k=kG/L=k\Delta,\;\;\;(k=0,1,\cdots,L) \end{equation}
$$
where $\Delta\stackrel{\triangle}{=}G/L$, and define the $L$ discrete gray levels to represent the $L$ intervals: 
$$
\begin{equation}r_k=t_k+\Delta/2=k\Delta+\Delta/2=(k\Delta+k\Delta+\Delta)/2
=(t_k+t_{k+1})/2 \end{equation}
$$
Then the quantization can be defined as a function
$$
\begin{equation}x'=f(x)=r_k,\;\;\;\;\mbox{iff}\;\;t_k \le x \le t_{k+1} \end{equation}
$$
<img src='./pictures/04-linear_quantize.gif' width=50%>

##### 1.1.2 Mean square error optimization

Define **mean square error of the quantization** process as 
$$
\begin{equation}{\cal E} = E[ (x-x')^2]=\int_{-\infty}^\infty (x-x')^2 p(x)dx=
\sum_{i=0}^{L-1} \int_{t_i}^{t_{i+1}} (x-r_i)^2 p(x)dx \end{equation}
$$
where $p(x)$ is distribution of input intensify $x$. The optimal quantization in terms of $t_k$ and $r_k$ can be found by minimizing $ \cal{E}$, by solving 
$$
\begin{equation}\frac{\partial {\cal E}}{\partial t_k}=\frac{\partial {\cal E}}{\partial r_k}=0 \end{equation}
$$
This method requires $p(x)$ to be known. The previous quantization is optimal when $p(x)$ is a uniform distribution. When $p(x)$ is not uniform, more gray levels will be assigned to the gray scale regions corresponding to higher $p(x)$.

##### 1.1.3 Contrast equalization

The perceived contrast is a function of the intensity. Specifically, we perceive the same contrast between the object and its surrounding if 
$$
\begin{equation}\frac{\Delta f}{f} \approx {\frac{df}{f}}=d(\ln f)=\textrm{constant} \end{equation}
$$
where $f$ is the intensity and $\Delta f \approx df$ is the intensity difference, the absolute contrast ([Weber's law](http://en.wikipedia.org/wiki/Weber-Fechner_law)). For example, 
$$
\frac{20-10}{10}=\frac{200-100}{100}
$$
i.e., a high contrast of $\Delta f=200-100=100$ at a high absolute intensity $f=100$ is perceived the same as a much lower contrast of $\Delta f=20-10=10$ at a low absolute intensity $f=10$. In other words, we are less sensitive to contrast when the intensity $f$ is high. As another example, consider the perceived brightness of a 3-way light bulb with 50, 100 and 150 Watts (with the assumption that the brightness is proportional to the power consumption). The perceived contrast between 50 and 100 is higher than that between 100 and 150 as $(100-50)/50 > (150-100)/100$. Consequently, the **perceived contrast** can be defined as a *logarithmic* function of the intensity: 
$$
\begin{equation}y=f(x)=a\;\log_e(1+x) \end{equation}
$$
As shown in the figure, to perceive the same contrast, larger intensity difference is needed for higher intensity regions than lower ones. To most efficiently use the limited number of gray levels available, we can allocate more gray levels in the low intensity region where our eye is more sensitive to contrast) than in high intensity region.

<img src='./pictures/04-contrast.gif' width=60%>

Weber's law describes a general phenomenon in human perception. Another example is the difference between different sound frequencies. The difference between $C_4$ (middle C, 261.63 Hz) and $C_5$ (523.25 Hz) is an octave, perceived the same as the difference between $C_5$ and $C_6$ (1046.5 Hz), although the frequency differences between the two pairs are quite different (261.63 Hz. vs. 523.25 Hz).

##### 1.1.4 Gamma correction

In the image acquisition process, nonlinear mapping may occur in various stages. To compensate for all such nonlinear mappings, the following power function that relates the input $x$ to the output $y$ can be considered:
$$
y=A x^\gamma
$$

where the ranges of both the input and output are normalized so that  $0\le x,y\le 1$. Here $A$ is a constant scaling factor, and $\gamma$ is a parameter that characterizes the nonlinearity. Obviously when $\gamma=1$,  $y$ is linearly related to $x$. Otherwise, we have a nonlinear mapping. As an example, the nonlinear CRT mapping modeled by $y=x^\gamma=2^{2.2}$ can be corrected by another nonlinear mapping $z=y^{1/\gamma}=y^{1/2.2}=(x^{2.2})^{1/2.2}=x$, as shown below:

<img src='./pictures/04-CRTgamma.gif' width=40%>

#### 1.2 Spatial sampling

Also, the continuous two-dimensional image space need to be sampled by the digital image acquisition system to form a raster, a 2D array of pixels (picture-elements) in rows and columns. Same as in 1D case, the sampling theorem also applies her, with the only difference that the sampling is carried out in two spatial dimensions, instead of one temporal dimension.

<img src='./pictures/04-ImageResolution.gif' width=100%>

#### 1.3 Color and pseudo-color images

A color image is usually represented by three functions of space. In most color formats, the three functions are for three primary colors such as red, green and blue $f_r(x,y)$, $f_g(x,y)$, and $f_b(x,y)$, or some other three parameters such as intensity, hue and saturation, $f_i(x,y)$, $f_h(x,y)$, and $f_s(x,y)$.

Sometimes artificial colors can be assigned to a gray level image to better distinguish visually the different gray levels.

The display of *gray level*, *pseudo-color* and *true-color* images on a monitor screen through color-map (color lookup table) is illustrated below.

<img src='./pictures/04-image_display_a.gif' width=70%>

<img src='./pictures/04-image_display_b.gif' width=70%>

### 2. Neighbors and Connectivities

#### 2.1 Neighbors of Pixel

There are two different ways to define the neighbors of a pixel $p$ located at $(x,y)$:

- **4-neighbors**
  The 4-neighbors of pixel $p$, denoted by $N_4(p)$, are the four pixels located at $(x-1,y)$, $(x+1,y)$, $(x,y-1)$, and $(x,y+1)$, there are, respectively, above (north), below (south), to the left (west) and right (east) of the pixel $p$.

- **8-neighbors**
  The 8-neighbors of pixel $p$, denoted by $N_8(p)$, include the four 4-neighbors and four pixels along the diagonal direction located at $(x-1,y-1)$ (northwest), $(x-1,y+1)$ (northeast), $(x+1,y-1)$ (southwest) and $(x+1,y+1)$ (southeast).

#### 2.2 Connectivity

**Binary Image**: In a binary (black and white) image, two neighboring pixels (as defined above) are connected if their values are the same, i.e., both equal to 0 (black) or 255 (white).

**Gray Image**: In a gray level image, two neighboring pixels are connected if their values are close to each other, i.e., they both belong to the same subset of similar gray levels: $p \in V$ and $q \in V$, where $V$ is a subset of all gray levels in the image.

Specifically, the connectivity can be defined as one of the following:

- **4-connected**: Two pixels $p$ and $q$ are *4-connected* if they are 4-neighbors and $p \in V$ and $q \in V$;

- **8-connected**: Two pixels $p$ and $q$ are *8-connected* if they are 8-neighbors and  $p \in V$ and $q \in V$;

- **mixed-connected**: Two pixels $p$ and $q$ are *mix-connected* if

  - $p$ and $q$ are 4-connected, **or**

  - $p$ and $q$ are 8-connected *and* not 4-connected through a third pixel ( $N_4(p) \cap N_4(q) \not\subset V$)

    The second condition states that if $p$ and $q$ are 8-connected and they are also 4-connected through a third pixel, the tighter 4-connectivity through a third pixel is preferred and therefore $p$ and $q$ are no longer considered as 8-connected.

Two pixels $p$ at $(x,y)$ and $q$ at $(u,v)$ not 4, 8, or mix-connected can still be connected through a path composed of a sequence (chain) of pixels 

$$
(x_0,y_0)=(x,y), (x_1,y_1), \cdots, (x_{n-1},y_{n-1}),(x_n,y_n)=(u,v)
$$
with all neighboring pixels $(x_i,y_i)$ and $(x_{i+1},y_{i+1})$ 4, 8, or mix-connected.

**Example:**

The upper-right pixel and the lower-left pixel are 8 and mix-connected, but they are not 4-connected:
$$
0 \quad 0 \quad 1 \\
0 \quad 1 \quad 0 \\
1 \quad 0 \quad 0
$$
The upper-right pixel and the lower-left pixel are 4, 8 and mix-connected:
$$
0 \quad 1 \quad 1 \\0 \quad 1 \quad 0 \\1 \quad 1 \quad 0
$$
#### 2.3 Distances

Any *distance metric* $D(p,q)$ between pixels $p$ and $q$ must satisfy:

1. $D(p,q) \ge 0,\;\; (D(p,q)=0 \;\;\mbox{iff}\;\; p=q)$;
2. $D(p,q)=D(q,p)$;
3. $D(p,q) \le D(p,r)+D(r,q)$.

where $r$ is an arbitrary pixel.

Specifically, the distance between pixels $p$ at $(x,y)$ and $q$ at $(u,v)$ can be defined by one of the following:

- **Euclidean distance**
  $$
  D_E(p,q)=\sqrt{(x-u)^2+(y-v)^2}=[\vert x-u\vert^2+\vert y-v\vert^2]^{1/2}
  $$

- **City-block distance**
  $$
  D_4(p,q)=\vert x-u\vert+\vert y-v\vert=[\vert x-u\vert^1+\vert y-v\vert^1]^{1/1}
  $$

- **Chess-board distance**
  $$
  D_8(p,q) = \max\{\vert x-u\vert,\vert y-v\vert\}=[\vert x-u\vert^\infty+\vert y-v\vert^\infty]^{1/\infty}
  $$

From these definitions we see that a general distance definition is 

$$
D(p,q)=[\vert x-u\vert^L+\vert y-v\vert^L]^{1/L}
$$
where $L$ can take any value between 1 and $\infty$. When $L$ is small (e.g., 1), contributions of the two dimensions are treated equally, but when $L$ is large (e.g., toward $\infty$), the dimension with larger contribution is more emphasized. Note that other types of distance metrics can also be used.

The $D_E$ distance in digital image approximates the actual Euclidean distance in continuous situation.

**Examples**:

*Example 1*: The numbers in the following array show the $D_4$ distances to the pixel in the center. Note that all 4-neighbors have distance 1.
$$
4 \quad 3 \quad 2 \quad 3 \quad 4 \\
3 \quad 2 \quad 1 \quad 2 \quad 3 \\
2 \quad 1 \quad 0 \quad 1 \quad 2 \\
3 \quad 2 \quad 1 \quad 2 \quad 3 \\
4 \quad 3 \quad 2 \quad 3 \quad 4
$$
*Example2*: The numbers here are the $D_8$ distances to the pixel in the center. Note that all 8-neighbors have distance 1.
$$
2 \quad 2 \quad 2 \quad 2 \quad 2 \\
2 \quad 1 \quad 1 \quad 1 \quad 2 \\
2 \quad 1 \quad 0 \quad 1 \quad 2 \\
2 \quad 1 \quad 1 \quad 1 \quad 2 \\
2 \quad 2 \quad 2 \quad 2 \quad 2
$$
The following figure shows the iso-distance contours composed of all points having equal distance to the center point. The circle is for Euclidean distance, the square is for the $D_8$ distance, the diamond is for the $D_4$ distance.

<img src='./pictures/04-iso_distance.gif' width=30%$>

Distance between two connected pixels can be defined as the number of hops from one pixel to the next along the shortest path connecting the two pixels, according to the definition of connectivity (4, 8, or mix-connected).

### 3. Histogram and normalization

The **histogram** $h[i]$ ($i=0,\cdots,255)$ is the probability of an arbitrary pixel taking the gray level $i$, which can be approximated as: 

$$
h[i]=\frac{\mbox{Number of pixels of gray level $i$}}{\mbox{Total number of pixels}}
$$
The **cumulative density function** is defined as: 
$$
H[j]=\sum_{i=0}^j h[i],\;\;\;\;\;(j=0,1,\cdots,255)
$$
Obviously $H[255]=1$.

The image can be normalized or rescaled by:
$$
y=f(x)=255 \times \frac{x-x_{\min}}{x_{\max}-x_{\min}}
$$