---
layout: post
title: 图像纹理
date: 2019-01-25
categories: Image-processing
tags: Texture
status: publish
type: post
published: true
author: Quebradawill
---

### 1. LBP

转自：[LBP纹理特征提取学习笔记](https://blog.csdn.net/hongbin_xu/article/details/79924961)

#### 1.1 概述

LBP（Local Binary Pattern，局部二值模式）是一种用来描述图像局部纹理特征的算子；它具有旋转不变性和灰度不变性等显著的优点。它首先由 T. Ojala, M.Pietikäinen 和 D. Harwood 于 1994 年提出，用于纹理特征提取。而且，提取的特征是图像的局部的纹理特征。常用的特征描述子有：*HOG*、*Harris*、*LBP* 等等，其中 LBP 是最为简单且有效的一种特征描述子。

#### 1.2 公式

表示成数学公式：
$$
LBP(x_c, y_c) = \sum_{p=0}^{P-1} s(I(p) - I(c)) \times 2^p
$$
其中，$p$ 表示 $ 3 \times 3 $ 窗口中除中心像素点外的第 $p$ 个像素点；$ I(c) $ 表示中心像素点的灰度值，$ I(p) $ 表示邻域内第 $p$ 个像素点的灰度值；$ s(x)$ 为 Kronecker 函数，公式如下：

$$
s(x) = \begin{cases} 1, & x \geq 0 \\ 0, & \textrm{otherwise} \end{cases}
$$

#### 1.3 直观解释

通过上述变换，我们可以将一个像素点与 8 个相邻点之间的差值关系用一个数表示，这个数的范围是 $0 \sim 255$。因为 LBP 记录的是中心像素点与邻域像素点之间的差值，所以当光照变化引起像素灰度值同增同减时，LBP 变化并不明显。所以可以认为<font color="red"> LBP 对光照变化不敏感</font>，LBP 检测的仅仅是图像的纹理信息，因此，进一步还可以**将 LBP 做直方图统计**，这个<font color="red">直方图</font>可以用来作为纹理分析的特征算子。

### 2. 圆形 LBP

#### 2.1 概念

我们不难注意到原始的 LBP 算子仅仅只是覆盖了很小的一个 $3\times3$ 邻域的范围，这显然不能满足提取不同尺寸纹理特征的需求。为了适应不同尺度的纹理特征，并达到灰度和旋转不变性的要求，Ojala 等对 LBP 算子进行了改进，将 $3\times3$ 邻域扩展到任意邻域，并用圆形邻域代替了正方形邻域。改进后的 LBP 算子允许在半径为 $R$ 的圆形邻域内有任意多个像素点。

对于没有落在像素格子内的，可以使用**双线性插值法**来计算该点的像素值。

#### 2.2 公式

圆形 LBP 跟原始公式很像，无非就是增加了两个变量：采样点数 $P$ 以及采样圆形邻域半径 $R$。表示成数学公式：

$$
 LBP_{P,R} (x_c, y_c) = \sum_{p=0}^{P-1} s(I(p) - I(c)) \times 2^p 
$$
圆形边界上的 $p$ 个点的坐标：

$$
\begin{cases} x_p = x_c + R \cdot \cos (\frac{2 \pi p}{P} ) \\ y_p = y_c - R \cdot \sin (\frac{2 \pi p}{P} ) \end{cases}
$$
$s(x)$ 公式与原始 LBP 中的一样：

$$
 s(x) = \begin{cases} 1, & x \geq 0 \\ 0, & \textrm{otherwise} \end{cases} 
$$

### 3. 旋转不变 LBP

#### 3.1 概念

从原始 LBP 的定义来看，LBP 算子是灰度不变的，但不是旋转不变的。图像旋转的话就会得到不同的 LBP 值。

Maenpaa 等人又将 LBP 算子进行了扩展，提出了具有旋转不变性的 LBP 算子，即不断旋转圆形邻域得到一系列初始定义的 LBP 值，取其最小值作为该邻域的 LBP 值。 

原始 LBP 得到的数值转化为二进制编码，对它进行循环移位操作，有 8 种情况（包括自身）。取其中最小的一个值，这个值是旋转不变的，因为对图像做旋转操作等价于上面 8 种移位的过程了，而 8 种情况都对应同一个值，即 8 个值中的最小值，即拥有了旋转不变特性。

#### 3.2 公式

LBP 值计算方式与原始的 LBP 一样，这里不做赘述。

区别在于对 LBP 的结果进行二进制编码，并做循环位移，取所有结果中最小的那个值：

$$
 LBP_{P,R}^{rot} = \min \{ ROR (LBP_{P,R}, i) \vert i = 0, 1, \cdots, P-1 \} 
$$

### 4. 均匀模式 LBP（uniform LBP）

#### 4.1 概念

基本 LBP 算子可以产生 8 位二进制数对应的 $2^8 - 1$ 种模式（<font color="blue">此处应该是 $2^8$ 种模式吧？</font>），对于半径为 $R$ 的圆形区域内含有 $P$ 个采样点，会有 $2^P - 1 $ 种模式（<font color="blue">此处应该是 $2^P$ 种模式吧？</font>）。很显然，随着采样点数 $P$ 的增加，二进制模式的种类是呈指数趋势增长的。过多的二进制模式对于纹理的表达是不利的，过于复杂且庞大的信息量，很明显违背了提取特征信息的初衷，我们需要的只是尽可能少且具有代表性的特征，因此需要对 LBP 得到的二进制模式种类进行降维，使用更少的数据量来最好地表示图像的信息。而这种降维的方法就是 **uniform LBP**。

**均匀模式 LBP（uniform LBP）**，就是限制一个二进制序列从 0 到 1 或从 1 到 0 的跳变次数不超过 2 次。

><font color="blue">为解决二进制模式过多的问题，提高统计性，Ojala 提出了采用一种“等价模式”（Uniform Pattern）来对 LBP 算子的模式种类进行降维。Ojala 等认为，在实际图像中，绝大多数 LBP 模式最多只包含两次从 1 到 0 或从 0 到 1 的跳变。因此，Ojala 将“**等价模式**”定义为：当某个 LBP 所对应的循环二进制数从 0 到 1 或从 1 到 0 最多有两次跳变时，该 LBP 所对应的二进制就称为一个等价模式类。如 00000000（0 次跳变），00000111（只含一次从 0 到 1 的跳变），10001111（先由 1 跳到 0，再由 0 跳到 1，共两次跳变）都是等价模式类。除等价模式类以外的模式都归为另一类，称为**混合模式类**，例如 10010111（共四次跳变）。通过这样的改进，二进制模式的种类大大减少，而不会丢失任何信息。模式数量由原来的 $2^P$ 种减少为 $P  \times (P-1)+2$ 种，其中 $P$ 表示邻域集内的采样点数。对于 $3\times3$ 邻域内 $8$ 个采样点来说，二进制模式由原始的 $256$ 种减少为 $58$ 种，即：它把值分为 $59$ 类，$58$ 个 uniform pattern 为一类，其它的所有值为第 $59$ 类。这样直方图从原来的 $256$ 维变成 $59$ 维。这使得特征向量的维数更少，并且可以减少高频噪声带来的影响。</font>

#### 4.2 公式

$$
U(LBP_{P,R}) = \left \vert s[I(P-1) - I(c)] - s[I(0) - I(c)] \right \vert + \sum_{p=1}^{P-1} \left \vert s[I(p) - I(c)] - s[I(p-1) - I(c)] \right \vert
$$

上式目的就是统计二进制数的跳变次数。

跳变次数小于等于 $2$，则各自代表一类，跳变次数大于 $2$ 的所有情况归为一类。

#### 4.3 实现

为简便起见，我是在原始 LBP 的基础之上做 uniform LBP 的。所以总共会有 $8$ 位二进制数，跳变数量小于等于 $2$ 的模式共有 $58$ 种（共有 <font color="red">$ P \times (P-1) + 3 $ </font>种），加上剩余的所有模式归为一种，加起来共 $59$ 种。我将跳变数量小于 $2$ 的模式分别定义为 $1 \sim 58$，剩余的情况，即第 $59$ 种情况，定义为 $0$。

### 5. 均匀模式+旋转不变模式 LBP

#### 5.1 概念

使用 uniform LBP 加上旋转不变模式 LBP 来提取特征，概念上面都介绍了，无非就是叠加起来而已。

#### 5.2 公式

先计算跳变次数：

$$
 U(LBP_{P,R}) = \vert s[I(P-1) - I(c)] - s[I(0) - I(c)] \vert + \sum_{p=1}^{P-1} \vert s[I(p) - I(c)] - s[I(p-1) - I(c)] \vert
$$
跳变次数小于等于 $2$，则各自代表一类，跳变次数大于 $2$ 的所有情况归为一类。再对其二进制编码做循环移位，求出最小值。

$$
 LBP_{P,R}^{rot, uni} = \min \{ ROR (LBP_{P,R}^{rot, uni}, i)\vert i = 0, \cdots, P-1 \}
$$

### 6. CLBP (Completed Local Binary Pattern)

zz-IP2010-19-6-A Completed Modeling of Local Binary Pattern Operator for Texture Classification.pdf

#### 6.1 Abstract

In this correspondence, a completed modeling of the local binary pattern (LBP) operator is proposed and an associated **completed LBP (CLBP)** scheme is developed for texture classification.

#### 6.2 Completed LBP (CLBP)

##### A. Local Difference Sign-Magnitude Transform (LDSMT)

Given a central pixel $g_c$ and its $P$ circularly and evenly spaced neighbors $g_p, p = 0, 1, \cdots, P-1$, we can simply calculate the difference between $g_c$ and $g_p$. The local difference vector $[d_0, d_1, \cdots, d_{P-1}]$ characterizes the image local structure at $g_c$. $d_p$ can be further decomposed into two components
$$
d_p = s_p * m_p \ \text{and} \ \begin{cases} s_p = \text{sign}(d_p) \\ m_p = \vert d_p \vert \end{cases}
$$
where $ s_p =  \begin{cases} 1, & d_p \geq 0 \\ -1, & d_p < 0 \end{cases} $, and $ m_p$ is the magnitude of $d_p$. So $[d_0, d_1, \cdots, d_{P-1}]$ is transformed into a sign vector $ [s_0, s_1, \cdots, s_{P-1}] $ and a magnitude vector $ [m_0, m_1, \cdots, m_{P-1}] $.

##### B. Analysis on Sign and Magnitude Components

Several issues need to be addressed for LBP-based feature representation:

1. Why LBP works reasonably well by using only the sign components of the local difference vector?
2. How to exploit the remaining information existed in the magnitude components?
3. Can we design a scheme to efficiently and conveniently fuse the sign-magnitude features?

The difference $d_p$ can be well modeled by Laplacian distribution $Q_L(g) = \exp(-|g|/\lambda)/2\lambda$, where $\lambda$ depends on the image content. It can be observed that the sign component $s_p$ follows a Bernoulli distribution $ Q_B (n) = b^{(1+n)/2} (1-b)^{(1-n)/2} $ with $ b = 0.5 $ and $ n\in \{-1,1\} $.

##### C. CLBP_S, CLBP_M and CLBP_C Operators

The CLBP framework is illustrated in following:

![](/pictures/20190125-clbpframework.png)

1. **CLBP_S and CLBP_M** 

   The **CLBP_S** operator is the same as the original LBP operator.

   Inspired by CLBP_S, **CLBP_M** operator is defined as follows:

$$
CLBP\_M_{P,R} = \sum_{p=0}^{P-1} t(m_p,c) 2^p, \quad t(x,c) = \begin{cases} 1, & x \geq c \\ 0, & x < c \end{cases}
$$

​	where $c$ is a threshold to be determined adaptively, here we set it as the mean value of $m_p$ from the whole image.

​	Similar to $\text{LBP}_{P,R}^{\text{riu2}}$, the rotation invariant version of $CLBP\_M_{P,R}$ denoted by $\text{CLBP_M}_{P,R}^{\text{riu2}}$.

2. **Fuse CLBP_S and CLBP_M vectors**

   There are two ways to combine CLBP_S and CLBP_M codes: **concatenation** and **jointly**. The first method represented as "<font color="red">CLBP_S_M</font>", we concatenate two histograms, whereas the second method represented as "<font color="red">CLBP_S/M</font>", we calculate a **joint 2-D histogram**.

3. **CLBP_C**

   We code **CLBP_C** as
   $$
   CLBP\_C_{P,R} = t(g_c, c_I)
   $$
   where the threshold $c_I$ is set as the average gray level of the whole image.

   > <font color="blue">Here, CLBP_S and CLBP_M are **gray image**, but CLBP_C is a **binary image**.</font>

4. **Fuse the three operators**

   Also there are two ways: **jointly** and **hybridly**. In the first way, we build a **3-D joint histogram**, denoted by "<font color="red">CLBP_S/M/C</font>". In the second way, CLBP_S/C or CLBP_M/C is built first, then the histograms are converted to a 1-D histogram,  which is then concatenated with CLBP_M or CLBP_S, denoted by "<font color="red">CLBP_M_S/C</font>" or "<font color="red">CLBP_S_M/C</font>".

   > 1. 2-D histogram is calculated in two dimensions. The similar is 3-D histogram.
   > 2. 2-D histogram can be stretched to 1-D histogram.

##### D. Dissimilarity Metric and Multi-Scale CLBP

There are various metrics such as **histogram intersection**, **log-likelihood ratio** and **chi-square statistic**.

Multi-resolution analysis could be used by employing multiple operators of various $(P,R)$. In the study, we measure the dissimilarity as the sum of *chi-square distances* from all operators
$$
D_Y = \sum_{y=1}^Y D(S^Y, Z^Y)
$$

### 7. LPQ (Local Phase Quantization)

#### 7.1 Abstract

In this paper, we propose a new descriptor for texture classification that is robust to image blurring. The descriptor utilizes phase information computed locally in a window for every image position.

#### 7.2 Blur Invariance Using Fourier Transform Phase 

The original image $f(\mathbf{x})$, observed image $g(\mathbf{x})$ and the point spread function (PSF) $h(\mathbf{x})$.
$$
g(\mathbf{x}) = (f * h) (\mathbf{x})
$$
In the Fourier domain, this corresponds to
$$
G(\mathbf{u}) = F(\mathbf{u}) \cdot H(\mathbf{u})
$$
where $G(\mathbf{u}) $, $F(\mathbf{u})$ and $H(\mathbf{u})$ are the discrete Fourier transforms (DFT) of $g(\mathbf{x})$, $f(\mathbf{x})$ and $h(\mathbf{x})$. We may separate the magnitude and phase parts, resulting in
$$
\begin{equation}
\begin{split}
\vert G(\mathbf{u}) \vert & = \vert F(\mathbf{u}) \vert \cdot \vert H(\mathbf{u}) \vert \\
\angle G(\mathbf{u}) & = \angle F(\mathbf{u}) + \angle H(\mathbf{u})
\end{split}
\end{equation}
$$

> <font color="blue">Why we can separate the magnitude and phase parts?</font>

Assume that the blur PSF $h(\mathbf{x})$ is centrally symmetric, namely $h(\mathbf{x}) = h(-\mathbf{x})$, its Fourier transform is always real-valued, and as a consequence its phase is only a two-valued function, given by
$$
\angle H(\mathbf{u}) = \begin{cases} 0 \quad & \text{if} \ H(\mathbf{u}) \geq 0 \\
\pi \quad & \text{if} \ H(\mathbf{u}) < 0 \end{cases}
$$
This means
$$
\angle G(\mathbf{u}) = \angle F(\mathbf{u}) \quad \text{for all $H(\mathbf{u}) \geq 0$}
$$

#### 7.3 Local Phase Quantization for Texture Classification

##### 1. Short-Term Fourier Transform

The local phase quantization (LPQ) method is based on the *blur invariance property* of the Fourier phase spectrum described in the above. A **short-term Fourier transform (STFT)** computed over a rectangular $M$-by-$M$ neighborhood $\mathcal{N}_{\mathbf{x}}$ at each pixel position $\mathbf{x}$ of the image $f(\mathbf{x})$ defined by
$$
F(\mathbf{u}, \mathbf{x}) = \sum_{\mathbf{y} \in \mathcal{N}_{\mathbf{x}}} f(\mathbf{x} - \mathbf{y}) e^{- j 2\pi \mathbf{u}^T \mathbf{y}} = \mathbf{w}_{\mathbf{u}}^T \mathbf{f}_\mathbf{x}
$$
where $\mathbf{w}_{\mathbf{u}}$ is the basis vector of the 2-D DFT at frequency $\mathbf{u}$, and $\mathbf{f}_{\mathbf{x}}$ is another vector containing all $M^2$ image samples from $\mathcal{N}_{\mathbf{x}}$.

In LPQ only four complex coefficients are considered, corresponding to 2-D frequencies $\mathbf{u}_1 = [a, 0]^T$, $\mathbf{u}_2 = [0, a]^T$, $\mathbf{u}_3 = [a, a]^T$, and $\mathbf{u}_4 = [a, -a]^T$, where $a$ is a scalar frequency below the first zero crossing of $H(\mathbf{u})$ that satisfies the condition ($18$). Let
$$
\begin{split} \mathbf{F}_{\mathbf{x}}^c & = [F(\mathbf{u}_1, \mathbf{x}), F(\mathbf{u}_2, \mathbf{x}), F(\mathbf{u}_3, \mathbf{x}), F(\mathbf{u}_4, \mathbf{x})] \\ \mathbf{F}_{\mathbf{x}} & = [\text{Re}\{\mathbf{F}_{\mathbf{x}}^c\}, \text{Im}\{\mathbf{F}_{\mathbf{x}}^c\}]^T \end{split}
$$
The corresponding $8$-by-$M^2$ transformation matrix is
$$
\mathbf{W} = [\text{Re}\{ \mathbf{w}_{\mathbf{u}_1}, \mathbf{w}_{\mathbf{u}_2}, \mathbf{w}_{\mathbf{u}_3}, \mathbf{w}_{\mathbf{u}_4} \}, \text{Im}\{ \mathbf{w}_{\mathbf{u}_1}, \mathbf{w}_{\mathbf{u}_2}, \mathbf{w}_{\mathbf{u}_3}, \mathbf{w}_{\mathbf{u}_4} \}]^T
$$
so that
$$
\mathbf{F_x} = \mathbf{W f_x}
$$

##### 2. Statistical Analysis of the Coefficients

Let us assume that the image function $f(\mathbf{x})$ is a result of a first-order Markov process, where the correlation coefficient between adjacent pixel values is $\rho$, and the variance of each sample is $\sigma^2$. Without a loss of generality we can assume that $\sigma^2 = 1$. As a result, the covariance between positions $\mathbf{x}_i$ and $\mathbf{x}_j$ becomes
$$
\sigma_{ij} = \rho^{\Vert\mathbf{x}_i - \mathbf{x}_j \Vert}
$$
where $\Vert \cdot \Vert $ denotes $L_2$ norm, and the covariance matrix of all M samples in $\mathcal{N}_{\mathbf{x}}$ can be expressed by
$$
\mathbf{C} = \begin{bmatrix} 1 & \sigma_{12} & \cdots & \sigma_{1M} \\ \sigma_{21} & 1 & \cdots & \sigma_{2M} \\ \vdots & \vdots & \ddots & \vdots \\ \sigma_{M1} & \sigma_{M2} & \cdots & \sigma_{MM} \end{bmatrix}
$$
Hence, the covariance matrix of the transform coefficient vector $\mathbf{F}_{\mathbf{x}}$ can be obtained from
$$
\mathbf{D} = \mathbf{WCW}^T
$$
One can easily notice that $\mathbf{D}$ is not a diagonal matrix for $\rho > 0$, meaning that the coefficients are correlating.

##### 3. Decorrelation and Quantization

Assuming Gaussian distribution, independence can be achieved using a whitening transform
$$
\mathbf{G_x} = \mathbf{V}^T \mathbf{F_x}
$$
where $\mathbf{V}$ is an orthonormal matrix derived from the singular value decomposition (SVD) of the matrix $\mathbf{D}$ that is
$$
\mathbf{D} = \mathbf{U \Sigma V}^T
$$
The resulting vectors are quantized using a simple scalar quantizer
$$
q_j = \begin{cases} 1, \quad & \text{if $g_j \geq 0$} \\ 0, \quad & \text{otherwise} \end{cases}
$$
where $g_j$ is the $j$th component of $\mathbf{G_x}$. The quantized coefficients are represented as integer values between $0-255$ using binary coding
$$
b = \sum_{j=1}^8 q_j 2^{j-1}
$$
Finally, a histogram of these integer values from all image positions is composed and used as a $256$-dimensional feature vector in classification.

### 8. GLCM (Gray-Level Cooccurrence Matrices)

Gray-level co-occurrence matrix (**GLCM**), also known as the gray-level spatial dependence matrix.

Consider the image (below left). If we use the position operator “<font color="magenta">1 pixel to the right and 1 pixel down</font>” then we get the *gray-level co-occurrence matrix (GLCM)* (below right)
$$
\begin{split} \begin{matrix} 0 & 0 & 0 & 1 & 2 \\ 1 & 1 & 0 & 1 & 1 \\ 2 & 2 & 1  & 0 & 0 \\ 1 & 1 & 0 & 2 & 0 \\ 0 & 0 & 1 & 0 & 1 \end{matrix} \qquad \qquad \qquad C = \frac{1}{16} \begin{bmatrix} 4 & 2 & 1 \\ 2 & 3 & 2 \\ 0 & 2 & 0 \end{bmatrix} \end{split}
$$
where an entry $c_{ij}$ is a count of the number of times that $F(x, y) = i$ and $F(x + 1, y + 1) = j$.

Now we can analyze $C$:

- maximum probability entry
- element difference moment of order $k$: $\sum_i \sum_j (i-j)^k c_{ij}$
- Contrast: $\sum_i \sum_j (i-j)^2 c_{ij}$, that is, when $k=2$.
- Entropy: $ - \sum_i \sum_j c_{ij} \log c_{ij}$. This is a measure of randomness, having its highest value when the elements of $C$ are all equal. Int he case of a checkerboard, the entropy would be low.
- Uniformity (also called Energy): $\sum_i \sum_j c_{ij}^2$. (smallest value when all entries are equal)
- Homogeneity: $\sum_i \sum_j \frac{c_{ij}}{1 + \vert i - j \vert}$ (large if big values are on the main diagonal)

**Problems** associated with the co-occurrence matrix methods:

1. They require a lot of computation (many matrices to be computed).
2. Features are not invariant to rotation or scale changes in the texture.

### 9. PFTAS (Parameter-Free Threshold Adjacency Statistics)

1. Applying a threshold to an image to create a binary image, pixels with intensity in the range $ [\mu-\sigma, \mu+\sigma] $ are shown in white, where $\mu$ is the average pixel intensity of each image.
2. For each white pixel, the number of adjacent white pixels is count (there are 9 cases). The nine statistics are normalized by dividing each by the total number of white pixels in the threshold image.
3. Two other sets of threshold adjacent statistics are also calculated as above, but for binary threshold images with pixels in the range $ [\mu-\sigma, 255] $ and $ [ \mu, 255] $, giving in total $27$ statistics.

> In the paper, $\sigma=30$.

### 10. ORB (Oriented FAST and Rotated BRIEF)

##### Reference:

1. [ORB特征点检测](http://www.cnblogs.com/ronny/p/4083537.html)
2. [图像处理检测方法 — ORB(Oriented FAST and Rotated BRIEF)](https://www.cnblogs.com/eilearn/p/9403878.html)
3. [ORB（Oriented FAST and Rotated BRIEF）](https://azurery.github.io/2018/11/05/ORB/)
4. [ORB算法原理解读](https://blog.csdn.net/yang843061497/article/details/38553765)

#### 1. FAST (Features from Accelerated Segment Test)

FAST 算子的**基本原理**是：若某像素点与其周围邻域内足够多的连续的像素点存在某一属性差异，并且该差异大于指定阈值，则可以断定该像素点与其邻域像素有可被识别的不同之处，可以作为一个特征点（角点）；对于灰度图像，FAST 算子考察的属性是像素与其邻域的灰度差异。 

这个检查过程可以用下图更清楚的描述：对于图像上所有像素点，考察其 $7\times7$ 邻域内以该点为圆心，半径是 $3$ 的圆周上的共计 $16$ 个像素点和中心点的差异。如果有连续的 $12$（或 $9$）个像素点与中心点的灰度差的绝对值大于或低于某一给定阈值，则该点被检测为 FAST 特征点。

为了提高检测速度，FAST 提出**分割测试**的概念，不是逐个遍历考察圆周上的 $16$ 个像素点，而是<font color="blue">先考察垂直和水平方向上的 $4$ 个点</font>，$1,9,5,13$ 进行测试（先测试 $1$ 和 $9$，如果它们符合阈值要求再测试 $5$ 和 $13$）。如果 $p$ 是角点，那么这四个点中<font color="blue">至少有 $3$ 个要符合阈值要求</font>。如果不是的话肯定不是角点，就放弃。对通过这步测试的点再继续进行测试（是否有 $12$ 个点符合阈值要求）。基于 FAST 算子要求圆周上<font color="blue">最少有 $12$（或 $9$）个连续的差异较大的点</font>，如果垂直和水平方向上 $4$ 个点中有 $2$ 个或 $2$ 个以上不满足要求的点，则可以直接判断该点不是 FAST 特征点，这样可以排除绝大部分非 FAST 特征点。进过初步筛选，再对剩下的符合条件的点实施 FAST 算子进行特征点检测，最后进行非极大值抑制后得到最终的特征点检测结果。

![](/pictures/20190125-fast.jpg)

FAST 速度较快。缺点是不具有方向性，尺度不变性。 

- 当 $n<12$ 时它不会丢弃很多候选点（获得的候选点比较多）。 
- 像素的选取不是最优的，因为它的效果取决于要解决的问题和角点的分布情况。 
- 高速测试的结果被抛弃。
- 检测到的很多特征点都是连在一起的。

前 3 个问题可以通过机器学习的方法解决，最后一个问题可以使用非最大值抑制的方法解决。

#### 2. BRIEF (Binary Robust Independent Elementary Features)

1. 取目标像素点一定范围内的邻域，一般是 $9 \times 9$。
2. 对该邻域进行高斯模糊处理，一般选核参数 $\sigma = 2$。
3. 以满足高斯分布的方式在该邻域内随机选取 $N$ 组像素点对，比较这两个像素点的灰度值大小，$x\geq y$ 则返回 $1$，$x< y$ 则返回 $0$（所有特征点的计算均采用统一的随机取样模板）。
4. 将步骤 3 的结果组合成一个 $N$ 位的二进制编码，即为目标像素点的特征值。根据精度和速度要求，一般 $N$ 会取 $256$ 和 $32$。

> 如何以满足高斯分布的方式随机选取 $N$ 组像素点？

BRIEF 不去计算描述符而是直接找到一个二进制字符串。这种算法使用的是已经平滑后的图像，它会按照一种特定的方式选取一组像素点对，然后在这些像素点对之间进行灰度值对比。例如，第一个点对的灰度值分别为 $p$ 和 $q$，如果 $p$ 小于 $q$，结果就是 $1$，否则就是 $0$。就这样对 $N$ 个点对进行对比得到一个 $N$ 维的二进制字符串。$N$ 可以是 $128,256,512$。OpenCV 对这些都提供了支持，但在默认情况下是 $ 256$（OpenCV 是使用字节表示它们的，所以这些值分别对应于 $16,32,64$）。当我们获得这些二进制字符串之后就可以使用**汉明距离**对它们进行匹配了。

非常重要的一点是：BRIEF 是一种特征描述符，它不提供查找特征的方法。所以我们不得不使用其他特征检测器，比如 SIFT 和 SURF 等。原始文献推荐使用 CenSurE 特征检测器，这种算法很快。而且 BRIEF 算法对 CenSurE关键点的描述效果要比 SURF 关键点的描述更好。 

原生的BRIEF的缺点也比较明显，不具备旋转不变性、尺度不变性，而且对噪声敏感，但是它算法简单而且计算复杂度低，计算速度很快。

#### 3. ORB (Oriented FAST and Rotated BRIEF)

首先它使用 FAST 找到关键点，然后再使用 Harris 角点检测对这些关键点进行排序找到其中的前 $N$ 个点。它也使用金字塔从而产生尺度不变性特征。但是有一个问题，FAST 算法不计算方向。那旋转不变性怎样解决呢？作者进行了如下修改：它使用灰度矩的算法计算出角点的方向，以角点到角点所在（小块）区域质心的方向为向量的方向。为了进一步提高旋转不变性，要计算以角点为中心半径为 $r$ 的圆形区域的矩，再根据矩计算方向。对于描述符，ORB 使用的是 BRIEF 描述符。但是我们已经知道 BRIEF对旋转是不稳定的。所以我们在生成特征前，要把关键点领域的这个 patch 的坐标轴旋转到关键点的方向。

假如有两张美女图片，要确认这两张图片中的美女是否是同一个人。如果由人来判断，则很简单。但是，让计算机来完成这个功能就困难重重。一种可行的方法是：

1. 分别找出两张图片中的特征点。
2. 描述这些特征点的属性。
3. 比较这两张图片的特征点的属性。如果有足够多的特征点具有相同的属性，那么就可以认为两张图片中的人物就是同一个人。

ORB（Oriented FAST and Rotated BRIEF）就是一种特征提取并描述的方法。ORB 分两部分，即**特征点提取**和**特征点描述**。

特征提取是由 FAST（Features from Accelerated Segment Test）算法发展来的，特征点描述是根据 BRIEF（Binary Robust Independent Elementary Features）特征描述算法改进的。ORB 特征是将 FAST 特征点的检测方法与 BRIEF 特征描述子结合起来，并在它们原来的基础上做了改进与优化。

##### 1. 特征点检测

1. 粗提取（FAST 具体计算过程）
   1. 从图片中选取一个像素点 $p$，接着判断它是否是一个特征点。首先，把它的密度（即灰度值）设为 $I_p$。
   2. 预先设定一个合适的阙值 $t$：当两个点的灰度值之差的绝对值大于 $t$ 时，便认为这两个点不相同。考
   3. 虑该像素点周围的 $16$ 个像素（以 $p$ 为圆心，画一个半径为 $3$ pixel 的圆）。
   4. 如果这 $16$ 个点中有连续的 $n$ 个点都和点 $p$ 不同，则点 $p$ 就是一个特征点（这里 $n$ 设定为 $12$）。
2. 使用 ID3 决策树，将特征点圆周上的 $16$ 个像素输入决策树中，以此来筛选出最优的 FAST 特征点。
3. 使用非极大值抑制算法去除临近位置的多个特征点。具体做法：为每一个特征点计算出其响应大小（即，特征点 $p$ 和其周围 $16$ 个特征点偏差的绝对值和）。在比较临近的特征点中，保留响应值较大的特征点，删除其余的特征点。
4. 特征点的尺度不变性。需要构造尺度金字塔：设置一个比例因子 factor（OpenCV 默认为 1.2）和金字塔的层数 octaves（OpenCV 默认为 8，与 SIFT 不同，每层仅有一幅图像）。将原图像按比例因子缩小成 octaves 幅图像。第 $s$ 层的尺度为 $\mathrm{scale}_x = \mathrm{factor}^s$，原图在第 $0$ 层；第 $s$ 层图像大小为：$\mathrm{Size}_s = \left (H \times \frac{1}{\mathrm{scale}} \right ) \times \left (W \times \frac{1}{\mathrm{scale}} \right ) $.

#### 2. Rotated BRIEF（rBRIEF）特征点的描述

BRIEF 算法的核心思想是：在关键点 $p$ 的周围，以一定模式选取 $N$ 个点对，把这 $N$ 个点对的比较结果组合起来作为描述子。

#### 3. 理想的特征点描述子应该具备的属性

理想的特征描述子应该在大小、方向、明暗不同的图像中，同一特征点应具有足够相似的描述子，称之为**描述子的可复现性**。

BRIEF 算法得到的描述子不具备以上性质。ORB 没有解决尺度一致性问题，在 OpenCV 的 ORB 实现中，采用了图像金字塔来改善这方面的性能。ORB 主要解决 BRIEF 描述子不具备旋转不变性的问题。

ORB 在计算 BRIEF 描述子时，建立的坐标系是以关键点为圆心，以关键点和取点区域的形心的连线为 $X$ 轴建立二维坐标系。

![](pictures/20190125-brief.png)

在上图中，$P$ 为关键点。圆内为取点区域，每个小格子代表一个像素。现在，把这块圆心区域看做一块木板，木板上每个点的质量等于其对应的像素值。根据积分学的知识，可以求出这个密度不均匀木板的质心 $Q$。计算公式如下（其中，$R$ 为圆的半径）：
$$
\begin{split}
M_{00} & = \sum_{x=-R}^R \sum_{y=-R}^R I(x,y) \\
M_{10} & = \sum_{x=-R}^R \sum_{y=-R}^R xI(x,y) \\
M_{01} & = \sum_{x=-R}^R \sum_{y=-R}^R yI(x,y) \\
Q_X & = \frac{M_{10}}{M_{00}}, \quad Q_Y = \frac{M_{01}}{M_{00}}
\end{split}
$$

#### 4. 特征点的匹配

ORB 算法最大的特点就是计算速度快。这首先得益于使用 FAST 检测特征点。其次是使用 BRIEF 算法计算描述子，该描述子特有的二进制串的表现形式，不仅节约了存储空间，而且大大缩短了匹配的时间。

例如，特征点 $A$、$B$ 的描述子如下：
$$
\begin{split}
A: 10101011 \\
B: 10101010
\end{split}
$$

现设定一个阈值，比如 $80\%$。在上述例子中，$A$，$B$ 只有最后一位不同，相似度为 $87.5\%$，大于 $80\%$，则 $A$ 和 $B$ 是匹配的。

将 $A$ 和 $B$ 进行**异或**操作，就可以轻松计算出 $A$ 和 $B$ 的相似度。而异或操作可以借组硬件完成，具有很高的效率，加快了匹配的速度。