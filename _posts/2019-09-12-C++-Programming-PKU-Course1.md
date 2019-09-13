---
layout: post
title: 程序设计与算法（一）C 语言程序设计-北京大学
date: 2019-09-12
categories: Programming
tags: C++
status: publish
type: post
published: true
author: Quebradawill
---

### 第 12 周 STL 初步

#### 1. 算法

要使用其中的算法，需要 `#include <algorithm>`。

##### 1.1 排序算法 `sort`

- 对基本类型的数组从小到大排序：`sort(arrayName + n1, arrayName + n2);`，将数组中下标范围为 `[n1, n2)` 的元素从小到大排序；

- 对元素类型为 `T` 的基本类型数组从大到小排序：`sort(arrayName + n1, arrayName + n2, greater<T>())`；

- 用自定义的排序规则，对任何类型 `T` 的数组排序 `sort(arrayName + n1, arrayName + n2, sort_rule_struct())`，排序规则结构的定义方式：

  ```C++
  struct struct_name
  {
      bool operator()(const T &a1, const T &a2){
          // 若 a1 应该在 a2 前面，则返回 true，否则返回 false。
      }
  }
  ```

##### 1.2 二分查找算法

二分查找算法有：`binary_serach`、`lower_search`、`upper_search`。

- 在从小到大排好序的基本类型数组上进行查找 `binary_search(arrayName + n1, arrayName + n2, value)`，查找区间为 `[n1, n2)` 的元素；
- 在用自定义排序规则排好序的、元素为任意 `T` 类型的数组中进行二分查找，`binary_search(arrayName + n1, arrayName + n2, value, sort_rule_struct())`，**查找时的排序规则，必须和排序时的规则一致！**
- 在对元素类型为 `T` 的从小到大排好序的基本类型的数组中进行查找，`T* lower_bound(arrayName + n1, arrayName + n2, value)`，返回一个指针 `T* p`；`*p` 是查找区间里下标最小的， **大于等于**“值” 的元素。如果找不到， `p` 指向下标为 `n2` 的元素。
- 在元素为任意的 `T` 类型、按照自定义排序规则排好序的数组中进行查找，`T* lower_bound(arrayName + n1, arrayName + n2, value, sort_rule_struct())`，返回一个指针 `T* p`；`*p` 是查找区间里下标最小的，按自定义排序规则，**可以排在“值”后面**的元素。如果找不到，`p` 指向下标为 `n2` 的元素。
- 在对元素类型为 `T` 的从小到大排好序的基本类型的数组中进行查找，`T* upper_bound(arrayName + n1, arrayName + n2, value)`，返回一个指针 `T* p`；`*p` 是查找区间里下标最小的， **大于**“值” 的元素。如果找不到， `p` 指向下标为 `n2` 的元素。
- 在元素为任意的 `T` 类型、按照自定义排序规则排好序的数组中进行查找，`T* upper_bound(arrayName + n1, arrayName + n2, value, sort_rule_struct())`，返回一个指针 `T* p`；`*p` 是查找区间里下标最小的，按自定义排序规则，**必须排在“值”后面**的元素。如果找不到，`p` 指向下标为 `n2` 的元素。

#### 2. STL 中的平衡二叉树

可以使用“平衡二叉树”存放数据，增加、删除、查找数据都能在 $$\log (n) $$ 复杂度完成，体现在 STL 中，就是以下四种“排序容器”：`multiset`、`set`、`multimap`、`map`。需要 `#include <set>` 或者 `#include <map>`。

- `multiset`：`multiset<T> st;`，能**自动排序**，排序规则为：表达式 `a<b` 为 `true`，则 `a` 排在 `b` 前面；可用 `st.insert` 添加元素，`st.find` 查找元素，`st.erase` 删除元素，复杂度都是 $$\log(n)$$；

  `multiset` 上的迭代器 `multiset<T>::iterator p;`，`p` 是迭代器，相当于指针，可用于指向 `multiset` 的元素，访问 `multiset` 中的元素要通过迭代器。与指针的不同：`multiset` 上的迭代器可 `++`，`--`，用 `!=` 和 `==` 比较，不可比大小，不可加减整数，不可相减。`st.begin()` 和 `st.end()`。

- `set`：`set` 和 `multiset` 的区别在于容器里**不能有重复元素**，`set` 插入元素可能不成功。

- `multimap`：`multimap` 容器里的元素，都是 **`pair` 形式**的 `multimap <T1, T2> mp;`，则 `mp` 里的元素都是如下类型：

  ```C++
  struct {
      T1 first;    // 关键字
      T2 second;   // 值
  }
  ```

  `multimap` 中的元素按照 `first` 排序，并可以按 `first` 进行查找，缺省的排序规则是 `a.first<b.first` 为 `true`，则 `a` 排在 `b` 前面。

- `map`：和 `multimap` 的区别在于：**不能有关键字重复**的元素；可以使用 `[]`，下标为关键字，返回值为 `first` 和关键字相同的元素的 `second`；插入元素可能失败。