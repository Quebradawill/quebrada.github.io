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

并查集是一种用来**管理元素分组情况**的数据结构，并查集可以高效地进行如下操作：<br><ol><li>查询元素 $a$ 和元素 $b$ 是否属于同一组；</li><br><li>合并元素 $a$ 和元素 $b$ 所在的组。</li></ol>



参考：<br>1. [https://www.jianshu.com/p/b37ba1f7d45c](https://www.jianshu.com/p/b37ba1f7d45c)<br>2. [https://www.jianshu.com/p/fc17847b0a31](https://www.jianshu.com/p/fc17847b0a31)<br>3. [https://www.cnblogs.com/cyjb/p/UnionFindSets.html](https://www.cnblogs.com/cyjb/p/UnionFindSets.html)