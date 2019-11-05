---
layout: post
title: 线段树
date: 2019-11-05
categories: Programming
tags: C++
status: publish
type: post
published: true
author: Quebradawill
---

### 1. 定义

线段树（segment tree），顾名思义，是用来存放给定区间（segment or interval）内对应信息的一种数据结构。与[树状数组（binary indexed tree）](https://www.jianshu.com/p/5b209c029acd)相似，线段树也用来处理数组相应的区间查询（range query）和元素更新（update）操作。与树状数组不同的是，线段树不但适用于区间求和查询，也可以进行区间最大值、区间最小值（Range Minimum / Maximum Query problem）或区间异或值的查询。

对应于树状数组，线段树进行更新（update）的操作为 $O(\log n)$，进行区间查询（range query）的操作也为 $O(\log n)$。

参考：[线段树（segment tree），看这一篇就够了](https://www.jianshu.com/p/6fd130084a43)

