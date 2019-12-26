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