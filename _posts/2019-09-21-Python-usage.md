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

### 1. 更改 jupyterlab 中的字号

C:\Users\Quebradawill\AppData\Local\Programs\Python\Python37\share\jupyter\lab\themes\@jupyterlab\theme-dark-extension 文件夹下的 index.css 中的字号（font-size）

### 2. 一键升级所有过期库

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

### 3. Jupyter 中显示 matplotlib 的图片

发现有个比较简单的解决方案，就是画图前先运行 `%matplotlib inline` 命令，就可以解决了。

### 4. 更改 pip 源至国内镜像，显著提升下载速度

Windows 下，直接在 user 目录中创建一个 pip 目录，如：C:\Users\xx\pip，新建文件 pip.ini，内容如下：

[global]

index-url = https://pypi.tuna.tsinghua.edu.cn/simple

### 5. Python 更改默认路径

在快捷方式属性的 Start in 中更改

### 6. 查看内建函数

`dir(__builtins__)`

### 7. Python 中 `if __name__ == '__main__':` 的解析

模块是对象，并且所有的模块都有一个内置属性 `__name__`。一个模块的 `__name__` 的值取决于您如何应用模块。如果 `import` 一个模块，那么模块 `__name__` 的值通常为模块文件名，不带路径或者文件扩展名。但是您也可以像一个标准的程序样直接运行模块，在这种情况下，`__name__` 的值将是一个特别缺省“`__main__`”。

### 8. 更改 jupyterlab 中的字号

C:\Users\Quebradawill\AppData\Local\Programs\Python\Python37\share\jupyter\lab\themes\@jupyterlab\theme-dark-extension 文件夹下的 index.css 中的字号（font-size）

### 9. Python 列出文件夹下所有文件的 4 种方法

**方法 1：**使用 `os.listdir`

````python
import os
for filename in os.listdir(r'C:\Windows'):
    print(filename)
````

**方法 2：**使用 `glob` 模块，可以设置文件过滤

```python
import glob
for filename in glob.glob(r'C:\Windows\*.exe'):
    print(filename)
```

**方法 3：**通过 `os.path.walk` 递归遍历，可以访问子文件夹

```python
import os.path

def process_directory(args, dirname, filenames):
    print('Directory: ', dirname)
    for filename in filenames:
        print('File: ', filename)
        
os.path.walk('r'C:\Windows', process_directory, None)
```

**方法 4：**非递归

```python
import os
for dirpath, dirnames, filenames in os.walk(r'C:\Windows'):
    print('Directory: ', dirpath)
    for filename in filenames:
        print('File: ', filename)
```

另外，判断文件或目录是否存在：

```python
import os
os.path.isfile('text.txt')
os.path.exists(directory)
```

### 10. Pandas 读取 excel 文件有时候提示 index out of range

确认是 Excel 文件，有时候保存为 Strict Open XML Spreadsheet 文件，其扩展名也是 .xlsx。

### 11.
