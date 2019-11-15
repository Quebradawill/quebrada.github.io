---
layout: post
title: 计算机辅助设计与图形学学报 2019 年图像分割论文
date: 2019-11-15
categories: Papers
tags: Segmentation
status: publish
type: post
published: true
author: Quebradawill
---

### 1. 结合超像素和 U 型全卷积网络的胰腺分割方法

#### 1.1 本文方法

**Step 1. 图像预处理**

Step 1.1 提出改进的线性谱聚类（Linear Spectral Clustering，LSC）超像素分割方法，通过改进距离度量方式达到可以实现医学图像超像素分割的目的，也就是把彩色度量换成灰度度量；


$$
d(p,q) = C_s^2 \left ( \cos \frac{\pi}{2} (x_p - x_q) + \cos \frac{\pi}{2} (y_p - y_q) \right) + C_c^2 \left ( \cos \frac{\pi}{2} (g_p - g_q) \right)
$$


像素的颜色距离发生改变，原来的颜色距离是


$$
d_{c_1} = \cos \frac{\pi}{2} (l_p - l_q) + 2.55^2 \left( \cos \frac{\pi}{2} (a_p - a_q) + \cos \frac{\pi}{2} (b_p - b_q) \right)
$$


要同时实现高规则性（Superpixel Compactness，SC）和高边界召回率（Boundary Recall，BR），提出了新的评价指标 $I = 1 - BR / CP$，采用模拟退火求取 $r = C_s / C_c $。

Step 1.2 图像映射降维，用超像素灰度平均值，重新赋值给超像素的每一像素，生成视觉概要图；

**Step 2. 改进的 U-NET 网络**

U-NET 由一个收缩路径（左路）和一个扩张路径（右路）组成，它能够同时结合底层和高层信息，底层信息有助于提高精度，高层信息用来提取复杂特征。提出了一种加快速度的方法，只对超像素内的其中一个像素进行卷积分类，得出超像素的标签。

#### 1.2 实验结果和分析

评价指标包括欠分割率（Under-segmentation Error，UE）、BR、分割准确率（Achievable Segmentation Accuracy，ASA）和超像素形状规则性（Superpixel Compactness，CP）。


$$
\textrm{UE} = \frac{U_S}{R_S + O_S}
$$

$$
\textrm{BR}(S,G) = \frac{\textrm{TP}(S,G)}{\textrm{TP}(S,G) + \textrm{FN}(S,G)}
$$

$$
\textrm{ASA} = \left( 1 - \frac{\vert R_S - T_S \vert}{R_S} \right) \times 100\%
$$

$$
 \textrm{CP} = \sum_{S \in \mathcal{E}} Q_S \cdot \frac{\vert S \vert}{\vert I \vert}
$$


### 2

