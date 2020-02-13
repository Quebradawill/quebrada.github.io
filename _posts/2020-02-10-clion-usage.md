---
layout: post
title: CLion 使用技巧
date: 2020-02-10
categories: Programming
tags: C++
status: publish
type: post
published: true
author: Quebradawill
---

### 1、CLion 添加用户自定义拼写检查字典

File $\to$ Settings... $\to$ Editor $\to$ Spelling

### 2、CLion 中设置代码提示不区分大小写

File $\to$ Settings... $\to$ Editor $\to$ General $\to$ Code Completion，取消 “Match case”的复选。

### 2、选定内容敲引号、括号括住

File $\to$ Settings... $\to$ Editor $\to$ General $\to$ Smart Keys，复选 Surround selection on typing quote or brace

### 3、设置比较大的换行

File $\to$ Settings... $\to$ Editor $\to$ Code Style，设置 Hard warp at 中比较大的数值。

### 4、鼠标滑轮改变字号大小

File $\to$ Settings... $\to$ Editor $\to$ General，复选 Change font size (Zoom) with Ctrl+Mouse Wheel

### 5、保存当前窗口布局

Windows $\to$ Store Current Layout as Default

### 6、显示行号

File $\to$ Settings... $\to$ Editor $\to$ General $\to$ Appearance，复选 Show line numbers

### 7、自定义键盘快捷方式

如果默认代码提示和补全快捷键跟输入法冲突，如何解决：File $\to$ Settings... $\to$ Keymap

### 8、如何让光标不随意定位

File $\to$ Settings... $\to$ Editor $\to$ General $\to$ Appearance，去掉复选 Allow placement of caret after end of line。

### 9、中文乱码问题

File $\to$ Settings... $\to$ Editor $\to$ File Encondings 选择 Global Encoding 为 GBK。

### 10. CLion 新建文件具有相同文件头

File $\to$ Settings $\to$ File and Code Templates，选中“C Source File”，输入如下：

```c++
#parse("C File Header.h")
#if (${HEADER_FILENAME})
#[[#include]]# "${HEADER_FILENAME}"
#end

```

或者更复杂的：

```c++
/*
encoding: utf-8
@author: Quebradawill
@license: (C) Copyright 2010-2020.
@contact: dwqiu@foxmail.com
@software: CLion
@filename: ${NAME}.cpp
@datetime: ${DATE} ${TIME}
@desc:
*/


```

