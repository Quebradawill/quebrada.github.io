---
layout: post
title: C++ 问题汇总
date: 2019-09-12
categories: Programming
tags: C++
status: publish
type: post
published: true
mathjax: true
author: Quebradawill
---

#### 1. VS 2019 支持 C++ 17 标准

Debug $$\to$$ Properties... $$\to$$ C/C++ $$\to$$ Language $$\to$$ C++ Language Standard

#### 2. VS 2019 打开命令行窗口显示 C++ 标准

Project Properties... $$\to$$ C/C++ $$\to$$ Command Line $$\to$$ Additional Options，填写“/Zc:__cplusplus”。

#### 3. 更改默认项目

右键单击 Solution，选择 Properties，选中 Current selection。

#### 4. VS 2019 中的 Extensions

一、Extensions $$\to$$ Manage Extensions，安装 Productivity Power Tools 2017/2019。这是一个扩展套装，其中包含12个扩展（截至2019年6月）。只要安装这一个扩展套装，也就安装了其中包含的12个扩展。这12个扩展是：

1. Align Assignments
2. Copy As Html
3. Double-Click Maximize
4. Fix Mixed Tabs
5. Match Margin
6. Middle Click Scroll
7. Peek Help
8. Power Commands for Visual Studio
9. Quick Launch Tasks
10. Solution Error Visualizer
11. Shrink Empty Lines
12. Time Stamp Margin

二、 安装 CodeMaid，CodeMaid 能够帮助我们在保存代码的时候，清理代码中无用的空格和空行。

三、Open In Explorer，该扩展在解决方案管理器中添加了一些类似文件资源管理器的功能。只要在解决方案管理器中单击鼠标右键，在弹出菜单中就能看到“在资源管理器中打开文件夹”、“拷贝文件”等功能。

四、Trailing Whitespace Visualizer，该扩展能够显示行尾无用的空格。当然，如果安装了 CodeMaid 扩展的话，在保存代码时，CodeMaid 会自动将行尾五用空格删除。

五、Viasfora，该扩展可以使程序中的成对匹配的大中小括号以不同的颜色显示，便于我们将括号的左右半边匹配。

六、Visual Studio IntelliCode，基于机器学习的代码编写辅助工具。目前功能还比较弱。感兴趣可以尝尝鲜。

七、PowerMode，敲键盘写代码的时候，字符会出现烟花效果。本课程中相当一部分示例都有该效果。

八、Snippetica，代码片段工具。按下特定的字符或者字符组合，然后按 TAB 键，Snippetica 就会将该扩展中存储的一些代码片段直接粘贴到你的编辑器中。该工具能比较有效地提升编码的速度。你可以尝试输入  forr  然后按 tab 键，它会自动将基于范围的 for 循环的框架代码贴到你的编辑器中。

九、VSColorOutput，该扩展与 Output enhancer 扩展的功能类似，但是比 Output enhancer 好用，所以如果同时安装了 Output enhancer 扩展的话，将其禁用即可。

十、Smooth Scroll，让代码编辑器窗口的滚动更平滑。

十一、Word Highlight With Margin，当你用鼠标选中某个单词/标识符后，该扩展可以将所有的单词/标识符同时加亮显示。这是一个非常有用的扩展。

十二、VSColorOutput，以不同颜色显示编译输出。

#### 2. C++ 删除字符串中某字符

```C++
string str = ",hello,";
str.erase(remove(str.begin(), str.end(), ','), str.end());    // hello
```

