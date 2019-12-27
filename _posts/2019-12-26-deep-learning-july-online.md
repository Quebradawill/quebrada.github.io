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
\begin{aligned}
\\ \det (P^{-1} A P) & = \det(P^{-1}) \det(A) \det(P) \\ & =  \det(P^{-1}) \det(P) \det(A) \\ & = \det(A) \end{aligned}
$$


- 迹（trace）$ {\rm tr} (AB) = {\rm tr} (BA)$，${\rm tr} (P^{-1} A P) = {\rm tr} (AP P^{-1}) = {\rm tr} (A \cdot I) = {\rm tr} (A)$
- 秩（rank）
- 特征值：特征方程 $ \det (A - \lambda I)  = 0$ 的根。如果 $ \det (A - \lambda I) = 0 $，那么 $\det \left ( P^{-1} (A - \lambda I) P \right) = 0$，于是 $  \det (P^{-1} A P - \lambda I)  = 0 $。

<font color = 'blue'>特征值是最重要的相似不变量，利用这个相似不变量可以方便地得出上面所有的不变量。</font>

**相合变换（二次型）**：假设 $V$ 是一个实系数线性空间，那么线性空间上的**度量**指的是空间中向量的内积关系 $G(v_1,v_2)$。如果 $\alpha \{\alpha_1, \cdots, \alpha_k \}$ 是空间 $V$ 的一组基，那么这个内积一般可以用一个对称矩阵 $H_{\alpha} = [h_{ij}]_{n \times n}$ 来表示。

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
  - 避免在类似样本上计算梯度造成的冗余计算；
  - 增加了跳出当前的局部最小值的潜力；
  - 在中逐渐缩小学习率的情况下，有与批梯度下降法类似的收敛速度。
- 小批量随机梯度下降法（Mini Batch SGD），每次梯度计算使用一个小批量样本
  - 梯度计算比单样本更加稳定；
  - 可以很好地利用现成的高度优化的矩阵运算工具。

**注意：**神经网络训练的文献中经常把 Mini Batch SGD 称为 SGD。

随机梯度下降法的**主要困难**在于前述的第二个问题，即学习率的选取。

- 局部梯度的反方向不一定是函数整体下降的方向：对图像比较崎岖的函数，尤其是隧道型曲面，梯度下降表现不佳；
- 预定学习率衰减法的问题：学习率衰减法很难根据当前数据进行自适应；
- 对不同参数采取不同的学习率的问题：在数据有一定稀疏性时，希望对不同特征采取不同的学习率；
- 神经网络训练中梯度下降法容易被困在鞍点附近的问题：比起局部极小值，鞍点更加可怕。

为什么不用牛顿法？

- 牛顿法要求计算目标函数的二阶导数（Hessian matrix），在高维特征情形下这个<font color='blue'>矩阵非常巨大</font>，计算和存储都成问题；
- 在使用小批量情形下，牛顿法对于二阶导数的估计<font color='blue'>噪声太大</font>；
- 在目标函数非凸时，牛顿法更容易受到<font color='blue'>鞍点</font>甚至<font color='blue'>最大值点</font>的吸引。

### 2.3 随机梯度下降法的优化算法

**1. 动量法（Momentum）**（适用于隧道型曲面）

动量法每次更新都吸收一部分上次更新的余势：


$$
\begin{aligned} \\ v_t &= \color{red}{ \gamma v_{t-1} } + \eta \nabla_{\theta} J(\theta) \\ \theta_t &= \theta_{t-1} - v_t \\ &= \theta_{t-1} \color{red}{ - \gamma v_{t-1} } - \eta \nabla_{\theta} J(\theta) \end{aligned}
$$


**2. 动量法的改进算法（Nesterov accelerated gradient）**

动量法的一个问题：从山顶推下的铁球越滚越快，以至于到了山底停不下来。希望算法更聪明一些，到达山底前景自己刹车。

利用主体下降方向的先见之明，预判下一步的位置，并到预判位置计算梯度。


$$
\begin{aligned} \\ v_t &= \color{red}{\gamma v_{t-1}} + \eta \nabla_{\theta} J(\theta \color{magenta}{- \gamma v_{t-1}}) \\ \theta_t &= \theta_{t-1} - v_t \\ &= \theta_{t-1} \color{red}{- \gamma v_{t-1}} - \eta \nabla_{\theta} J(\theta \color{magenta}{- \gamma v_{t-1}}) \end{aligned}
$$


**3. Adagrad**（自动调整学习率，适用于稀疏数据）

梯度下降法在每一步对每一个参数使用相同的学习率，这种一刀切的做法不能有效利用每一个数据集自身的特点。

Adagrad 是一种自动调整学习率的方法

- 随着模型的训练，学习率自动衰减；
- 对于更新频繁的参数，采取较小的学习率；
- 对于更新不频繁的参数，采取较大的学习率。

对每个参数历史上的每次更新叠加，以此来做下一次更新的惩罚系数：

