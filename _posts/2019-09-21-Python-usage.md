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

### 11. 查看关键字

查看 Python 所有关键字，及判断某字符串是否是关键字：

```python
import keyword
keyword.kwlist
keyword.iskeyword('continue')
```

### 12. 查看某变量的身份和类型

```python
a = 5
id(a)
type(a)
```

### 13. Python 读取写入字典的方法

**1）使用 json 转换方法**

（1）字典写入 txt

```python
import json

dic = {
    'andy':{
        'age': 23,
        'city': 'beijing',
        'skill': 'python'
    },
    'william': {
        'age': 25,
        'city': 'shanghai',
        'skill': 'js'
    }
}

js = json.dumps(dic)
file = open('test.txt', 'w')
file.write(js)
file.close()
```

（2）读取 txt 中的字典

```python
import json

file = open('test.txt', 'r')
js = file.read()
dic = json.loads(js)
print(dic)
file.close()
```

**2）使用 str 转换方法**

（1）字典写入 txt

```python
dic = {
    'andy':{
        'age': 23,
        'city': 'beijing',
        'skill': 'python'
    },
    'william': {
        'age': 25,
        'city': 'shanghai',
        'skill': 'js'
    }
}

fw = open('test.txt', 'w+')
fw.write(str(dic))
fw.close()
```

（2）读取 txt 中的字典

```python
fr = open('test.txt', 'r+')
dic = eval(fr.read())
print(dic)
fr.close()
```

参考：[Python txt文件读取写入字典的方法（json、eval）](https://blog.csdn.net/li532331251/article/details/78203438)

### 14. Python 列表、字典读写文件：pickle 模块的基本使用

Python 的 pickle 模块实现了基本的数据序列和反序列化。通过 pickle 模块的序列化操作我们能够将程序中运行的对象信息保存到文件中去，永久存储；通过 pickle 模块的反序列化操作，我们能够从文件中创建上一次程序保存的对象。

基本接口：`pickle.dump(obj, file, [,protocol])`<br>
注解：<br>将对象 obj 保存到文件 file 中去；<br>protocol 为序列化使用的协议版本，0：ASCII 协议，所序列化的对象使用可打印的 ASCII 码表示；1：老式的二进制协议；2：2.3 版本引入的新二进制协议，较以前的更高效。其中协议 0 和 1 兼容老版本的 Python。protocol 默认值为 0；<br>file：对象保存到的类文件对象，file 必须有 `write()` 接口， file 可以是一个以 'w' 方式打开的文件或者一个 StringIO 对象或者其他任何实现 write() 接口的对象。如果protocol >= 1，文件对象需要是二进制模式打开的。

`pickle.load(file)`<br>注解：<br>从 file 中读取一个字符串，并将它重构为原来的 Python 对象；<br>file：类文件对象，有 `read()` 和 `readline()` 接口。

使用 pickle 模块将数据对象保存到文件

```python
import pickle

data1 = {'a': [1, 2.0, 3, 4+6j],
         'b': ('string', u'Unicode string'),
         'c': None}

selfref_list = [1, 2, 3]
selfref_list.append(selfref_list)

output = open('data.pkl', 'wb')

# Pickle dictionary using protocol 0.
pickle.dump(data1, output)

# Pickle the list using the highest protocol available.
pickle.dump(selfref_list, output, -1)

output.close()
```

使用 pickle 模块从文件中重构 Python 对象

```python
import pprint, pickle

pkl_file = open('data.pkl', 'rb')

data1 = pickle.load(pkl_file)
pprint.pprint(data1)

data2 = pickle.load(pkl_file)
pprint.pprint(data2)

pkl_file.close()
```

### 15.