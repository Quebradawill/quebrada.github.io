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

### 2. 实现原理

从数据结构的角度来说，线段树是用一个**完全二叉树**来存储对应于其每一个区间（segment）的数据。该二叉树的每个结点中保存着相对应于这个区间的信息。同时，线段树所使用的这个二叉树是用一个数组保存的，与堆（Heap）的实现方式相同。

例如，给定一个长度为 $N$ 的数组 $\textrm{arr}$，其所对应的线段树 $T$ 各个结点的含义如下：<br>1. $T$ 的根结点代表整个数组所在区间对应的信息，即 $\textrm{arr}[0:N]$（**不含 $N$**）所对应的信息；<br>2. $T$ 的每个叶结点存储对应于输入数组的每个单个元素构成的区间 $\textrm{arr}[i]$ 所对应的信息，此处 $0 \leq i < N$；<br>3. $T$ 的每个中间结点存储对应于输入数组某一区间 $\textrm{arr}[i:j]$ 对应的信息，此处 $0 \leq i < j < N$。

线段树的高度为 $\log N$。

对于一个线段树来说，其应该支持的两种操作为：<br>1. `Update`：更新输入数组中的某一个元素并对线段树做相应的改变；<br>2. `Query`：用来查询某一区间对应的信息（如最大值，最小值，区间和等）。

### 3. 线段树的初始化

线段树的初始化是自底向上进行的。从每个叶子结点开始（也就是原数组中的每个元素），沿从叶子结点到根结点的路径向上按层构建。在构建的每一步中，对应两个子结点的数据将被用来构建应当存储于它们母结点中的值。每个中间结点代表它的左右两个子结点对应区间融合过后的大区间所对应的值。这个融合信息的过程可能依所需要处理的问题不同而不同（例如对于保存区间最小值的线段树来说，merge 的过程应为 `min()` 函数，用以取得两个子区间中的最小区间最小值作为当前融合过后的区间的区间最小值）。但从叶子节点（长度为 1 的区间）到根结点（代表输入的整个区间）更新的这一过程是统一的。

**注意此处我们对于 `segmentTree` 数组的索引从 1 开始算起**。则对于数组中的任意结点 $i$，其左子结点为 $ 2 \times i$，右子结点为 $2 \times i + 1 $，其母结点为 $i/2$。

构建线段树的算法描述如下：

```C++
construct(arr):
    n = length(arr);
    segmentTree = new int[2*n];
    for i from n to 2*n-1:
        segmentTree[i] = arr[i - n];
    for i from n - 1 to 1:
        segmentTree[i] = merge(segmentTree[2*i], segmentTree[2*i+1]);
```

由于所构建的是完全二叉树，完全二叉树度为 1 的结点最多有 1 个，分情况讨论：<br>1. 若完全二叉树没有度为 1 的结点，则 $n = n_0 + n_2, \ n_0 = n_2 + 1$，此时完全二叉树的结点总数为 $ 2 n_0 - 1$；<br>2. 若完全二叉树有一个度为 1 的结点，则 $ n = n_0 + n_2 + 1,  \ n_0 = n_2 + 1$，此时完全二叉树的结点总数为 $ 2 n_0$。

因此，若叶子结点的总数为 $n$，则线段树的结点数量为 $2n$ 或者 $2n-1$。



参考：[线段树（segment tree），看这一篇就够了](https://www.jianshu.com/p/6fd130084a43)
