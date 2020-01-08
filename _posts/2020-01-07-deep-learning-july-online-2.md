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

## 第 2 课：Softmax，DNN，Wide & Deep Model

### 1. 深度学习与应用

### 2. 一点基础：线性分类器

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

“正向传播”求损失，“反向传播”回传误差

BP 算法，也叫 $\delta$ 算法。

## 第 3 课：卷积神经网络与典型结构

### 1. 神经网络与卷积神经网络

#### 1.1 卷积神经网络层次

- 数据输入层（Input Layer）
- 卷积计算层（CONV Layer）
- 激励层（Activation Layer）
- 池化层（Pooling Layer）
- 全连接层（FC Layer）
- Batch Normalization 层（可能有）

**1. 数据输入层**

数据输入层有 3 种常见的数据处理方式：

- **去均值**，各个维度都中心化到 0；
- **归一化**，幅度归一化到同样的范围；
- **PCA/白化**，用 PCA 降维，白化是对数据每个特征轴上的幅度归一化。<font color='blue'>这跟前面的归一化有什么不同？</font>

```python
# decorrelated data
X -= np.mean(X, axis=0)
cov = np.dot(X.T, X) / X.shape[0]
U, S, V = np.linalg.svd(cov)
X_rot = np.dot(X, U)

# whitened data
W_white = X_rot / np.sqrt(S + 1e-5)
```

<font color='blue'>可以试一下代码，看一下原图。</font>

**2. 卷积计算层**

- 局部关联。每个神经元看做一个 filter。
- 窗口（receptive field）滑动，filter 对局部数据计算。
- 涉及概念
  - 深度/depth
  - 步长/stride
  - 填充值/zeor-padding
- 参数共享机制，假设每个神经元连接数据窗的权重是固定的。

**3. 激励层**

把卷积层输出结果做非线性映射。Sigmoid，Tanh，ReLU，Leaky ReLU，ELU，Maxout，……

- CNN 慎用 Sigmoid！慎用 Sigmoid！慎用 Sigmoid！
- 首先试 ReLU，因为快，但要小心点。
- 如果 ReLU 失效，请用 Leaky ReLU 或者 Maxout。
- 某些情况下 tanh 倒是有不错的结果，但是很少。

**4. 池化层**

夹在连续的卷积层中间，压缩数据和参数的量，减小过拟合。Max pooling 和 Average pooling。

**5. 全连接层**

通常 FC 在 CNN 的尾部。

**6. 典型的 CNN 的结构**

$ \textrm{INPUT} \to [[\textrm{CONV} \to \textrm{ReLU}] * N] \to \textrm{POOL}?] * M \to [\textrm{FC} \to \textrm{ReLU}] * K \to \textrm{FC} $

#### 1.2 卷积神经网络优缺点

优点：

- 共享卷积核，优化计算量
- 无需手动选取特征，训练好权重，即得特征
- 深层次的网络抽取图像信息丰富，表达效果好

缺点：

- 需要调参，需要大样本量，GPU 等硬件依赖
- 物理含义不明确

### 2. 正则化与 Dropout

#### 2.1 Dropout

**Dropout：**randomly set some neurons to zero in the forward pass

防止过拟合的第 1 种理解方式：

- 别让你的神经网络记住那么多东西（虽然 CNN 记忆力好）
- 学习过程中，保持泛化能力

防止过拟合的第 2 种理解方式：

- 每次都关掉一部分感知器，得到一个新模型，最后做融合。不至于听一家之言。

#### 2.2 典型 CNN

- LeNet，这是最早用于数字识别的 CNN
- AlexNet，2012 ILSVRC 比赛远超第 2 名的 CNN， 比 LeNet 更深， 用多层小卷积层叠加替换单大卷积层。
- ZF Net，2013 ILSVRC 比赛冠军
- GoogLeNet，2014 ILSVRC 比赛冠军
- VGGNet，2014 ILSVRC 比赛中的模型， 图像识别略差于 GoogLeNet， 但是在很多图像转化学习问题（比
  如 object detection）上效果很好
- ResNet，即 Deep Residual Learning Network，微软亚洲研究院提出，2015 ILSVRC 比赛冠军， 结构修正（残差学习）以适应深层次 CNN 训练。比 VGG 还要深 8 倍。

### 3. 典型结构与训练

## 第 4 课：深度学习框架与应用

Caffe

TensorFlow

PyTorch