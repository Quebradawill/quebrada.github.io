---
layout: post
title: GitHub + Jekyll 搭建 blog
date: 2019-09-06
categories: Programming
tags: Blog
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  views: '2'
author:
  login: Quebradawill
  email: dwqiu@foxmail.com
  display_name: Quebradawill
---

### 申请账号

1. 申请一个 GitHub 账号；
2. 创建一个新仓库（Repositories），注意勾选上 `Initialize this repository with a README`；
3. 给自己的仓库（Repositories）命名，**注意：**名称格式为：username.github.io（username 是你 GitHub 用户名，最好不要用别的），username.github.io 将成为网站的网址；
4. 进入到仓库界面，点击 settings 往下拉，找到 Github pages 设置界面，在 `source` 选项里选择 `master branch`，然后点击 save 保存。也可以在 `Theme Chooser` 里选择一个网页主题，这时候你的一个简单的网站已经搭建好了。对，你没听错，就这么简单！在 `Source` 上面出现了 `Your site is published at thhps://username.github.io/` 字样，这就是你自己网站的网址了。

### 主题美化

1. 推荐从 GitHub 上下载 Jekyll 类型的模板，便于使用 Jekyll Writer 管理博文。下载后解压出网页源代码，并找到文件名为 `_config.yml` 的文件，在这个文件里面你可以修改网页的名称，要展示的内容等等；
2. 然后就是把网页源代码上传到 GitHub 上刚创建的仓库里了，这里有两种方案。一种是通过 GitHub Desktop 上传，一种是通过 GitHub 网页端上传。这里介绍从 GitHub Desktop 上传；
3. 打开 GitHub Desktop，点击 `file`，选择 `Clone repository`，选择 username.github.io；
4. 转到 GitHub Desktop 界面，这时候它已经检测到了本地文件的变换，在左下角输入这次改动的摘要就可以上传了，点击 `Commit to master`，再点击 `Fetch origin` 完成上传。

参考：[个人网站的搭建（基于 GitHub 和 Jekyll 主题）](https://blog.csdn.net/qq_19799765/article/details/80869363)