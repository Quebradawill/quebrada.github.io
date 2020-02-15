---
layout: post
title: Python 程序设计 - 浙江大学
date: 2020-02-12
categories: Programming
tags: Python
status: publish
type: post
published: true
author: Quebradawill
---

## 第一章 Python 语言概述

1.2、`id( )` 是 Python 的内置函数， 可以显示对象的地址 。

1.3、输入及输出函数

读取整数

```python
a = int(input())
```

一行输入多个值

```python
m, n = input('请输入多个值：').split()
```

## 第二章 用 Python 语言编写程序

2.1、数值类型

`complex()` 函数用来创建一个值为 $\textrm{real} + \textrm{imag} \cdot j$ 的函数。

## 第三章 使用字符串、列表和元组



## 第四章 条件、循环和其他语句

异常处理：

```python
try:
    statements_1
except exception_type_1:
    statements_2
except exception_type_2:
    statements_3
...
except exception_type_N:
    statements_N+1
except:
    statements_N+2
else:
    statements_N+3
finally:
    statements_N+4
```

## 第五章 集合与字典



## 第六章 函数

lambda 函数的一般形式是关键字 lambda 后面跟一个或多个参数，然后紧跟一个冒号， 后面是一个表达式。  

函数的返回值：如函数没有用 `return` 语句返回，这时函数返回的值为 `None`； 如果 `return` 后面没有表达式，调用的返回值也为 `None`。`None `是 Python 中一个特殊的值，虽然它不表示任何数据，但仍然具有重要的作用。  

命名空间和作用域：

1、变量可被访问范围称为**变量的作用域**，也称为**变量命名空间**或**变量名字空间**。 Python 程序用命名空间区分不同空间的相同名字。<br>2、Python 解释器启动时建立一个全局命名空间，全局变量就放在这个空间，还建立内置命名空间（built-in namespace），记录所有标准常量名、标准函数名等。 在全局命名空间中定义的变量是全局变量。<br>3、每一个函数定义自己的命名空间，函数内部定义的变量是局部变量。如果在一个函数中定义一个变量 $x$，在另外一个函数中也定义 $x$ 变量，因为是在不同的命名空间，所以两者指代的是不同的变量。可以通过多种方式获取其他命名空间的变量。  

使用 `dir(__builtins__)` 可列出所有内置函数，比较常用的几个内置函数如下：

1、`sorted` 函数对字符串，列表，元组，字典等对象进行排序操作。<br>2、`map` 会根据提供的函数对指定序列做映射。<br>3、`zip` 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表或迭代器；<br>4、Python 是一种动态语言，它包含很多含义。Python变量类型，操作的合法性检查都在动态运行中检查；运算的代码需要到运行时才能动态确定；程序结构也可以动态变化，容许动态加载新模块等。`eval` 和 `exec` 这两个函数就体现了这个特点。`eval` 是计算表达式，返回表达式的值。`exec` 可运行 Python 的程序，返回程序运行结果。<br>5、`all` 和 `any` 函数将可迭代的对象作为参数。

如下方法也可以列出所有内置函数：

```python
import builtins
dir(builtins)
```

引入模块的方法：

```python
import module_name
```

下面这种方法引入模块中的所有函数，调用的时候不需要再加模块名

```python
from module_name import *
```

下面这种方法引入模块中的单个函数，调用的时候也不需要再加模块名

```python
from module_name import function_name
```

**包**：我们已使用过单行代码、多行函数、独立程序以及同一目录下的多个模块。为了使 Python 应用更具可扩展性，你可以把多个模块文件组织成目录，称之为**包**。包是一个目录，其中包含一组模块文件和一个 init.py 文件。如果模块存在于包中，使用 `import package.module_name` 形式导入包中模块，用以下形式调用函数：`package.module_name.function_name`。

## 第七章 文件



## 第八章 类和对象



## 第九章 Web 应用程序开发和网络爬虫