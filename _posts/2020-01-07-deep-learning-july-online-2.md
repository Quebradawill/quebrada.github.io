---
layout: post
title: Deep Learning 课程 - PART 2 - 七月在线
date: 2020-01-07
categories: Deep-learning
tags: PyTorch Tensorflow 七月在线
status: publish
type: post
published: true
author: Quebradawill
---

# 第 2 课：Softmax，DNN，Wide & Deep Model

## 1. 深度学习与应用

## 2. 一点基础：线性分类器

损失函数（loss function）、代价函数（cost function）、客观度（objective）。

$f(x, W) = Wx$

**损失函数1：hinge loss/支持向量机损失**

$L_i = \sum_{j \neq y_i} \max \left( 0, f(x_i, W)_j - f(x_i, W)_{y_i} + \Delta \right)$

- 因为是线性模型，可以简化成 $L_i = \sum_{j \neq y_i} \max \left( 0, w_j^T x_i - w_{y_i}^T x_i + \Delta \right)$
- 加正则化项：$L = \frac1N \sum_i L_i + \lambda R(W)$

**损失函数2：交叉熵损失（softmax 分类器）**

对于训练集中的第 $i$ 张图片数据 $x_i$，在 $W$ 下会一个得分结果向量 $f_{y_i}$，则损失函数记作：


$$
L_i = - \log \left( \frac{e^{f_{y_i}}}{\sum_j e^{f_j}} \right)
$$


或者


$$
L_i = - f_{y_i} + \log \sum_j e^{f_j}
$$


实际工程中一般这样计算：


$$
\frac{e^{f_{y_i}}}{\sum_j e^{f_j}} = \frac{C e^{f_{y_i}}}{C \sum_j e^{f_j}} = \frac{e^{f_{y_i} + \log C}}{\sum_j e^{f_j + \log C}}
$$


### 3. 神经网络表达力与过拟合

