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
from torchvision import models
from torchsummary import summary

# cannot recognize arg scale
# model = models.resnet50(pretrained=False, num_classes=7, scale=1)
model = models.resnet50(pretrained=False, num_classes=7)
print(model)
summary(model, (3, 640, 640))
```

参考：[使用torchsummary打印torch模型结构，包括每层名字以及形状](https://www.cnblogs.com/ywheunji/p/12368667.html)

### 2. PyTorch 快速加载预训练模型参数的方式

**1）直接修改源码，改为本地地址**

通过查找自己代码里所调用网络的类，使用 PyCharm 自带的函数查找功能（Ctrl+鼠标左键），查看此网络的加载方法，修改 `model.load_state_dict()` 函数。

例如：已经下载好的 resnet50 的参数文件：放在 model_urls 指定的文件夹里，这样就可以提前下载直接使用。

```python
model_urls = {'resnet50': '/home/huihua/NewDisk1/pretrain_parameter/resnet50-19c8e357.pth'}

def resnet50(pretrained=False, **kwargs):
    """
    Constructs a ResNet-50 model.
    Args:
        pretrained (bool): If True, returns a model pretrained on ImageNet.
    """
    # define neural network model
    model = ResNet(Bottleneck, [3,4,6,3], **kwargs)
    if pretrained:
        # load parameters from local folder
        model.load_state_dict(model_zoo.load_url(model_urls['resnet50']))
    return model
```
下面的代码是从官网加载预训练好的模型，也就是默认的方式：

```python
import torchvision.models as models
 
model = models.vgg16(pretrained=True)
print(model)
```

也可以按如下方式定义和加载本地 .pth 文件：

```python
import torchvision.models as models

# define neural network model
model = models.vgg16(pretrained=False)
# load parameters
pre = torch.load(r'D:/Python_models/pytorch_models/vgg16-397923af.pth')
# load parameters to model
model.load_state_dict(pre)
```

**2）把模型权重下载至 torch 的缓存文件夹**

由于 torch 在加载模型时首先检查本地缓存是否已经存在该模型，所以在本地用户目录（Windows 系统：C:\Users\dwqiu\\.cache\torch\checkpoints）下，预先下载模型参数文件 .pth，存放在该缓存文件夹里，可快速加载该模型。相比第一种方法简单点，不过需占用 C 盘空间。

参考：

[pytorch预训练模型的下载地址以及解决下载速度慢的方法](https://www.cnblogs.com/ywheunji/p/10605614.html)<br>[pytorch从本地加载 .pth 格式模型](https://blog.csdn.net/TomorrowAndTuture/article/details/100219240)

### 3. PyTorch 保存和加载模型

当提到保存和加载模型时，有三个核心功能需要熟悉：<br>
 1、[torch.save](https://pytorch.org/docs/stable/torch.html?highlight=save#torch.save)：将序列化的对象保存到磁盘。这个函数使用 Python 的 pickle 实用程序进行序列化。使用这个函数可以保存各种对象的模型、张量和字典；<br>
 2、[torch.load](https://pytorch.org/docs/stable/torch.html?highlight=torch load#torch.load)：使用 pickle 的 unpickle 工具将 pickle 的对象文件反序列化到内存；<br>
 3、[torch.nn.Module.load_state_dict](https://pytorch.org/docs/stable/nn.html?highlight=load_state_dict#torch.nn.Module.load_state_dict)：把反序列化状态的字典加载到 model 的参数字典中。

**1）什么是 state_dict**

在 PyTorch 中，`torch.nn.Module` 的可学习参数（即权重和偏差），模块模型包含在 model 的参数中（通过 `model.parameters()` 访问）。`state_dict` 是个简单的 Python dictionary 对象，它将每个层映射到它的参数张量。

注意，只有具有可学习参数的层（卷积层、线性层等）才有 model 的 `state_dict` 中的条目。优化器对象（`connector.optim`）也有一个 `state_dict`，其中包含关于优化器状态以及所使用的超参数的信息。

```python
import torch
import torch.nn as nn
import torch.nn.functional as F


