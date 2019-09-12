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

要使用其中的算法，需要 `#include <algorithm>`

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

