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

### 1. 并查集（Union-Find Sets）

并查集（Union-Find Sets）是一种非常精巧而实用的数据结构，它主要用于处理一些**不相交集合**的合并问题。一些常见的用途有求<font color='blue'>连通子图</font>、求最小生成树的 <font color='blue'>Kruskal 算法</font>和求<font color='blue'>最近公共祖先（Least Common Ancestors，LCA）</font>等。 

#### 1.1 并查集是什么

并查集是一种用来**管理元素分组情况**的数据结构，并查集可以高效地进行如下操作：<br>- 查询元素 $a$ 和元素 $b$ 是否属于同一组；<br>- 合并元素 $a$ 和元素 $b$ 所在的组。

#### 1.2 并查集的结构

并查集使用**树形结构**实现。我们可以使用树这种数据结构来表示集合，不同的树就是不同的集合，并查集中包含了多棵树，表示并查集中不同的子集，树的集合是**森林**，所以并查集属于森林。

#### 1.3 并查集支持的操作

1、建立并查集： 建立一个新的并查集，其中包含 $s$ 个单元素集合。 

2、合并： 把元素 $x$ 和元素 $y$ 所在集合合并，要求 $x$ 和 $y$ 所在集合不相交，如果相交则不合并。 

3、查询：只需返回该元素所在树的根结点。若要判断两个元素是否在一个集合，只需要通过 `Find(x)` 和 `Find(y)` 返回各自的根结点，比较是否相等即可。已知树中的一个结点，找到其根结点的时间复杂度为 $O(D)$，$D$ 为结点的深度。

**1.3.1 查询**

树的节点表示集合中的元素，指针表示指向父节点的指针，根节点的指针指向自己，表示其没有父节点。沿着每个节点的父节点不断向上查找，最终就可以找到该树的根节点，即该集合的代表元素。 

参考：<br>1. [https://www.jianshu.com/p/b37ba1f7d45c](https://www.jianshu.com/p/b37ba1f7d45c)<br>2. [https://www.jianshu.com/p/fc17847b0a31](https://www.jianshu.com/p/fc17847b0a31)<br>3. [https://www.cnblogs.com/cyjb/p/UnionFindSets.html](https://www.cnblogs.com/cyjb/p/UnionFindSets.html)