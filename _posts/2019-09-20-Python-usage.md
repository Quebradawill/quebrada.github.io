---
layout: post
title: Python 的使用问题
date: 2019-09-20
categories: Programming
tags: Python
status: publish
type: post
published: true
author: Quebradawill
---

### 1. 新版 PyCharm 中 Matplotlib 图像不再弹出独立的显示窗口

PyCharm 从 2017.3 版之后，将 `matplotlib` 的绘图的结果默认显示在 `SciView` 窗口中，而不是弹出独立的窗口。如果要弹出独立窗口，则需取消勾选：File $$\to$$ Settings $$\to$$ Tools $$\to$$ Python Scientific $$\to$$ Show plots in tool window。

A **reference** is an alias for another variable. （**引用**就是另一个变量的别名）

引用的性质（非常重要）：

- Any changes made through the reference variable are actually performed on the original variable.（通过引用所做的读写操作实际上是作用于原变量上。）
- A reference must be initialized in declaration. （引用必须在声明的时候初始化。）
- Once initialized, the name of the reference cannot be assigned to other variables.（引用一旦初始化，引用名字就不能再指定给其它变量。）