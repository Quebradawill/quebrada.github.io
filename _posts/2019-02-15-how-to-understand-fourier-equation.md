---
layout: post
title: 如何理解傅里叶级数公式
date: 2019-02-15
categories: Ma-Student
tags: Fourier
status: publish
type: post
published: true
author: Quebradawill
---

### 1. 对周期函数进行分解的猜想

拉格朗日等数学家发现某些周期函数可以由**三角函数的和**来表示，比如下图中，黑色的斜线就是周期为 $2\pi$ 的函数，而红色的曲线是三角函数之和，可以看出两者确实近似：

![](/pictures/20190215-1.png width='70%' align='center')

而另外一位数学家*让·巴普蒂斯·约瑟夫·傅立叶男爵*（1768 －1830）猜测任意周期函数都可以写成三角函数之和。

### 2. 分解的思路

假设 $f(x)$ 是周期为 $T$ 的函数，傅立叶男爵会怎么构造三角函数的和，使之等于 $f(x)$？

#### 2.1 常数项

对于 $y=C, C \in \Bbb{R}$ 这样的常数函数，根据周期函数的定义，常数函数是周期函数，周期为任意实数。所以，分解里面得有一个**常数项**。

#### 2.2 通过 $\sin (x), \cos (x)$ 进行分解

首先，$\sin(x), \cos(x)$ 是周期函数，进行合理的加减组合，结果可以是周期函数。其次，它们的微分和积分都很简单。然后，$\sin(x)$ 是奇函数，即：$ -\sin(x) = \sin(-x)$

奇函数与奇函数加减只能得到奇函数，即：$ f_{\rm{odd}} \pm f_{\rm{odd}} = f_{\rm{odd}}$

而 $ \cos (x) $ 是偶函数，即：$ \cos (x) = \cos (-x) $

同样的，偶函数与偶函数加减只能得到偶函数，即：$  f_{\rm{even}} \pm f_{\rm{even}} = f_{\rm{even}} $

但是任意函数可以分解和奇偶函数之和：
$$
f(x) = \frac{f(x) + f(-x)}{2} + \frac{f(x) - f(-x)}{2} = f_{\rm{even}} + f_{\rm{odd}}
$$

#### 2.3 保证组合出来周期为 $T$

之前说了，$f(x)$ 是周期为 $T$ 的函数，我们怎么保证组合出来的函数周期依然为 $T$ 呢？

比如下面这个函数的周期为 $ 2 \pi$：

![](/pictures/20190215-2.png width="60%" align="center")

很显然，$\sin(x)$ 的周期也是$ 2\pi$：

![](/pictures/20190215-3.png width="60%" align="center")

$\sin(2x)$ 的周期也是 $2\pi$，虽然最小周期是 $\pi$：

![](/pictures/20190215-4.png width="60%" align="center")

很显然，$\sin(nx), n\in\Bbb{N}$ 的周期都是 $2\pi$：

![](/pictures/20190215-5.png width="60%" align="center")

更一般的，如果 $f(x)$ 的周期为 $T$，那么：
$$
\sin \left({\frac{2\pi n}{T}x} \right) \quad \cos \left({\frac{2\pi n}{T}x} \right), \quad n\in\Bbb{N}
$$
这些函数的周期都为 $T$。将这些函数进行加减，就保证了得到的函数的周期也为 $T$。

#### 2.4 调整振幅

现在我们有一堆周期为 $2\pi$ 的函数了，比如说 $\sin(x),\sin(2x),\sin(3x),\sin(4x),\sin(5x)$，通过调整振幅可以让它们慢慢接近目标函数。

#### 2.5 小结

综上，构造出来的三角函数之和大概类似下面的样子：
$$
\displaystyle f(x)=C+\sum _{{n=1}}^{\infty}\left(a_{n} \cos \left({\frac{2\pi n}{T}x} \right)+b_{n} \sin \left({\frac{2\pi n}{T}x} \right) \right),C\in\Bbb{R}
$$
这样就符合之前的分析：

- 有常数项
- 奇函数和偶函数可以组合出任意函数
- 周期为 $T$
- 调整振幅，逼近原函数

之前的分析还比较简单，后面开始有点难度了。即怎么确定这三个系数：$C, a_n, b_n$

### 3. $ \sin(x)$ 的另外一种表示方法

#### 3.1 $e^{i \omega t}$

