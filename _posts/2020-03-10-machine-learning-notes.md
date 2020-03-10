---
layout: post
title: 机器学习笔记
date: 2020-03-10
categories: Research
tags: machine-learning
status: publish
type: post
published: true
author: Quebradawill
---

## 1. 王的机器——小孩都看得懂

### 1.1 [小孩都可以玩的神经网络](https://mp.weixin.qq.com/s?__biz=MzIzMjY0MjE1MA==&mid=2247487666&idx=1&sn=48e85270ba3d6c396aa63ee3fd15986d&chksm=e89093bbdfe71aad8352f68cd4fa56fc81a1c3dfcf7dddf38ed485b17ef77c78abea3769ebad&scene=21#wechat_redirect)

一个非常好的网站：[https://teachablemachine.withgoogle.com/#](https://teachablemachine.withgoogle.com/#)

这个 Teachable Machine 网站非常酷，该神经网络完成的图像分类是由 Tensorflow.js 实现的，从 github 看源码背后的网络架构是 SqueezeNet。它属于卷积神经网络中轻量级的网络，在参数只有 AlexNet 1/50 的时候和其表现相当。

### 1.2 [小孩都看得懂的推荐系统](https://mp.weixin.qq.com/s?__biz=MzIzMjY0MjE1MA==&mid=2247487991&idx=1&sn=3ce1bc46dec7edd92fa1a7a45b82c7ae&chksm=e89092fedfe71be85a28bc9a887199af6a899c4309bf6edd56de9d66fd527a41c1f045b629b1&scene=21#wechat_redirect)

### 1.3 [小孩都看得懂的逐步提升](https://mp.weixin.qq.com/s?__biz=MzIzMjY0MjE1MA==&mid=2247488151&idx=1&sn=ec98916b6dedd5df6360e1fc1838984c&chksm=e890919edfe7188850ae0ec1b5cdd5611551dfd7b78b8a3afc0a2e4b8101a7f2c889576c4af6&scene=126&sessionid=1583762096&key=bca082e537533061aedfe3b237c612944f8c73c2a359b26166c88e84906d97911344aa4ea0f2ac5e1b49d88469b0e76137fa92d1f89064d81d31c00287ba9c5fe8eba72c7ba7a57bfbaf38d1947d1157&ascene=1&uin=MjQwMDA2NDE0Mw%3D%3D&devicetype=Windows+10&version=62080079&lang=en&exportkey=AY6sZb0CY6ndd6c20vZ7sNM%3D&pass_ticket=sF2QUN0lpuQP3Ov7rnxpUj5mY1AvrDoOEcJXeRgDwyNGd4NkDR9NMQotsQeJCRVl)

逐步提升法（adaboost）里把「分类错误的数据权重增大，把分类正确的数据权重减小」。

### 1.4 [小孩都看得懂的聚类](https://mp.weixin.qq.com/s?__biz=MzIzMjY0MjE1MA==&mid=2247488214&idx=1&sn=4875b8ab2dc72a4d89b0586f558fe659&chksm=e89091dfdfe718c9d6ed90631d99ed896e9356fb2cf07cb1b4f53215a327ab6f60be6a78a93e&scene=126&sessionid=1583762096&key=6e52129ce3f67088eb69a65c37adf21c032247871de29edb2bcd2cfe85f121deddec21a2ec46adff34b694c3e4475449e55dd3193bc6cc78eced5ee3f2505904bdb29f2bc27036d4df713d666188229e&ascene=1&uin=MjQwMDA2NDE0Mw%3D%3D&devicetype=Windows+10&version=62080079&lang=en&exportkey=AfVh86PoUwHP7fkvtBg3%2BNs%3D&pass_ticket=sF2QUN0lpuQP3Ov7rnxpUj5mY1AvrDoOEcJXeRgDwyNGd4NkDR9NMQotsQeJCRVl)

**K 均值聚类**和**层级聚类**都非常简单，而确定它们聚类停止条件的，分别是**肘部法则**和**树形图**，都也很直观。不管数据有多少维，**肘部法则**和**树形图**永远是二维图。

### 1.5 [小孩都看得懂的主成分分析](https://mp.weixin.qq.com/s?__biz=MzIzMjY0MjE1MA==&mid=2247488234&idx=1&sn=b99d1248dde7b62ac25716c0aca2e57d&chksm=e89091e3dfe718f5dba0ba21d39aba52d188ab18dfa54c557e710fb5ed64e72aa83d1629d151&scene=126&sessionid=1583762096&key=bca082e5375330612c38cde48c7b74768075f4f6b1648a0f336efecbc227d7f92a69ce9c2d27edc4a41f857de78834110908418564489092ac366cdea9b760f32a6b5a6f9366ad25296fdf7ff6a2ca02&ascene=1&uin=MjQwMDA2NDE0Mw%3D%3D&devicetype=Windows+10&version=62080079&lang=en&exportkey=AczGGxM%2F3nqcZb%2FMeV6vAhE%3D&pass_ticket=sF2QUN0lpuQP3Ov7rnxpUj5mY1AvrDoOEcJXeRgDwyNGd4NkDR9NMQotsQeJCRVl)

PCA 是无监督学习中的最常见的数据降维方法，但实际问题特征很多的情况，PCA 通常会预处理来减少特征个数。

总结一下 PCA 的完整操作：<br>1、一开始我们有 5 维特征，分别是**房间面积**、**房间个数**、**卧室个数**、**周边好学校个数**和**周边犯罪率**；<br>2、这 5 维特征可以体现在一个 5D 图中，虽然我们无法精准地把它们画出来；<br>3、计算协方差矩阵，5 维特征得到 $5 \times 5$ 的对称矩阵；<br>4、求出**特征向量**和**特征值**，将特征值从大到小排序，去除**明显比较小**（这个需要点主观判断）的，假设去除了后三个，保留了前两个；<br>5、这 2 维特征可以体现在一个 2D 图中，我们人类终于可以可视化它了；<br>6、当然原来「5 维特征的数据表」缩减成了「2 维特征的数据表」，希望这 2 维体现的是抽象的**尺寸特征**和**环境特征**，就像开头那张图一样。

### 1.6 [小孩都看得懂的循环神经网络](https://mp.weixin.qq.com/s?__biz=MzIzMjY0MjE1MA==&mid=2247488290&idx=1&sn=427aee1a0afaf4c4f34b6d451de8bd3d&chksm=e890902bdfe7193d612a6c6516ad4c5ebae2936bf9428fa46edcfa631271fbe9a6c2be43d943&scene=126&sessionid=1583762096&key=1eff3ffcd7f0512886c5e33abd93a1cb373e712fdb3e9b5b5d20de091db9d37d32066696b3e8325a367f28c89818b74c345bb7faee970b42ab729cb1724fc7bf995fc8aa02a9d0520946678172f44a86&ascene=1&uin=MjQwMDA2NDE0Mw%3D%3D&devicetype=Windows+10&version=62080079&lang=en&exportkey=AShGIMkFl3V9SHuREPI%2F5A4%3D&pass_ticket=sF2QUN0lpuQP3Ov7rnxpUj5mY1AvrDoOEcJXeRgDwyNGd4NkDR9NMQotsQeJCRVl)