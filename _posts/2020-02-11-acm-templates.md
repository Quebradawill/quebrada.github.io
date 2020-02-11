---
layout: post
title: 算法竞赛初级杂烩包
date: 2020-02-11
categories: Programming
tags: C++ ACM
status: publish
type: post
published: true
author: Quebradawill
---

### 1. C++ 参考网站

推荐大家去这些网站查询自己需要的东西

- [cpluscplus](http://www.cplusplus.com/reference/vector/vector/)
- [cppreference](http://en.cppreference.com/w/)
- [cppreferne中文](http://zh.cppreference.com/w/首页)

STL中有很多好用的容器和算法，非常好用。 简单介绍一下

- `vector` 向量（可以理解成可变长数组）
- `utility` （pair）
- `algorithm` （sort）
- `queue` 队列（queue，priority_queue）
- `list` （链表）
- `map` （key-value映射）
- `bitset`（用 int 各位表示的数组）

### 2. C++ 模板简单介绍

我们来看看 [cplusplus](http://www.cplusplus.com) 上[对 vector 的介绍](http://www.cplusplus.com/reference/vector/vector/)

```c++
template < class T, class Alloc = allocator<T> > class vector;    // generic template
```

```c++
vector<int> arr; 
queue<int> q;
vector< pair<int, int> > ponints;    // 注意那个空格
map<string, int> reflect;
```

使用 `typedef` 可以缩短代码长度，但是会降低可读性

```c++
typedef pair<int, int> pii;
vector<pii> points;
```

#### 2.0 iterator迭代器

迭代器是 C++ 的重要组成部分，但是这里不细讲，只是很多 STL 容器的方法都返回迭代器，不对迭代器有些了解是不行的 比如遍历一个 vector，可以这么做

```c++
vector<int> n;
for(vector<int>:: it=n.begin(); it!=n.end(); it++)
    cout << *it << endl;
```

访问 iterator 指向的内容可以直接用 `*it` 访问，如果访问其成员的话，也可以用 `->` 访问

```c++
vector< pair<int, int> > points;
for（vector< pair<int, int> >::iterator = n.begin(); it != n.end(); it++)
    cout << "x: " << it->first << "y: " << (*it).second << endl;
```

当然，我们平常是不会这么遍历数组的，这里只是演示下 iterator 的用法。









转自：[算法竞赛初级杂烩包](https://comzyh.com/blog/archives/1052/)