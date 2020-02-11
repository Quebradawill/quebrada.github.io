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
// 下面这句会编译失败，因为 make 出来的是 pair<double, double>，却赋值给了 pair<int, int>
pair<int, int> point4 = make_pair(1.0, 2.0);
```

**2、如何使用 pair**

pair 有两个主要成员 `first` 和 `second`，类型分别和为 `pair<U,V>` 里 U 和 V 的类型。

```c++
pair<int, double> yz = make_pair(1, 179.99999);
cout << yz.first << ":" << yz.second << endl;
cout << sizeof(yz.first) << " " << sizeof(yz.second) << endl;
```

### 2.3 sort

cplusplus 上关于 [sort](http://www.cplusplus.com/reference/algorithm/sort/) 的页面，sort 是 STL 里面一个非常重要的算法函数，排序效率非常高，几乎在任何时候都不需要手敲，所以，需要排序的时候，用 `sort` 吧！

`std::sort` 需要 `#include <algorithm>`

**1、基本排序**

第一种用法原型如下，传入两个 `RandomAccessIterator`，对 `[first,last)` 区间的元素进行排序，注意**区间左闭右开**，也就是传入的 **last 迭代器指向的位置不会参与排序**。

```c++
template <class RandomAccessIterator>
void sort(RandomAccessIterator first, RandomAccessIterator last);
```

对 int 数组从大到小排序，因为指针也是 `RandomAccessIterator` 的一种，所以 `sort` 直接传入指针就好了

```c++
const int N = 1000;
int arr[N];
// 注意 arr + N 指向的位置已经越界，但是 sort 传入的 last 参数就是指向最后一个元素后的一个位置
sort(arr, arr + N);
```

排序 `vector`，`vector` 能直接返回迭代器，很方便

```c++
vector<int> array;
sort(array.begin(), array.end());
```

**2、从大到小排序**

我们来看看 `sort` 的第二个原型

```c++
template <class RandomAccessIterator, class Compare>
void sort(RandomAccessIterator first, RandomAccessIterator last, Compare comp);
```

这里出现了第三个参数 `Compare comp`，`comp` 是一个**比较器**，可以有很多种玩法，如果我们想从大到小排序，把 `greater` 比较器传给 `sort` 就行了

```c++
vector<double> height;
sort(height.begin(), begin.end(), greater<double>());
```

