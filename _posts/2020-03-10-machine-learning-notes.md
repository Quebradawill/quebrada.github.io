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