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

### 2. 让 PyCharm 运行程序时只打开一个 Python Console

1) 改变单个文件默认运行方式：取消 Run $$\to$$ Edit Configuration $$\to$$ Run with Python Console 复选；

2) 修改整体运行方式：取消 Run $$\to$$ Edit Configuration $$\to$$ Templates $$\to$$ Python $$\to$$ Run with Python Console 复选。

### 3. 更改 jupyterlab 中的字号

C:\Users\Quebradawill\AppData\Local\Programs\Python\Python37\share\jupyter\lab\themes\@jupyterlab\theme-dark-extension 文件夹下的 index.css 中的字号（font-size）

### 4. 一键升级所有过期库

查询所有过期库：`pip list --outdated`

升级单个库：`pip install --upgrade …`

批量升级过期库：

```Python
import pip
from subprocess import call
from pip._internal.utils.misc import get_installed_distributions
for dist in get_installed_distributions():
    call("pip install --upgrade " + dist.project_name, shell=True)
```

### 5. Jupyter 中显示 matplotlib 的图片

发现有个比较简单的解决方案，就是画图前先运行 `%matplotlib inline` 命令，就可以解决了。

### 6. 更改 pip 源至国内镜像，显著提升下载速度

Windows 下，直接在 user 目录中创建一个 pip 目录，如：C:\Users\xx\pip，新建文件 pip.ini，内容如下：

[global]

index-url = https://pypi.tuna.tsinghua.edu.cn/simple

### 7. Python 更改默认路径

在快捷方式属性的 Start in 中更改

### 8. 查看内建函数

`dir(__builtins__)`

### 9. Python 中 `if __name__ == '__main__':` 的解析

模块是对象，并且所有的模块都有一个内置属性 `__name__`。一个模块的 `__name__` 的值取决于您如何应用模块。如果 `import` 一个模块，那么模块 `__name__` 的值通常为模块文件名，不带路径或者文件扩展名。但是您也可以像一个标准的程序样直接运行模块，在这种情况下，`__name__` 的值将是一个特别缺省“`__main__`”。