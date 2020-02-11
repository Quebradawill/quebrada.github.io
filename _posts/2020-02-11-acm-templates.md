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

推荐大家去这些网站查询自己需要的东西：

- [cpluscplus](http://www.cplusplus.com/reference/vector/vector/)
- [cppreference](http://en.cppreference.com/w/)
- [cppreferne中文](http://zh.cppreference.com/w/首页)

STL中有很多好用的容器和算法，非常好用。 简单介绍一下：

- `vector` 向量（可以理解成可变长数组）
- `utility` （pair）
- `algorithm` （sort）
- `queue` 队列（queue，priority_queue）
- `list` （链表）
- `map` （key-value映射）
- `bitset`（用 int 各位表示的数组）

### 2. C++ 模板简单介绍

我们来看看 [cplusplus](http://www.cplusplus.com) 上[对 vector 的介绍](http://www.cplusplus.com/reference/vector/vector/)。

```c++
template < class T, class Alloc = allocator<T> > class vector;    // generic template
```

```c++
vector<int> arr; 
queue<int> q;
vector< pair<int, int> > ponints;    // 注意那个空格
map<string, int> reflect;
```

使用 `typedef` 可以缩短代码长度，但是会降低可读性。

```c++
typedef pair<int, int> pii;
vector<pii> points;
```

#### 2.0 iterator迭代器

迭代器是 C++ 的重要组成部分，但是这里不细讲，只是很多 STL 容器的方法都返回迭代器，不对迭代器有些了解是不行的 比如遍历一个 vector，可以这么做：

```c++
vector<int> n;
for(vector<int>:: it=n.begin(); it!=n.end(); it++)
    cout << *it << endl;
```

访问 iterator 指向的内容可以直接用 `*it` 访问，如果访问其成员的话，也可以用 `->` 访问

```c++
vector< pair<int, int> > points;
for (vector< pair<int, int> >::iterator = n.begin(); it != n.end(); it++)
    cout << "x: " << it->first << "y: " << (*it).second << endl;
```

当然，我们平常是不会这么遍历数组的，这里只是演示下 iterator 的用法。

#### 2.1 vector

vector 是 C++ 中最常用的数据结构，相当于可变长数组，效率和使用数组没有明显差别。

常见用法，建立邻接表样例：

```c++
#include <vector>
const int MAXN = 100;
vector<int> tab[MAXN+1];
int main()
{
    cin >> N >> M;
    for (int i = 1; i <= N; i++)
        tab[i].clear();
    for (int i = 0; i < M；i++)
    {
        int a, b;
        cin >> a >> b;
        tab[a].push_back(b);
    }
}
```

我们来看一看上面发生了什么：

1、声明 `vector arrary;` 声明了一个 vector，而 `vector tab[101]` 则声明了一个 vector 的数组，访问 5 点引出的第 0 条边使用 `tab[5][0]` 就可以了；<br>2、`tab[i].clear` 是将一个 vector 清空。 这一句在这个程序里是没有用的，但是很多题目需要多组输入输出，上一个 case 的邻接表没有清空是要死得很惨的；<br>3、`tab[a].push_back(b)` 在 vector `tab[a]` 最后添加一个元素，值为 b。

**1、如何使用这张邻接表呢**

遍历a点所有连接的点

```c++
vecotr<int> tab[100];
for (int i = 0; i < tab[a].size(); i++)
    cout << tab[a][i] << endl;
```

注意坑

```c++
for (int i = 0; i <= tab[a].size() - 1; i++)
    cout << endl;
```

这样是有可能会跪掉的，因为 `vector` 的 `size()` 返回的是一个`size_t` 也就是 `unsigned int` ，即 32 位无符号数 类型，这样如果 vector 为空的话，`size()` 返回 0 ，然而 32 位无符号数 0 – 1 = 4294967295，这样会导致访问越界然后开心的 RE 掉。

**2、其他重要的成员**

- `vector::resize()` 如果你想立即得到一个长度为 100 的数组而不想一个一个 push_back 进去的话，直接 `xxx.resize(100)` 就好了；<br>
- `vector::begin()` 返回首个元素的迭代器；<br>
- `vector::end()` 返回终止位置的迭代器，注意并非指向最后一个元素，而是比最后一个元素还要往后一个元素的位置。

#### 2.2 pair

使用 [pair](http://www.cplusplus.com/reference/utility/pair/) 需要 `#include <utility>`

pair 代表的是数据对，可以用来表示二维坐标 $(x, y)$，图中的边之类的东西，pair 的两个分量类型可以不同，像下面这样。

```c++
typedef pair<int, int> point;    //藐视一个点
typedef pair<string, int> name_and_id_pair;    // 学生姓名和学号
typedef pair<int, double> id_to_height_pair;   // 学生学号和身高
```

**1、如何制造 pair**

常用的方式有构造函数法和 `make_pair`

```c++
pair<int, int> point1 = make_pair(1, 1);
pair<double, double> point2 = make_pair(2.0, 3.0);
pair<int, int> point3 = pair<int, int>(1.0, 2.0);
pair<int, int> point4 = make_pair(1.0, 2.0);    // 这句会编译失败，因为 make 出来的是 pair<double, double>，却赋值给了 pair<int, int>
```

**2、如何使用 pair**

pair 有两个主要成员 `first` 和 `second`，类型分别和为 `pair<U,V>` 里 U 和 V 的类型。

```c++
pair<int, double> yz = make_pair(1, 179.99999);
cout << yz.first << ":" << yz.second << endl;
cout << sizeof(yz.first) << " " << sizeof(yz.second) << endl;
```





转自：[算法竞赛初级杂烩包](https://comzyh.com/blog/archives/1052/)