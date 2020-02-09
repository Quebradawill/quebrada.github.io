---
layout: post
title: 计算机相关问题
date: 2019-10-08
categories: Computer
tags: Computer
status: publish
type: post
published: true
author: Quebradawill
---

### 1. Win 10 安装 iCloud Drive 并更改默认同步位置

Windows 10 从 Microsoft Store 中获取并安装。

1) iCloud 安装好登录后，会在 C 盘的用户名目录下生成一个 iCloud Drive 的目录。先把这个目录“复制”到你想要移动的位置，比如把它移动到 E 盘目录下。然后把当前已经在运行的 iCloud 客户端退出去，会提示删除原来 C 盘的iCloud Drive文件夹，选择“是”。2) 接下来请在系统中，找到“命令提示符”应用，请右键单击，选择“以管理员方式运行”。3) 随后在提示符中输入：`MKLINK /D C:\Users\Quebradawill\iCloudDrive E:\Cloud\iCloudDrive`，当命令执行成功以后，提示链接成功，就这样把 iCloud Drive 从 C 盘移到了 E 盘中。

### 2. 英文版 Win 10 记事本等程序中显示中文乱码

Control Panel $$\to$$ Region $$\to$$ Administrative $$\to$$ Change system locale... $$\to$$ Current system locale 中选择 Chinese (Simplified China)，不勾选 Beta: Use Unicode UTF-8 for worldwide language support

### 3. Win 10 系统不显示 U 盘的解决方法

1）按下 Win+i 进入到 Win 10 下的“设置界面”，点击“设备”；<br>2）点击已连接设备，然后电脑的外接设备就会出现在左侧的列表里面，可以看见，我们连接的 U 盘出现了；<br>3）点击 U 盘就出现了“删除设备”的点击项，点击“删除设备”；<br>4）最后拔掉U盘，再插上 U 盘就会出现在电脑中即可正常打开！

### 4.