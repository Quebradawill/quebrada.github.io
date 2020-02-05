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





**参考：**

1、[PyCharm 使用技巧](https://www.cnblogs.com/xxtalhr/p/11083279.html)

