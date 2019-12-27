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

## 第 1 周：概率论和微积分

### 1. 局部极值算法：牛顿法

- 这种方法只能寻找局部极值；<br>
- 必须给出一个初始点 $x_0$；<br>
- 数学原理：牛顿法使用二阶逼近；<br>
- 对局部凸的函数找到极小值，对局部凹的函数找到极大值，对局部不凸不凹的可能会找到鞍点；<br>
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

### 2. 方阵的相似变换

**定义：**如果两个方阵满足 $\tilde{A} = P^{-1} A P $，那么这两个方阵互为相似矩阵。

相似矩阵的**几何意义**是同一个线性变换在不同基下的表达形式。

当研究对象是线性变换的时候，我们只关心矩阵在相似变换下不变的几何性质：

- 行列式（det）


$$
\begin{array} \det (P^{-1} A P) & = \det(P^{-1}) \det(A) \det(P) \\ & =  \det(P^{-1}) \det(P) \det(A) \\ & = \det(A) \end{array}
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

