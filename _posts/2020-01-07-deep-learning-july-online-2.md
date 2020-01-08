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

$$L_i = \sum_{j \neq y_i} \max \left( 0, f(x_i, W)_j - f(x_i, W)_{y_i} + \Delta \right)$$

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

- 理论上说单隐层神经网络可以逼近任何连续函数（只要隐层的神经元个数足够多）。
- 虽然从数学上看表达能力一致，但是多隐藏层的神经网络比单隐藏层的神经网络工程效果好很多。
- 对于一些分类数据（比如 CTR 预估里），3 层神经网络效果优于 2 层神经网络，但是如果把层数再不断增加（4、5、6 层），对最后结果的帮助就没有那么大的跳变了。
- 图像数据比较特殊，是一种深层（多层次）的结构化数据，深层次的卷积神经网络，能够更充分、更准确地把这些层级信息表达出来。
- 提升隐层层数或者隐层神经元个数，神经网络“容量”会变大，空间表达力会变强。
- 过多的隐层和神经元节点，会带来过拟合问题。
- 不要试图通过降低神经网络参数量来减缓过拟合，用正则化或者 dropout。

### 4. 神经网络之传递函数

- S 函数（Sigmoid）


$$
f(x) = \frac{1}{1 + e^{-x}}
$$


- 双 S 函数


$$
f(x) = \frac{1 - e^{-x}}{1 + e^{-x}}
$$


### 5. 神经网络之 BP 算法

- “正向传播”求损失，“反向传播”回传误差

BP 算法，也叫 $\delta$ 算法。