- 梯度：$g_{t,i} = \nabla_{\theta} J(\theta_i)$
- 梯度历史矩阵：$G_t$ 对角矩阵，其中 $G_{t,ii} = \sum_k g_{k,i}^2$
- 参数更新：


$$
\theta_{t+1,i} = \theta_{t,i} - \frac{\eta}{\sqrt{G_{t,ii} + \epsilon}} \cdot g_{t,i}
$$


**4. Adadelta**（Adagrad 的改进算法）

Adagrad 的一个问题在于随着训练的进行，学习率快速单调衰减。Adadelta 则使用梯度平方的移动平均来取代全部历史平方和。定义移动平均：$$E[g^2]_t = \gamma E[g^2]_{t-1} + (1 - \gamma) g_t^2$$，于是就得到参数更新法则：


$$
\theta_{t+1,i} = \theta_{t,i} - \frac{\eta}{\sqrt{E[g^2]_{t,ii} + \epsilon}} \cdot g_{t,i}
$$


Adagrad 以及一般梯度下降法的另一个问题在于，梯度与参数的单位不匹配。Adadelta 使用参数更新的移动平均取代学习率 $\eta$，于是参数更新法则：


$$
\theta_{t+1,i} = \theta_{t,i} - \frac{\sqrt{E[\Delta \theta]_{t-1}}}{\sqrt{E[g^2]_{t,ii} + \epsilon}} \cdot g_{t,i}
$$


**注意：**Adadelta 的第一个版本也叫做 RMSprop，是 Geoff Hinton 独立于 Adadelta 提出来的。

**5. Adam**（结合了动量法与 Adadelta 的算法）

如果把 Adadelta 里梯度的平方和看成是梯度的二阶矩，那么梯度自身的求和就是一阶矩。Adam 算法在 Adadelta 二阶矩基础上又引入了一阶矩。

而一阶矩，其实就类似于动量法里面的动量。


$$
\begin{aligned} m_t &= \beta_1 m_{t-1} + (1 - \beta_1) g_t \\ v_t &= \beta_2 v_{t-1} + (1 - \beta_2) g_t^2 \end{aligned}
$$


于是参数更新法则：


$$
\theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{v_t} + \epsilon} m_t
$$


**注意：**实际操作中 $v_t$ 与 $m_t$ 采取了更好的无偏估计，避免前几次更新时数据不足的问题。

**6. 如何选择算法**

- 动量法与 Nesterov 的改进方法着重解决目标函数图像崎岖的问题；
- Adagrad 与 Adadelta 主要解决学习率更新的问题；
- Adam 集中了两种做法的主要优点。

## 3. 概率论

马尔可夫（Markov）：$P(X_1, \cdots, X_n) = P(X_1) P(X_2 \vert X_1) P(X_3 \vert X_2) \cdots P(X_n \vert X_{n - 1})$

生成模型和判别模型：

- 生成模型：$P(Y \vert X) = P(X \vert Y) \times P(Y) / P(X) $
  - 朴素贝叶斯（Naïve Bayes）
  - 隐马尔可夫（Hidden Markov Model）
- 判别模型：$P(Y \vert X)$
  - 逻辑回归（Logistic Regression）
  - 支持向量机（Support Vector Machine）
  - 条件随机场（Conditional Random Field）

**ROC 曲线：**

- 评估模型时，如果数据不平衡，则用准确率是不合适的。

- $\rm{precision} = TP / (TP + FP)$

  
  $$
  \begin{aligned} \rm{recall} = & TPR = TP / (TP + FN) \\ & FPR = FP / (FP + TN) \end{aligned}
  $$
  

- ROC 曲线的横坐标为 FPR（False Positive Rate），纵坐标为 TPR（True Positive Rate）。

**熵（Entropy）：**$H(X) = - \sum_i P(X_i) \log P(X_i)$

**KL 散度（KL Divergence）：**


$$
\begin{aligned} \\ \textrm{KL} (p \Vert q) & = \sum_i p_i \log \frac{p_i}{q_i} \\ & = \sum_i p_i \log p_i - \sum_i p_i \log q_i \\ & = - H(p) + H(p,q) \end{aligned}
$$


$\textrm{KL} (p \Vert q) \geq 0$，$\textrm{KL} (p \Vert q) = 0 \Longleftrightarrow p = q$

KL 散度不对称！常用于解释 EM 算法。

**互信息（Mutual Information）：**


$$
\begin{align} I(X,Y) & = \textrm{KL} \left( P(X,Y) \Vert P(X) P(Y) \right) = \sum_x \sum_y P(X,Y) \log \frac{P(X,Y)}{P(X) P(Y)} \\ I(X,Y) & \geq 0, \qquad I(X,Y) = 0 \Longleftrightarrow P(X,Y) = P(X)P(Y) \\
I(X,Y) & = H(X) - H(X \vert Y) \end{align}
$$
