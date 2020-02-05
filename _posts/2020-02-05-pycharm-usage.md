---
layout: post
title: PyCharm使用技巧
date: 2020-02-05
categories: Programming
tags: Python
status: publish
type: post
published: true
author: Quebradawill
---

### 1、代码排版，自动PEP8

1）安装 Python 代码自动排版为 PEP8 风格的工具，`pip install autopep8`<br>2）autopep8 的配置：打开 File $\to$ Setting... $\to$ External Tools，点击“+”，Name：autopep8，Description：autopep8，Program：autopep8，Arguments：`--in-place --aggressive --aggressive $FilePath$`，Working directory：`$ProjectFileDir$`，Output filters：`$FILE_PATH$\:$LINE$\:$COLUMN$\:.*`

### 2、误删文件，一秒找回

在你的项目目录里，点击右键，有个 `Local History` 的选项，再点击子选项 `Show History`，你可以看到这里有个记录板。如果你想恢复删除的文件，就在删除的记录项点击右键，选择 `Revert` 即可恢复。

### 3、代码模板，效率编码

File $\to$ File and Code Templates，PyCharm 提供的这个代码模板，可以说是相当实用的一个功能了。它可以在你新建一个文件时，按照你预设的模板给你生成一段内容，比如解释器路径，编码方法，作者详细信息等。

在编写代码过程中也可以敲 `Ctrl+J`，弹出 Live Templates。

### 4、关闭烦人的灯泡提示

先来说下这个灯泡提示是什么，有什么用？

当我们在代码里面有语法错误，或者代码编写不符合 pep8 代码规范时，鼠标选择有问题的代码，就会自动弹出小灯泡，这个灯泡是有颜色之分的，如果是红灯泡，一般都是语法问题，如果不处理会影响代码运行。而如果是黄灯泡，就只是一个提示，提示你代码不规范等，并不会影响程序的运行。

虽然这个灯泡，是出于善意之举，但我认为它确实有点多余（可能是我个人没有使用它的习惯），要是语法错误会有红色波浪线提示。你可能会说灯泡不仅起到提示的作用，它还可以自动纠正代码，我个人感觉并没有人工校正来得效率，来得精准。

基于有时还会像知乎上这个朋友说的这样，会挡住我们的代码，会经常误点，这确实也是一个烦恼。

Pycharm （2018 版本）里是有开关按钮的，将选项（File $\to$ Settings... $\to$ Editor $\to$ General $\to$ Appearance $\to$ Show intention bulb）取消勾选，就可以关闭这个功能。

### 5、关闭碍眼的波浪线

Pycharm 本身会实时地对变量名进行检查，如果变量名不是一个已存在的英文单词，就会出现一条波浪线，当一个变量里有多个单词时，Python 推荐的写法是用下划线来分隔（其他语言可能会习惯使用**驼峰式命名法**，但 Python 是使用下划线），那么如何关闭这个非语法级别的波浪线呢？很简单，它的开关就在你的右下角那个像 人头像 一样的按钮。

**参考：**

1、[PyCharm 使用技巧](https://www.cnblogs.com/xxtalhr/p/11083279.html)