**注意**：比较器的使用方法，比较器 `std::greater` 是一个**使用模板**的**结构体**，参见 cplusplus 对 [std::greater的介绍](http://www.cplusplus.com/reference/functional/greater/)，`greater` 的原型为 `template  struct greater;`

我们需要传入的实际上是一个 `greater<double>` 类型的变量，所以需要调用 `greater<double>` 的构造函数，最后写成 `greater()`

**3、排序 pair**

排序 `pair` 非常容易，直接 `sort` 的时候默认以 `first` 为第一关键字，`second` 为第二关键字排序。比如我们要对一系列事件已开始时间为第一关键字，结束时间为第二关键字排序。

```c++
vector< pair<int, int> > events;
sort(events.begin(), events.end());
```

**4、对自定义结构体进行排序（重载运算符方案）**

我们只需要重载结构体的 `<` 运算符即可，例如，对事件以开始时间排序：

```c++
struct T_event
{
    int begin_at, end_at;
    bool operator < (const T_event &other) const {
        return begin < other.begin;
    }
};

vector<T_event> events;
sort(events.begin(), events.end());
```

**注意**：比较重载运算符的两处 `const`，和引用 `&`。`const T_event &other` 防止比较函数对 `other` 进行修改，第二个 `const` 是限制比较函数不得修改所在的结构体的成员。如果不加这两个 `const` 限定就会爆满屏幕的编译错误。而比较的时候，另一个变量必须以引用方式 `&` 传递。

**5、双（多）关键字排序**

比如我们要对一个结构体 `vector` 排序，要求很复杂，首先按照 `a` 降序，然后按照 `c` 升序，再按照 `b` 降序，而且 `c` 是 double 值，排序的时候认为如果两个结构体的 `c` 下取整一样就算 `c` 一样，怎么搞？

```c++
struct Three_key
{
    int a, b;
    double c;
    bool opeartor < (const Three_key &other) const
    {
        if (a != other.a)
            return a > other.a;
        if ((int)c != (int)other.c)
            return (int)c < (int)other.c;
        return b > other.b;
    }
};
```

**6、使用比较函数排序**

有时候我们需要对一个数组进行多次排序，每次排序标准还不一样，怎么搞？

比如坐标，第一次按照 X 坐标升序排序，搞点什么，然后再按照 Y 坐标降序排序，那么可以这样写：

```c++
vector< pair<int, int> > points;
bool cmp_x_inc(const pair<int, int> &p1, const pair<int, int> &p2) {
    return p1.first < p2.first;
}
bool cmp_y_dec(const pair<int, int> &p1, const pair<int, int> &p2) {
    return p1.second < p2.second;
}
 
sort(points.begin(), points.end(), cmp_x_inc);    //X 升序
//do something...
sort(points.begin(), points.end(), cmp_y_dec);    //Y 降序
```

**7、对字符串排序（使用结构体，不推荐）**

首先定义结构体

```c++
#include <cstdio>    // strcmp 函数在这里
struct T_String
{
    char str[10000];
    bool operator < (const T_String &s) {
        return strcmp(str, s.str) < 0;
    }
};
vector<T_String> strs;
sort(strs.begin(), strs.end());
```

这种方法虽然简单，但是有很多缺陷，比如：

- 因为字符串存储在结构体中，造成结构体很大，交换结构体的开销很大；<br>
- 不能对常量字符串排序。

一般不推荐使用。

**8、对字符串排序（使用 `char*` 数组）**

由于交换字符串开销很大，但是字符串本身是不会改变的，我们并不需要交换字符串本身，最终只需要能顺字典序访问所有字符串就行了，那么，可以对每个字符串建立一个指针，然后采用上面的比较函数方法对指针进行排序即可。

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <vector>
using namespace std;
int N = 0;
char strs[1000][1000];
vector<char *> strs_sorted;
bool char_ptr_cmp(const char *a, const char *b)
{
	return strcmp(a,b) < 0;
}
int main()
{
	while (scanf("%s", strs[N++]) != EOF);
	for (int i = 0; i < N; i++)
		strs_sorted.push_back(strs[i]);
	sort(strs_sorted.begin(), strs_sorted.end(), char_ptr_cmp);
	printf("sorted strs are:");
	for (vector<char*>::iterator it = strs_sorted.begin(); it != strs_sorted.end(); it++)
		printf("%s", *it);
	return 0;
}
```

**9、（拓展）使用自定义比较器（伪函数）**

如果我们定义了一个结构体，里面有一个长度为 10 的 int 数组

```c++
struct T_arr
{
    int arr[10];
};
vector<T_arr> array;
```

我们需要对 array 进行 10 次排序，每次分别以其中一个下标（arr[0], arr[1], …）为关键字进行排序，怎么办？

写 10 个比较函数？听起来好靠谱的样子~~ 才怪！

还记得我们刚才提到的 `greater` 吗，`std::greater` 是一个结构体，我们来看看它的实现

```c++
template <class T> struct greater : binary_function <T, T, bool> {
    bool operator() (const T& x, const T& y) const {return x > y;}
};
```

能够看到，`greater` 重载了一个奇怪的运算符 `()`，`sort` 比较两个值的时候会使用这个运算符来对两个元素进行比较，我们也可以这么写

```c++
#include <cstdio>
#include <algorithm>
#include <vector>
using namespace std;
struct T_arr
{
	int arr[4];
};
vector<T_arr> to_sort;
struct T_arr_cmp
{
	int index;
	T_arr_cmp(int index): index(index) {}    // 构造函数
	bool operator ()(const T_arr &a, const T_arr &b)
	{
		return a.arr[index] < b.arr[index];
	}
};
int main()
{
	int N;
	scanf("%d", &N);
	to_sort.resize(N);
	for (int i = 0; i < N; i++)
		for (int j = 0; j < 4; j++)
			scanf("%d", &to_sort[i].arr[j]);
	for (int j = 0; j < 4; j++)
	{
		sort(to_sort.begin(), to_sort.end(), T_arr_cmp(j));    // 传入比较器，以数组的第 j 位为关键字
		printf("the to_sort sort by arr[%d] is:", j);
		for (int i = 0; i < N; i++)
			printf("%4d %4d %4d %4d", to_sort[i].arr[0], to_sort[i].arr[1], to_sort[i].arr[2], to_sort[i].arr[3]);
	}
	return 0;
}
```

上面的代码 给 `sort` 函数传入了一个结构体，结构体有一个成员变量 `index`，表示用 `arr[index]` 为关键字进行比较，而这个 `index`，这个 `index` 是在结构体构造的时候由构造函数传进去的 相当于

```c++
T_arr_cmp cmp(j);
sort(array.begin(), array.end(), cmp);
```

**10、（拓展）使用lambda函数进行排序（C++11）**

每次都要定义一个排序函数太麻烦了有木有！看代码的时候找比较函数往上滚滚轮都快疯了，还打断思路有木有！！ 写比较器好多行好麻烦有木有！！！

然而 C++11 标准提供了 lambda 函数（匿名函数，现声明现调用），写出的代码就好看多了。参见：[Lambda函数（C++11 起）](http://zh.cppreference.com/w/cpp/language/lambda)

上面使用比较器对数组多处排序可以改成这样，注意使用 `g++ xxx.cpp -std=c++11` 来编译（启用 C++11 标准）

```c++
#include <cstdio>
#include <algorithm>
#include <vector>
using namespace std;
struct T_arr
{
	int arr[4];
};
vector<T_arr> to_sort;
int main()
{
	int N;
	scanf("%d", &N);
	to_sort.resize(N);
	for (int i = 0; i < N; i++)
		for (int j = 0; j < 4; j++)
			scanf("%d", &to_sort[i].arr[j]);
	for (int j = 0; j < 4; j++)
	{
		sort(to_sort.begin(), to_sort.end(), [&j](const T_arr &a, const T_arr &b)->bool
		{
			return a.arr[j] < b.arr[j]; 
		}); 
		printf("the to_sort sort by arr[%d] is:", j);
		for (int i = 0; i < N; i++)
			printf("%4d %4d %4d %4d", to_sort[i].arr[0], to_sort[i].arr[1], to_sort[i].arr[2], to_sort[i].arr[3]);
	}
	return 0;
}
```

我们看看 cppreference 中给出的第一种 lambda 函数语法：`[ capture ] ( params ) mutable exception attribute -> ret { body }`

再看看我们在 `sort` 最后一个参数写了什么？

```c++
sort(to_sort.begin(), to_sort.end(), [&j](const T_arr &a, const T_arr &b)->bool
{
	return a.arr[j] < b.arr[j]; 
});
```

首先我们用 `[&j]` 捕获了 `j`，这样排序函数内部就可以直接使用 lambda 外面的 `j` 啦，不用构造难用的比较器再传入 index 啦。 剩下的和之前说的使用函数比较没什么区别，只是把函数定义放在调用位置而且没起名而已~

我们再来看看使用指针排序字符串的程序，使用 lambda 函数可以改成这样

```c++
sort(strs_sorted.begin(), strs_sorted.end(), [](const char *a, const char *b)->bool
{
	return strcmp(a,b) < 0;
});
```

#### 2.4 queue

queue 是 STL 提供的一个队列类，比手写队列有很多优势，`std::queue` 需要 `#include <queue>`

queue 的主要成员：

- `push(const value_type& val)` 向队列压入一个元素；<br>
- `pop()` 将队头弹出；<br>
- `front()` 取出队头；<br>
- `empty()` 判断队列是否为空。

```c++
queue<int> q;
q.push(0);
while (!q.empty())
{
    int h = q.front();
    q.pop();
    if (h < 100)
        q.push(h+1)
    printf("%d", h);
}
```

注意，要及时判断 `queue` 的 `empty()`，你只有一次机会，如果队列为空再 `pop()` 的话之后 `empty()` 八成是返回 false，因为 size 变成 $2^{32} – 1$ 了，然后什么奇怪的事情都有可能发生。

### 2.5 priority_queue

顾名思义，优先队列，是算法竞赛中的非常重要数据结构，Dijkstra 等算法 少不了它。 可以参考 cplusplus 中的 [priority_queue](http://www.cplusplus.com/reference/queue/priority_queue/) 和 [它的构造函数](http://www.cplusplus.com/reference/queue/priority_queue/priority_queue/)。

`priority_queue` 的使用方法和 `queue` 基本一致，主要区别是 `front()` 换成了 `top()` ，因为 `priority_queue` 是使用**堆**实现的。

注意，`priority_queue` 默认是大根堆，也就是大的元素先出队，想让小的元素先出队则应当给出比较器。

**1、重载结构体运算符实现“小根堆”**

我们经常会遇到想要所有元素以某种优先方法出队，比如 Dijkstra 算法中，想要当前距离小的点先出队，我们可以这样做：

```c++
struct State
{
    int point, dis;
    bool operator < (const State &s)const {
        return dis > s.dis;
    }
};
priority_queue<State> q;
```

无论你使用怎样的方法，都不能改变 `priority_queue` 是一个大根堆的事实，我们只是重载了运算符让小的元素比较起来大了而已，事实上，这是算法竞赛中非常常用的一种写法，一般来说足够用了。

**2、（拓展）自定义 priority_queue 的比较方法**

上面那个例子，明明可以用 `pair` 存下来的嘛，如果我强行要使用 `pair` 这种不能重载运算符的怎么办？ 或者有的时候我们不能重载某个结构体的 `<` 运算符怎么办？

我们先来看看 priority_queue 的原型

```c++
template <class T, class Container = vector<T>,
    class Compare = less<typename Container::value_type> > class priority_queue;
```

可以看到，模板参数有三个，不过后面两参数已经有默认值了，如果我们想自定义比较器，那么三个参数都要填。还记得上面 `sort` 里面讲的比较器（仿函数）嘛，第三个参数填入一个仿函数就好了。

```c++
struct pair_cmp
{
	bool operator()(const pair<int, int> &a, const pair<int, int> &b) {
		return a.second > b.second;
	}
};
 
priority_queue<pair<int, int>, vector<pair<int, int> >, pair_cmp> q;
```

#### 2.6 set

`set` 是有序集合，使用 `set` 需要 `#include <set>`

set 是使用平衡树实现的，可以在 $O(\log(N))$ 的时间内完成插入删除修改操作。

set 常用来进行各种判重，比如搜索判重，状态判重等等。

cplusplus上[对set的介绍](http://www.cplusplus.com/reference/set/set/)。

声明一个 `set`

```c++
set<int> iset;
```

常用API有：

- `set::insert(val)` 插入一个元素；<br>
- `set::empty()` 判定 set 是否为空；<br>
- `set::clear()` 清空 set；<br>
- `set::size()` 取得 set 大小；<br>
- `set::erase()` 删除元素（有三只牛股用法）；<br>
- `set::find(val)` 返回指定元素迭代器，不存在的话返返回 `end()`；<br>
- `set::lower_bound(val)`；<br>
- `set::upperbound(val)`；<br>
- `set::begin()` 返回从左开始的迭代器（从小到大）；<br>
- `set::end()` 返回；<br>
- `set::rbegin`, `set::rend()`；<br>
- `set::count(val)` 返回 set 指定 val 的个数。

显然只能返回 1（有）或者 0（没有），可以用来判断元素是否存在。

转自：[算法竞赛初级杂烩包](https://comzyh.com/blog/archives/1052/)