# Define model
class TheModelClass(nn.Module):
    def __init__(self):
        super(TheModelClass, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def farward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = x.view(-1, 16 * 5 * 5)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x


# Initialize model
model = TheModelClass()
# Initialize optimizer
optimizer = torch.optim.SGD(model.parameters(), lr=1e-4, momentum=0.9)

print("Model's state_dict:")
# Print model's state_dict
for param_tensor in model.state_dict():
    print(param_tensor, "\t", model.state_dict()[param_tensor].size())
print("optimizer's state_dict:")
# Print optimizer's state_dict
for var_name in optimizer.state_dict():
    print(var_name, "\t", optimizer.state_dict()[var_name])
```

输出为：

```python
Model's state_dict:
conv1.weight 	 torch.Size([6, 3, 5, 5])
conv1.bias 	 torch.Size([6])
conv2.weight 	 torch.Size([16, 6, 5, 5])
conv2.bias 	 torch.Size([16])
fc1.weight 	 torch.Size([120, 400])
fc1.bias 	 torch.Size([120])
fc2.weight 	 torch.Size([84, 120])
fc2.bias 	 torch.Size([84])
fc3.weight 	 torch.Size([10, 84])
fc3.bias 	 torch.Size([10])
optimizer's state_dict:
state 	 {}
param_groups [{'lr': 0.0001, 'momentum': 0.9, 'dampening': 0, 'weight_decay': 0,
               'nesterov': False, 'params': [2524756933672, 2524756933752, 2524756933832,
                                             2524756933912, 2524756933992, 2524756934072,
                                             2524756934152, 2524756934232, 2524756934312,
                                             2524756934392]}]
```

**2）保存和加载模型**

（1）保存/加载 state_dict（推荐）

```python
torch.save(model.state_dict(), PATH)
```

在保存模型进行推理时，只需要保存训练过的模型的学习参数即可。一个常见的 PyTorch 约定是使用 .pt 或 .pth 文件扩展名保存模型。

```python
model = TheModelClass(*args, **kwargs)
model.load_state_dict(torch.load(PATH))
model.eval()
```

<font color='red'>记住：</font>f您必须调用 `model.eval()`，以便在运行推断之前将 dropout 和 batch 规范化层设置为评估模式。如果不这样做，将会产生不一致的推断结果。

**注意：**`load_state_dict()` 函数接受一个 dictionary 对象，而不是保存对象的路径。这意味着您必须在将保存的 state_dict 传至 load_state_dict() 函数之前反序列化它。

（2）保存/加载整个模型

```python
torch.save(model, PATH)

# Model class must be defined somewhere
model = torch.load(PATH)
model.eval()
```

（3）保存/加载更多信息

在实验中往往需要保存更多信息，比如优化器的参数，那么可以采取下面的方法保存：

```python
torch.save({'epoch': epochID + 1, 'state_dict': model.state_dict(), 'best_loss': lossMIN,
            'optimizer': optimizer.state_dict(), 'alpha': loss.alpha, 'gamma': loss.gamma},
           checkpoint_path + '/m-' + launchTimestamp + '-' + str("%.4f" % lossMIN) + '.pth.tar')
```

以上包含的信息有：epochID、state_dict、min_loss、optimizer，以及自定义损失函数的两个参数。格式以字典的格式存储。

加载方式：

```python
def load_checkpoint(model, checkpoint_PATH, optimizer):
    if checkpoint != None:
        model_CKPT = torch.load(checkpoint_PATH)
        model.load_state_dict(model_CKPT['state_dict'])
        print('loading checkpoint!')
        optimizer.load_state_dict(model_CKPT['optimizer'])
    return model, optimizer
```

其他的参数可以通过以字典的方式获得。

但是，我们可能修改了一部分网络，比如加了一些，删除一些，等等，那么需要过滤这些参数，加载方式：

```python
def load_checkpoint(model, checkpoint, optimizer, loadOptimizer):
    if checkpoint != 'No':
        print("loading checkpoint...")
        model_dict = model.state_dict()
        modelCheckpoint = torch.load(checkpoint)
        pretrained_dict = modelCheckpoint['state_dict']
        # 过滤操作
        new_dict = {k: v for k, v in pretrained_dict.items() if k in model_dict.keys()}
        model_dict.update(new_dict)
        # 打印出来，更新了多少的参数
        print('Total : {}, update: {}'.format(len(pretrained_dict), len(new_dict)))
        model.load_state_dict(model_dict)
        print("loaded finished!")
        # 如果不需要更新优化器那么设置为false
        if loadOptimizer == True:
            optimizer.load_state_dict(modelCheckpoint['optimizer'])
            print('loaded! optimizer')
        else:
            print('not loaded optimizer')
    else:
        print('No checkpoint is included')
    return model, optimizer
```

以下的方法与上述方法类似：

```python
# save
torch.save({
    'epoch': epoch,
    'model_state_dict': model.state_dict(),
    'optimizer_state_dict': optimizer.state_dict(),
    'loss': loss,
    ...
}, PATH)

# load
model = TheModelClass(*args, **kwargs)
optimizer = TheOptimizerClass(*args, **kwargs)

checkpoint = torch.load(PATH)
model.load_state_dict(checkpoint['model_state_dict'])
optimizer.load_state_dict(checkpoint['optimizer_state_dict'])
epoch = checkpoint['epoch']
loss = checkpoint['loss']

model.eval()
# - or -
model.train()
```

在保存用于推理或恢复训练的通用检查点时，必须保存模型的 `state_dict`。另外，保存优化器的 `state_dict` 也是很重要的，因为它包含缓冲区和参数，这些缓冲区和参数是在模型训练时更新的。要保存多个组件，请将它们组织在字典中，并使用 `torch.save()` 序列化字典。一个常见的 PyTorch 约定是使用 <font color='blue'>.tar</font> 文件扩展名保存这些检查点。

**3）通过设备保存/加载模型**

（1）保存到 GPU，加载到 CPU

```python
# save
torch.save(model.state_dict(), PATH)

# load
device = torch.device('cpu')
model = TheModelClass(*args, **kwargs)
model.load_state_dict(torch.load(PATH, map_location=device))
```

​	（2）保存到 GPU，加载到 GPU

```python
# save
torch.save(model.state_dict(), PATH)

# load
device = torch.device("cuda")
model = TheModelClass(*args, **kwargs)
model.load_state_dict(torch.load(PATH))
model.to(device)
# Make sure to call input = input.to(device) on any input tensors that you feed to the model
```

（3）保存到 CPU，加载到 GPU

```python
# load
torch.save(model.state_dict(), PATH)

# load
device = torch.device("cuda")
model = TheModelClass(*args, **kwargs)
# Choose whatever GPU device number you want
model.load_state_dict(torch.load(PATH, map_location="cuda:0"))
model.to(device)
# Make sure to call input = input.to(device) on any input tensors that you feed to the model
```

（4）保存/加载 `torch.nn.DataParallel` 模型

```python
# save
torch.save(model.module.state_dict(), PATH)

# load
# Load to whatever device you want
```

`torch.nn.DataParallel` 是支持并行 GPU 使用的模型包装器。为了节省 DataParallel 模型属性，保存 `model.module.state_dict()`。通过这种方式，您可以灵活地以任何方式加载模型以加载任何设备。

**4）冻结部分参数，训练另一部分参数**

（1）在模型中添加下面的代码

```python
for p in self.parameters():
    p.requires_grad = False
```

比如加载了 resnet 预训练模型之后，在 resenet 的基础上连接了新的模快，resenet 模块那部分可以先暂时冻结不更新，只更新其他部分的参数，那么可以在下面加入上面代码：

```python
class RESNET_MF(nn.Module):
    def __init__(self, model, pretrained):
        super(RESNET_MF, self).__init__()
        self.resnet = model(pretrained)
        for p in self.parameters():
            p.requires_grad = False
        self.f = SpectralNorm(nn.Conv2d(2048, 512, 1))
        self.g = SpectralNorm(nn.Conv2d(2048, 512, 1))
        self.h = SpectralNorm(nn.Conv2d(2048, 2048, 1))
        ...
```

同时在优化器中添加：`filter(lambda p: p.requires_grad, model.parameters())`

```python
optimizer = optim.Adam(filter(lambda p: p.requires_grad, model.parameters()), lr=0.001,
                       betas=(0.9, 0.999), eps=1e-08, weight_decay=1e-5)
```

（2）参数保存在有序的字典中，那么可以通过查找参数的名字对应的 id 值，进行冻结

查找代码：

```python
model_dict = torch.load('net.pth.tar').state_dict()
dict_name = list(model_dict)
for i, p in enumerate(dict_name):
    print(i, p)
```

保存一下这个文件，可以看到大致是这个样子的：

```python
0 gamma
1 resnet.conv1.weight
2 resnet.bn1.weight
3 resnet.bn1.bias
4 resnet.bn1.running_mean
5 resnet.bn1.running_var
6 resnet.layer1.0.conv1.weight
7 resnet.layer1.0.bn1.weight
8 resnet.layer1.0.bn1.bias
9 resnet.layer1.0.bn1.running_mean
....
```

同样在模型中添加这样的代码：

```python
for i,p in enumerate(net.parameters()):
    if i < 165:
        p.requires_grad = False
```

在优化器中添加上面的代码可以实现参数的屏蔽。

参考：

[PyTorch之保存加载模型](https://www.jianshu.com/p/4905bf8e06e5)<br>[Pytorch 保存模型与加载模型](https://zhuanlan.zhihu.com/p/38056115)