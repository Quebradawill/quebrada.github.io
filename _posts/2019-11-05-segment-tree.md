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

对应于树状数组，线段树进行更新（update）操作的时间复杂度为 $O(\log n)$，进行区间查询（range query）操作的时间复杂度也为 $O(\log n)$。

### 2. 实现原理

从数据结构的角度来说，线段树是用一个**完全二叉树**来存储对应于其每个区间（segment）的数据。该二叉树的每个结点保存着相对应于这个区间的信息。同时，线段树所使用的这个二叉树用一个数组保存，与堆（Heap）的实现方式相同。

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

由于所构建的是完全二叉树，完全二叉树度为 1 的结点最多有 1 个，由于总是有 $n_0 = n_2 + 1$，分情况讨论：<br>1. 若完全二叉树没有度为 1 的结点，则 $n = n_0 + n_2$，此时完全二叉树的结点总数为 $ 2 n_0 - 1$；<br>2. 若完全二叉树有一个度为 1 的结点，则 $ n = n_0 + n_2 + 1$，此时完全二叉树的结点总数为 $ 2 n_0$。

因此，若叶子结点的总数为 $n$，则线段树的结点数量为 $2n$ 或者 $2n-1$。

### 4. 更新

更新一个线段树的过程与上述构造线段树的过程相同。当输入数组中位于 $i$ 位置的元素被更新时，我们只需从这一元素对应的叶子结点开始，沿二叉树的路径向上更新至根结点即可。显然，这一过程是一个 $O( \log n)$ 的操作。其算法如下：

```c++
update(i, value):
    i = i + n;
    segmentTree[i] = value;
    while i > 1:
        i = i / 2;
        segmentTree[i] = merge(segmentTree[2*i], segmentTree[2*i+1]);
```

### 5. 区间查询

区间查询大体上可以分为 3 种情况讨论：<br>1. 当前结点所代表的区间完全位于给定需要被查询的区间之外，则不应考虑当前结点；<br>2. 当前结点所代表的区间完全位于给定需要被查询的区间之内，则可以直接查看当前结点的母结点；<br>3. 当前结点所代表的区间部分位于需要被查询的区间之内，部分位于其外，则我们先考虑位于区间外的部分，后考虑区间内的（注意总有可能找到完全位于区间内的结点，因为叶子结点的区间长度为 1，因此我们总能组合出合适的区间）。

以求最小值为例，其算法如下：

```C++
minimum(left, right):
    left = left + n;
    right = right + n;
    minimum = Integer.MAX_VALUE;
    while left < right:
        if left is odd:
            // left is out of range of parent interval, check value of left node first,
            // then shift it right in the same level
            minimum = min(minimum, segmentTree[left]);
            left = left + 1;
        if right is odd:
            // right is out of range of current interval,
            // shift it left in the same level and then check the value
            right = right - 1;
            minimum = min(minimum, segmentTree[right]);
        // move left and right one level up
        left = left / 2;
        right = right / 2;
```

### 6. $n$ 不是 2 的次方怎么办？

一个简单的方法是在原数组的结尾补 0，直到其长度正好为 2 的次方位置。但事实上这个方法比较低效。最坏情况下，我们需要 $O(4n)$ 的空间来存储相应的线段树。例如，如果输入数组的长度刚好为 $2^i +1$，则我们首先需要补 0 直到数组长度为 $2^{i+1} = 2 \times 2^i$ 为止。那么对于这个补 0 过后的数组，我们需要的线段树数组的长度为 $2 \times 2 \times 2^x = 4 \times 2^i = O(4n)$。

其实上面所说的算法对于 $n$ 不是 2 的次方的情况同样适用。这也是为什么我在上文中说线段树是一棵**完全二叉树**而非**满二叉树**的原因。

### 7. Java代码

**Range Minimum Query**

```Java
public class MinSegmentTree {
    private ArrayList<Integer> minSegmentTree;
    private int n;
    
    public MinSegmentTree(int[] arr) {
        n = arr.length;
        minSegmentTree = new ArrayList<>(2 * n);
        
        for  (int i = 0; i < n; i++) {
            minSegmentTree.add(0);
        }
        
        for (int i = 0; i < n; i++) {
            minSegmentTree.add(arr[i]);
        }
        
        for (int i = n - 1; i > 0; i--) {
            minSegmentTree.set(i, Math.min(minSegmentTree.get(2 * i), minSegmentTree.get(2 * i + 1)));
        }
    }
    
    public void update(int i, int value) {
        i += n;
        minSegmentTree.set(i,  value);
        
        while (i > 1) {
            i /= 2;
            minSegmentTree.set(i, Math.min(minSegmentTree.get(2 * i), minSegmentTree.get(2 * i + 1)));
        }
    }
    
    // Get the minimum of range [left, right)
    public int minimum(int left, int right) {
        left += n;
        right += n;
        int min = Integer.MAX_VALUE;
        
        while (left < right) {
            if ((left & 1) == 1) {
                min = Math.min(min, minSegmentTree.get(left));
                left++;
            }
            
            if ((right & 1) == 1) {
                right--;
                min = Math.min(min,  minSegmentTree.get(right));
            }
            left >>= 1;
            right >>= 1;
        }
        
        return min;
    }
}
```

**Range Sum Query**

```java
public class SumSegmentTree {
    private ArrayList<Integer> sumSegmentTree;
    private int n;
    
    public SumSegmentTree(int[] arr) {
        n = arr.length;
        sumSegmentTree = new ArrayList<>(2 * n);
        
        for  (int i = 0; i < n; i++) {
            sumSegmentTree.add(0);
        }
        
        for (int i = 0; i < n; i++) {
            sumSegmentTree.add(arr[i]);
        }
        
        for (int i = n - 1; i > 0; i--) {
            sumSegmentTree.set(i, sumSegmentTree.get(2 * i) + sumSegmentTree.get(2 * i + 1));
        }
    }
    
    public void update(int i, int value) {
        i += n;
        sumSegmentTree.set(i,  value);
        
        while (i > 1) {
            i /= 2;
            sumSegmentTree.set(i, sumSegmentTree.get(2 * i) + sumSegmentTree.get(2 * i + 1));
        }
    }
    
    // Get the sum of range [left, right)
    public int sum(int left, int right) {
        left += n;
        right += n;
        int sum = 0;
        while (left < right) {
            if ((left & 1) == 1) {
                sum += sumSegmentTree.get(left);
                left++;
            }
            
            if ((right & 1) == 1) {
                right--;
                sum += sumSegmentTree.get(right);
            }
            
            left >>= 1;
            right >>= 1;
        }
        
        return sum;
    }
}
```

参考：[线段树（segment tree），看这一篇就够了](https://www.jianshu.com/p/6fd130084a43)
