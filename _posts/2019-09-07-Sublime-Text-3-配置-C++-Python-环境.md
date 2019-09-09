---
layout: post
title: Sublime Text 3 配置 C++ 环境
date: 2019-09-07
categories: Programming
tags: C++
status: publish
type: post
published: true
author: Quebradawill
---

## C++ 环境

可以说对 Sublime Text 3 是真爱了，我最爱的代码编辑器，没有之一。网上有很多帖子说明如何配置，现总结如下：

###  MinGW的系统环境配置

- 安装 [MinGW](http://jaist.dl.sourceforge.net/project/mingw/Installer/mingw-get-setup.exe)；
- 设置环境变量：C:\Software\mingw64\bin；
- 运行 cmd，输入 `g++ -v`，看是否配置成功。

### 新建 Sublime Text 3的 C++ 语言编译环境

- 打开 Sublime Text 3 选择，Tools $$\to$$ Build System $$\to$$ New Build System ...，输入如下代码保存为 C++.sublime-build；

  ```javascript
  {
      "encoding": "utf-8",
      "working_dir": "$file_path",
      "shell_cmd": "g++ -Wall -std=c++11 \"$file_name\" -o \"$file_base_name\"",
      "file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
      "selector": "source.c++",
  
      "variants": 
      [
          {   
          "name": "Run",
              "shell_cmd": "g++ -Wall -std=c++11  \"$file\" -o \"$file_base_name\" && start cmd /c \"\"${file_path}/${file_base_name}\" & pause\""
          }
      ]
  }
  ```

- 保存之后，在 Tools $$\to$$ Build System 之下选择刚才保存的 C++；

- 编写一段代码，按 `Ctrl+Shift+B` 编译，按 `Ctrl+B` 运行；

### 新建代码片段（snippet）

选择 Tools $$\to$$ Developer $$\to$$ New Snippet...，在 CDATA 里输入代码片段，在 `<tabTrigger>` 里输入触发键，如下：

```javascript
<snippet>
	<content><![CDATA[

#include <iostream>
using namespace std;

int main()
{

    return 0;
}
]]></content>
	<!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
	<!-- <tabTrigger>hello</tabTrigger> -->
	<!-- Optional: Set a scope to limit where the snippet will trigger -->
	<!-- <scope>source.python</scope> -->
	<tabTrigger>newc</tabTrigger>
</snippet>
```

保存为 `newc.sublime-snippet`。

### 安装的插件

选择 Preferences $$\to$$ Package Control，输入 package install，然后搜索安装插件。

- `SublimeCodeIntel`，`SublimeREPL`，`SublLinter`，`SublimeClang` 等

## Python 环境

### Python 运行弹出 cmd 窗口

选择 Tools $$\to$$ Build System $$\to$$ New Build System ...，输入如下代码保存为 Python-3.7.4.sublime-build；

```javascript
{
	"cmd": ["python","-u","$file"],
	"file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
	"selector": "source.python",
	"variants":
		[
			{
				"name":"Run",
				"shell": true,
				"cmd": ["start","cmd","/c", "python $file &echo. & pause"],
				"working_dir": "${file_path}",
			}
		]
}
```

**参考：**

[sublime text 3配置c/c++编译环境，小白也能看懂！](https://www.jianshu.com/p/4cb799abb420)

[Sublime text 3中C++环境配置及命令行运行窗口创建](https://blog.csdn.net/huangmx1995/article/details/52823188)

[编写 Sublime Text 代码片段](https://www.jianshu.com/p/21b9fc973af4)