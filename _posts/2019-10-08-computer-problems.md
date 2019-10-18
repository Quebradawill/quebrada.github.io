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

1) iCloud 安装好以后，会在 C 盘的用户名目录下生成一个 iCloud Drive 的目录。先把这个目录“剪切”到你想要移动的位置，比如把它移动到 E 盘目录下。在操作之前，请把当前已经在运行的 iCloud 客户端退出去，否则会导致移动目录不成功。2) 接下来请在系统中，找到“命令提示符”应用，请右键单击，选择“以管理员方式运行”。3) 随后在提示符中输入：`MKLINK /D C:\Users\Quebradawill\iCloudDrive E:\Cloud\iCloudDrive`，当命令执行成功以后，提示链接成功，就这样把 iCloud Drive 从 C 盘移到了 E 盘中。

### 2. 英文版 Win 10 记事本等程序中显示中文乱码

Control Panel $$\to$$ Region $$\to$$ Administrative $$\to$$ Change system locale... $$\to$$ Current system locale 中选择 Chinese (Simplified China)，不勾选 Beta: Use Unicode UTF-8 for worldwide language support