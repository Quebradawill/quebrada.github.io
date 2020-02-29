---
layout: post
title: PyTorch 使用问题汇总
date: 2020-02-29
categories: Research
tags: Python PyTorch
status: publish
type: post
published: true
author: Quebradawill
---

### 1. 使用 torchsummary 打印 torch 模型结构

使用 torchsummary 模块，首先需要安装：`pip install torchsummary`

```python
import models
from torchsummary import summary

model = models.resnet50(pretrained=False, num_classes=7, scale=1)
print(model)
summary(model, (3, 640, 640))
```

参考：[使用torchsummary打印torch模型结构，包括每层名字以及形状](https://www.cnblogs.com/ywheunji/p/12368667.html)

### 2. PyTorch 快速加载预训练模型参数的方式

**1）直接修改源码，改为本地地址**

通过查找自己代码里所调用网络的类，使用 PyCharm 自带的函数查找功能（Ctrl+鼠标左键），查看此网络的加载方法，修改 `model.load_state_dict()` 函数。

例如：已经下载好的 resnet50 的参数文件：放在 model_urls 里面，这样就可以提前下载直接使用。

```python
model_urls = {'resnet50': '/home/huihua/NewDisk1/pretrain_parameter/resnet50-19c8e357.pth'}

def resnet50(pretrained=False, **kwargs):
    ```
    Constructs a ResNet-50 model.
    Args:
        pretrained (bool): If True, returns a model pretrained on ImageNet.
    ```
    model = ResNet(Bottleneck, [3,4,6,3], **kwargs)
    if pretrained:
        model.load_state_dict(model_zoo.load_url(model_urls['resnet50']))
    return model
```

从官网加载预训练好的模型：

```python
import torchvision.models as models
 
model = models.vgg16(pretrained=True)
print(model)
```

也可以按如下方式定义和加载本地 .pth 文件：

```python
import torchvision.models as models
 
model = models.vgg16(pretrained=False)
pre=torch.load(r'D:/Python_models/pytorch_models/vgg16-397923af.pth')
model.load_state_dict(pre)
```

**2）把模型权重下载至 torch 的缓存文件夹**

由于 torch 在加载模型时候首先检查本地缓存	是否已经存在模型，所以在本用户目录下，预先下载放入可快速加载模型。相比第一种方法简单点。

参考：

[pytorch预训练模型的下载地址以及解决下载速度慢的方法](https://www.cnblogs.com/ywheunji/p/10605614.html)<br>[pytorch从本地加载 .pth 格式模型](https://blog.csdn.net/TomorrowAndTuture/article/details/100219240)