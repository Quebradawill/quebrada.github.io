---
layout: post
title: 并查集
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

树的结点表示集合中的元素，指针表示指向父结点的指针，根结点的指针指向自己，表示其没有父结点。沿着每个结点的父结点不断向上查找，最终就可以找到该树的根结点，即该集合的代表元素。 

假设使用一个足够长的数组来存储树结点。

```C++
const int MAXSIZE = 500;
int uset[MAXSIZE];

void makeSet(int size) {
    for (int i = 0; i < size; i++)
        uset[i] = i;
}
```

如果每次都沿着父结点向上查找，那时间复杂度就是树的高度，完全不可能达到常数级。这里需要应用一种非常简单而有效的策略——**路径压缩**。

`find` 的两个版本，分别是递归版和非递归版。

```C++
int find(int x) {
    if (x != uset[x]) uset[x] = find(uset[x]);
    return uset[x];
}

int find(int x) {
    int p = x, t;
    while (uset[p] != p) p = uset[p];
    while (x != p) { t = uset[x]; uset[x] = p; x = t; }
    return x;
}
```

**1.3.2 合并**

合并操作 `unionSet` 也非常简单，就是将一个集合的根指向另一个集合的根。

这里应用一个简单的启发式策略——**按秩合并**。该方法使用秩来表示树高度的上界，在合并时，总是将具有较小秩的树根指向具有较大秩的树根。简单的说，就是总是将比较矮的树作为子树，添加到较高的树中。为了保存秩，需要额外使用一个与 `uset` 同长度的数组，并将所有元素都初始化为 0。

```C++
void unionSet(int x, int y) {
    if ((x = find(x)) == (y = find(y))) return;
    if (rank[x] > rank[y]) uset[y] = x;
    else {
        uset[x] = y;
        if (rank[x] == rank[y]) rank[y]++;
    }
}
```

下面是按秩合并的并查集的完整代码，这里只包含了递归的 `find` 操作。 

```C++
const int MAXSIZE = 500;
int uset[MAXSIZE];
int rank[MAXSIZE];
 
void makeSet(int size) {
    for(int i = 0;i < size;i++)  uset[i] = i;
    for(int i = 0;i < size;i++)  rank[i] = 0;
}
int find(int x) {
    if (x != uset[x]) uset[x] = find(uset[x]);
    return uset[x];
}
void unionSet(int x, int y) {
    if ((x = find(x)) == (y = find(y))) return;
    if (rank[x] > rank[y]) uset[y] = x;
    else {
        uset[x] = y;
        if (rank[x] == rank[y]) rank[y]++;
    }
}
```

除了按秩合并，并查集还有一种常见的策略，就是按**集合中包含的元素个数**（或者说树中的结点数）合并，将包含结点较少的树根，指向包含结点较多的树根。这个策略与按秩合并的策略类似，同样可以提升并查集的运行速度，而且省去了额外的 `rank` 数组。

这样的并查集具有一个略微不同的定义，即若 `uset` 的值是正数，则表示该元素的父结点（的索引）；若是负数，则表示该元素是所在集合的代表（即树根），而且值的相反数即为集合中的元素个数。相应的代码如下所示，同样包含递归和非递归的 `find` 操作： 

 ```C++
const int MAXSIZE = 500;
int uset[MAXSIZE];
 
void makeSet(int size) {
    for(int i = 0;i < size;i++) uset[i] = -1;
}

int find(int x) {
    if (uset[x] < 0) return x;
    uset[x] = find(uset[x]);
    return uset[x];
}

int find(int x) {
    int p = x, t;
    while (uset[p] >= 0) p = uset[p];
    while (x != p) {
        t = uset[x];
        uset[x] = p;
        x = t;
    }
    return x;
}

void unionSet(int x, int y) {
    if ((x = find(x)) == (y = find(y))) return;
    if (uset[x] < uset[y]) {
        uset[x] += uset[y];
        uset[y] = x;
    }
    else {
        uset[y] += uset[x];
        uset[x] = y;
    }
}
 ```

如果要获取某个元素 $x$ 所在集合包含的元素个数，可以使用 `-uset[find(x)]` 得到。

**参考：**<br>1. [https://www.jianshu.com/p/b37ba1f7d45c](https://www.jianshu.com/p/b37ba1f7d45c)<br>2. [https://www.jianshu.com/p/fc17847b0a31](https://www.jianshu.com/p/fc17847b0a31)<br>3. [https://www.cnblogs.com/cyjb/p/UnionFindSets.html](https://www.cnblogs.com/cyjb/p/UnionFindSets.html)