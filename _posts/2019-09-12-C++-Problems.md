---
layout: post
title: C++ 问题汇总
date: 2019-09-12
categories: Programming
tags: C++
status: publish
type: post
published: true
author: Quebradawill
---

#### 1. VS 2019 支持 C++ 17 标准

Debug $$\to$$ Properties... $$\to$$ C/C++ $$\to$$ Language $$\to$$ C++ Language Standard

#### 2. C++ 删除字符串中某字符

```C++
string str = ",hello,";
str.erase(remove(str.begin(), str.end(), ','), str.end());    // hello
```

