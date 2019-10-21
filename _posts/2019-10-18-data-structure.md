---
layout: post
title: 数据结构与算法相关问题
date: 2019-10-18
categories: Programming
tags: C++
status: publish
type: post
published: true
author: Quebradawill
---

### 1. 栈的输出序列数

一个栈的输入序列是 $$1,2,3,4,5$$，不可能的输出序列有哪些？算一下 $$1,2,3,4,5$$ 的全排列有多少种，合法的出栈序列的个数是 Catalan 数，两者相减就是不可能的序列的个数。

Catalan 数（卡特兰数）的定义令 $$h(1)=1$$，Catalan 数满足递归式：$$h(n) = h(1) h(n-1) + h(2) h(n-2) + \cdots + h(n-1) h(1), n \geq 2$$，该递推关系的解为：$$h(n) = C(2n-2,n-1)/n，n=1,2,3,...$$（其中 $$C(2n-2,n-1)$$ 表示 $$2n-2$$ 个中取 $$n-1$$ 个的组合数）。

