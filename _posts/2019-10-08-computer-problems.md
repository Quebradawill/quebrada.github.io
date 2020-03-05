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

### 4. 解决github下载速度变慢问题

**1）方法一**

主要修改的 hosts（C:\Windows\System32\drivers\etc\hosts）地址为：github.com 和 github.global.ssl.fastly.net 。查看网站对应的 IP 地址的方法为访问 ipaddress 网站，输入网址则可查阅到对应的IP地址。ipaddress 地址：https://www.ipaddress.com/

修改完 hosts 还不会立即生效，你需要刷新 DNS 缓存，告诉电脑我的 hosts 文件已经修改了。

输入指令：`sudo /etc/init.d/networking restart` 即可。（此处这个指令我用了不行，用的是 `sudo killall -HUP mDNSResponder`）

然后，你关闭浏览器再访问 github 就不会出现速度很慢的现象了。（亲测不关闭浏览器直接访问也可）

Windows下刷新 DNS 的方法：打开 CMD，输入 `ipconfig /flushdns`

方法一一般不太管用（亲测），可以试一下如下的方法。

**2）方法二**

（1）打开码云（当然不是福报）https://gitee.com/ 并注册登录；<br>（2）创建仓库；<br>（3）在新建仓库页选择 “导入已有仓库”；<br>（4）复制你需要下载的git链接，如：https://github.com/quebrada/fastai 放到导入已有仓库中；<br>（5）点击创建，然后下载。