---
layout: post
title: Deep Learning 课程 - 七月在线
date: 2019-12-26
categories: Deep-learning
tags: PyTorch Tensorflow
status: publish
type: post
published: true
author: Quebradawill
---

# 第 1 周：概率论和微积分

## 1. 微积分和线性代数

### 1.1 局部极值算法：牛顿法

- 这种方法只能寻找局部极值；
- 必须给出一个初始点 $x_0$；
- 数学原理：牛顿法使用二阶逼近；
- 对局部凸的函数找到极小值，对局部凹的函数找到极大值，对局部不凸不凹的可能会找到鞍点；
- 要求估计二阶导数。

首先在初始点 $x_0$ 处，写出二阶泰勒级数


$$
f(x_0 + \Delta_x) = f(x_0) + f^{'}(x_0) \Delta_x + \frac{f^{''}(x_0)}{2} \Delta_x^2 + \omicron (\Delta_x^2) = g(\Delta_x) + \omicron (\Delta_x^2)
$$


关于 $\Delta_x$ 的二次函数 $g(\Delta_x)$ 的极值点为 $ - \frac{f^{'}(x_0)}{f^{''}(x_0)}$，那么本着逼近的精神，$f(x) $ 的极值点估计在 $x_0 - \frac{f^{'}(x_0)}{f^{''}(x_0)}$ 附近，于是定义 $x_1 = x_0 - \frac{f^{'}(x_0)}{f^{''}(x_0)}  $，并重复此步骤得到序列


$$
x_n = x_{n-1} - \frac{f^{'}(x_{n-1})}{f^{''}(x_{n-1})}
$$


当初始点选的比较好的时候，$\lim_{n \to \infty} x_n $ 收敛于一个局部极值点。

### 1.2 方阵的相似变换

**定义：**如果两个方阵满足 $\tilde{A} = P^{-1} A P $，那么这两个方阵互为相似矩阵。

相似矩阵的**几何意义**是同一个线性变换在不同基下的表达形式。

当研究对象是线性变换的时候，我们只关心矩阵在相似变换下不变的几何性质：

- 行列式（determinant）


$$
\begin{array}
\\ \det (P^{-1} A P) & = \det(P^{-1}) \det(A) \det(P) \\ & =  \det(P^{-1}) \det(P) \det(A) \\ & = \det(A) \end{array}
$$


- 迹（trace）$ {\rm tr} (AB) = {\rm tr} (BA)$，${\rm tr} (P^{-1} A P) = {\rm tr} (AP P^{-1}) = {\rm tr} (A \cdot I) = {\rm tr} (A)$
- 秩（rank）
- 特征值：特征方程 $ \det (A - \lambda I)  = 0$ 的根。如果 $ \det (A - \lambda I) = 0 $，那么 $\det \left ( P^{-1} (A - \lambda I) P \right) = 0$，于是 $  \det (P^{-1} A P - \lambda I)  = 0 $。

<font color = 'blue'>特征值是最重要的相似不变量，利用这个相似不变量可以方便地得出上面所有的不变量。</font>

<font color='red'>相合变换（二次型）</font>：假设 $V$ 是一个实系数线性空间，那么线性空间上的**度量**指的是空间中向量的内积关系 $G(v_1,v_2)$。如果 $\alpha \{\alpha_1, \cdots, \alpha_k \}$ 是空间 $V$ 的一组基，那么这个内积一般可以用一个对称矩阵 $H_{\alpha} = [h_{ij}]_{n \times n}$ 来表示。


$$
h_{ij} = G(\alpha_i, \alpha_j)
$$


这时候对于任意两个向量 $v_1, v_2$，如果 $v_1 = \alpha \cdot x_1$，$v_2 = \alpha \cdot x_2$，那么


$$
G(v_1, v_2) = x_1^T H_{\alpha} x_2
$$


<font color='blue'>有什么几何意义呢？</font>

方阵的相合变换：

- 如果两个对称矩阵 $A$ 和 $\tilde{A}$ 满足，$\tilde{A} = P^T AP$，那么这两个方阵将互为相合矩阵；
- 相似矩阵的几何意义是同一个内积结构在不同基下的表示形式。

方阵的正交相似变换：

- 正交相似变换同时满足相似与相合变换的条件，也就是说它同时保持了矩阵的相似与相合不变量。
- 如果两个对称方阵 $A$ 和 $\tilde{A}$ 满足，$\tilde{A} = P^T AP$，而且 $P$ 是正交矩阵，$P^T = P^{-1}$，那么这两个矩阵将互为正交相似。

方阵的正交相似标准型：任何一个对称矩阵 $A$ 都可以正交相似于一个对角矩阵 $D$。总存在一个正交矩阵 $P$ 使得，$A = P^T DP$。

长方矩阵的奇异值分解（SVD）：对于任何一个矩阵 $B_{m \times n}$，存在正交矩阵 $P_{m \times m}$，$Q_{n \times n}$，使得 $B = PDQ$，其中 $D_{m \times n}$ 是一个只有对角元素不为零的矩阵。

主成分分析（PCA）：主要目的是降维，也可以起到分类的作用。

- 当数据维度很大的时候，大部分变量之间可能存在线性关系，希望降低维数，用较少的变量来抓住大部分的信息；
- 一般 PCA 之前要做 normalization，使得变量均值为 $0$，方差为 $1$。

主成分分析做法：首先计算变量之间的协方差矩阵 $\Sigma$（利用样本），然后找到 $\Sigma$ 的正交相似标准型。

## 2. 微积分

微分学的核心思想是用<font color='red'>熟悉且简单</font>的函数对复杂函数进行<font color='red'>局部</font>逼近。

重要极限（两边夹定理应用）：

- 三角函数


$$
\lim_{x \to 0} \frac{\sin (x)}{x} = 1
$$


- 自然对数底数


$$
e = \lim_{n \to \infty} \left( 1 + \frac{1}{n} \right)^n
$$


- 指数函数


$$
\lim_{x \to 0} \frac{e^x - 1}{x} = 1
$$


梯度（以二元函数为例）：对于一个可微函数 $f(x,y)$，梯度定义为 $ \nabla f(x,y) = (\partial_x f, \partial_y f)^T$。

- 梯度的代数意义：任意方向的偏导数可以由梯度来表示，如果 $v = (a,b)$


$$
\nabla_v f(x,y) = v \cdot \nabla f(x, y) = a \partial_x f(x,y) + b \partial_y f(x,y)
$$


- 梯度的几何意义：梯度方向就是函数增长最快的方向。

### 2.1 梯度下降法和牛顿法

梯度下降法：对函数进行一阶逼近寻找函数下降最快的方向。

牛顿法：对函数的二阶逼近，并直接估计函数的极小值点


$$
f(\theta_0 + \Delta \theta) = f(\theta_0) + \Delta \theta^T \cdot \nabla J(\theta_0) + \frac12 \Delta \theta^T \nabla^2 J(\theta_0) \Delta \theta + \omicron (\vert \Delta \theta \vert^2)
$$


于是关于 $\Delta \theta $ 的梯度为


$$
\nabla J(\theta_0) + \nabla^2 J(\theta_0) \Delta \theta + \omicron (\vert \Delta \theta \vert)
$$


零点的近似值为


$$
\Delta \theta = \left(\nabla^2 J(\theta_0) \right)^{-1} \cdot \nabla J(\theta_0)
$$

**困难：**

- 梯度的计算：样本量很大时，梯度的计算非常耗时耗力；
- 学习率的选择：过小算法收敛太慢，过大易导致算法不收敛。

### 2.2 随机梯度下降法

随机梯度下降法主要为了解决第一个问题：梯度计算。

由于随机梯度下降法的引入，通常将梯度下降法分为三种类型：

- 批梯度下降法（GD），原始的梯度下降法；
- 随机梯度下降法（SGD），每次梯度计算只使用一个样本

## 3. 概率论