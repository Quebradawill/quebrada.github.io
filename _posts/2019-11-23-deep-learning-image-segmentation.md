---
layout: post
title: 深度学习应用于医学图像分割
date: 2019-11-23
categories: Image-processing
tags: DeepLearning
status: publish
type: post
published: true
author: Quebradawill
---

对于只有一个标签的（只区分类别）的任务，我们称之为**语义分割**（semantic segmentation）；对于区分相同类别的不同个体的，则称之为**实例分割**（instance segmentation）。由于实例分割往往只能分辨可数目标，因此，为了同时实现实例分割与不可数类别的语义分割，2018 年 Alexander Kirillov 等人提出了**全景分割**（panoptic segmentation）的概念。 

<font color='blue'>对于语义分割，应该是相同类别只给一个标签，比如图像中有很多人，所有的人只给一个标签。而在实例分割中不同的人要给不同的标签。</font>

尽管 FCN 意义重大，在当时来讲效果也相当惊人，但是 FCN 本身仍然有许多局限。比如：<br>- 没有考虑全局信息<br>- 无法解决实例分割问题<br>- 速度远不能达到实时<br>- 不能够应对诸如3D点云等不定型数据

**参考：**<br>1. [从FCN说起](https://zhuanlan.zhihu.com/p/62839949)