看到复数也不要怕，根据之前的文章[如何通俗易懂地解释欧拉公式](https://www.matongxue.com/madocs/8.html)，看到类似于 $e^{i\theta}$ 这种就应该想到复平面上的一个夹角为 $\theta$ 的向量：

![](/pictures/20190215-6.png width="40%" align="center")

那么当 $\theta$ 不再是常数，而是代表时间的变量 $t$ 的时候：
$$
e^{i\theta}\to e^{i{\color{red}t}}
$$

随着时间 $t$ 的流逝，从 $0$ 开始增长，这个向量就会旋转起来，$2\pi$ 秒会旋转一圈，也就是 $T=2\pi$

#### 3.2 通过 $ e^{i \omega t}$ 表示 $\sin (x)$

根据欧拉公式，有：
$$
e^{it}= \cos(t)+i \sin(t)
$$
所以，在时间 $t$ 轴上，把 $e^{it}$ 向量的虚部（也就是纵坐标）记录下来，得到的就是 $\sin(t)$

![](/pictures/20190215-7.png width="60%" align="center")

在时间 $t$ 轴上，把 $e^{it}$ 向量的实部（横坐标）记录下来，得到的就是 $\cos(t)$

更一般地我们认为，我们具有两种看待 $ \sin(x),\cos(x)$ 的角度：
$$
e^{i\omega t}\iff \begin{cases} \sin(\omega t)\\ \cos(\omega t)\end{cases}
$$


这两种角度，一个可以观察到旋转的频率，所以称为**频域**；一个可以看到流逝的时间，所以称为**时域**：

![](/pictures/20190215-8.png width="60%" align="center")

> 即两种角度（频域和时域）分别来看 $\sin (x) $ 和 $\cos (x) $

### 4. 通过频域来求系数

#### 4.1 函数是线性组合

假设有这么个函数：
$$
g(x)= \sin(x)+ \sin(2x)
$$
是一个 $T=2\pi$ 的函数：

![](/pictures/20190215-9.png width="60%" align="center")

如果转到频域去，那么它们是下面这个复数函数的虚部：$e^{it}+e^{i2t}$

先看看 $e^{i\theta}+e^{i2\theta}$，其中 $ \theta$ 是常数，很显然这是两个向量之和：

![](/pictures/20190215-10.png width="40%" align="center")

现在让它们动起来，把 $\theta$ 变成流逝的时间 $t$，并且把虚部记录下来：

![](/pictures/20190215-11.png width="70%" align="center")

我们令：
$$
G(t)=e^{it}+e^{i2t}
$$
这里用大写的 $G$ 来表示复数函数。刚才看到了，$e^{it}$ 和 $e^{i2t}$ 都是向量，所以上式可以写作：
$$
\vec{G(t)}=\vec{e^{it}}+\vec{e^{i2t}}
$$
这里就是理解的重点了，从线性代数的角度：

- $e^{it}$ 和 $e^{i2t}$ 是基（可以参考[无限维的希尔伯特空间](https://ccjou.wordpress.com/2009/08/18/%E5%BE%9E%E5%B9%BE%E4%BD%95%E5%90%91%E9%87%8F%E7%A9%BA%E9%96%93%E5%88%B0%E5%87%BD%E6%95%B8%E7%A9%BA%E9%96%93/)）
- $G(t)$ 是基 $ e^{it}$，$e^{i2t}$ 的线性组合

$g(t)$ 是 $G(t)$ 的虚部，所以取虚部，很容易得到：
$$
\vec{g(t)}=\vec{\sin(t)}+\vec{\sin(2t)}
$$
即 $g(t)$ 是基 $ \sin(t),\sin(2t)$ 的线性组合。那么 $ \sin(t),\sin(2t)$ 的系数，实际上是 $g(t)$ 在基 $ \sin(t),\sin(2t)$ 下的坐标了。

#### 4.2 如何求正交基的坐标

假设：$\vec{w_{}}=2\vec{u_{}}+3\vec{v_{}}$，其中 $\vec{u_{}}=\begin{pmatrix}1\\1\end{pmatrix},\vec{v_{}}=\begin{pmatrix}-1\\1\end{pmatrix}$

通过点积（关于点积可以参考如何理解协方差、相关系数和点积？）：$\vec{u_{}}\cdot \vec{v_{}}=0$，可知这两个向量正交，是正交基。

通过点积可以算出 $\vec{u_{}}$ 的系数（对于**正交基**才可以这么做）：
$$
\frac{\vec{w_{}}\cdot \vec{u_{}}}{\vec{u_{}}\cdot \vec{u_{}}}=\frac{(1,5)\cdot(-1,1)}{(-1,1)\cdot(-1,1)}=2
$$

#### 4.3 如何求 $\sin(nt)$ 基下的坐标

在这里抛出一个结论（可以参考[无限维的希尔伯特空间](https://ccjou.wordpress.com/2009/08/18/%E5%BE%9E%E5%B9%BE%E4%BD%95%E5%90%91%E9%87%8F%E7%A9%BA%E9%96%93%E5%88%B0%E5%87%BD%E6%95%B8%E7%A9%BA%E9%96%93/)），函数向量的点积是这么定义的：
$$
\vec{f(x)}\cdot\vec{g(x)}=\int_{0}^{T}f(x)g(x)dx
$$
其中，$f(x)$ 是函数向量，$g(x)$ 是基，$T$ 是 $f(x)$ 的周期。那么对于：
$$
g(x)=\sin(x)+\sin(2x)
$$
其中，$g(x)$ 是向量，$\sin(t),\sin(2t)$ 是基，周期 $T=2\pi$。根

据刚才内积的定义：
$$
\vec{\sin(t)}\cdot\vec{\sin(2t)}=\int_{0}^{2\pi}\sin(t)\sin(2t)dt=0
$$
所以是正交基，那么根据刚才的分析，可以这么求坐标：
$$
\frac{\vec{g_{}}\cdot \vec{\sin(t)}}{\vec{\sin(t)}\cdot \vec{\sin(t)}}=\frac{\int_{0}^{2\pi}g(x)\sin(x)dx}{\int_{0}^{2\pi}\sin^2(x)dx}=1
$$

#### 4.4 更一般的

对于我们之前的假设，其中 $f(x)$ 周期为 $T$：
$$
\displaystyle f(x)=C+\sum _{{n=1}}^{\infty}\left(a_{n}\cos \left({\frac{2\pi n}{T}x} \right)+b_{n}\sin \left({\frac{2\pi n}{T}x} \right)\right),C\in\mathbb{R}
$$
可以改写为这样：
$$
\displaystyle f(x)=C\cdot 1+\sum _{{n=1}}^{\infty}\left(a_{n} \cos \left ({\frac{2\pi n}{T}x} \right)+b_{n}\sin \left({\frac{2\pi n}{T}x} \right)\right),C\in\mathbb{R}
$$
也就是说向量 $f(x)$ 的基为：
$$
\left \{1,\cos \left({\frac{2\pi n}{T}x} \right), \sin \left ({\frac{2\pi n}{T}x} \right) \right \}
$$
那么可以得到：
$$
a_n=\frac{\int_{0}^{T}f(x)\cos({\frac{2\pi n}{T}x})dx}{\int_{0}^{T}\cos^2({\frac{2\pi n}{T}x})dx}=\frac{2}{T}\int_{0}^{T}f(x)\cos({\frac{2\pi n}{T}x})dx
$$

$$
b_n=\frac{\int_{0}^{T}f(x)\sin({\frac{2\pi n}{T}x})dx}{\int_{0}^{T}\sin^2({\frac{2\pi n}{T}x})dx}=\frac{2}{T}\int_{0}^{T}f(x)\sin({\frac{2\pi n}{T}x})dx
$$

$C$ 也可以通过点积来表示，最终我们得到：
$$
\displaystyle f(x)={\frac{a_{0}}{2}}+\sum _{{n=1}}^{\infty}\left(a_{n}\cos({\tfrac  {2\pi nx}{T}})+b_{n}\sin({\tfrac  {2\pi nx}{T}})\right)
$$
其中：
$$
\displaystyle 
a_{n}={\frac  {2}{T}}\int _{{x_{0}}}^{{x_{0}+T}}f(x)\cdot \cos({\tfrac  {2\pi nx}{T}})\ dx, \quad n\in\{0\}\cup\mathbb{N}\\
b_{n}={\frac  {2}{T}}\int _{{x_{0}}}^{{x_{0}+T}}f(x)\cdot \sin({\tfrac  {2\pi nx}{T}})\ dx, \quad n\in\mathbb{N}
$$
怎么把傅立叶级数推导到傅立叶变换，请参看：[从傅立叶级数到傅立叶变换](https://www.matongxue.com/madocs/712.html)。