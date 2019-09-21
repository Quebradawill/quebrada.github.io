---
layout: post
title: Python 的使用问题
date: 2019-09-21
categories: Programming
tags: Python
status: publish
type: post
published: true
author: Quebradawill
---

### 1. 新版 PyCharm 中 Matplotlib 图像不再弹出独立的显示窗口

PyCharm 从 2017.3 版之后，将 `matplotlib` 的绘图的结果默认显示在 `SciView` 窗口中，而不是弹出独立的窗口。如果要弹出独立窗口，则需取消勾选：File $$\to$$ Settings $$\to$$ Tools $$\to$$ Python Scientific $$\to$$ Show plots in tool window。
