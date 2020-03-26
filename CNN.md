<!-- GFM-TOC -->
* [概述](#概述)
* [卷积层](#卷积层)
* [池化层](#池化层)
* [参考文献](#参考文献)
<!-- GFM-TOC -->

## 概述

卷积神经网络最早是在图像任务上提出的神经网络，后被应用在其他领域。该网络一般以三维数据作为输入，在提取三维数据局部特征上有显著效果。网络一般可以分为以下几层，其中卷积层和池化层可以多次出现。

<center><img width = '300' height ='200' src ="https://img-blog.csdn.net/20160404000501632"/></center>

- 输入层<br>
此层为神经网络的输入。在处理图像的CNN中，长和宽为一幅图像的大小，深度代表图像的色彩通道。
- 卷积层<br>
卷积层每一个节点的输入是上一层神经网络中的一小块，试图获取输入数据的局部特征。
- 池化层<br>
不会改变输入矩阵的深度，但会缩小三维矩阵的大小。
- 全连接层<br>
卷积层和池化层可视作特征提取的过程，此处利用1-2个全连接层进行分类。
- Softmax层<br>
得到样例属于不同种类的概率分布情况。

### 卷积层

CNN2

<center><img width = '300' height ='200' src ="https://img-blog.csdn.net/20160404000501632"/></center>

#### 简述

如图所示，卷积层中一个Filter会将输入层某个局部特征矩阵转化为下一层神经网络上的单位节点矩阵。Filter的长和宽由人工设定，单位节点矩阵的维度为[1x1xn],n为单位节点矩阵的深度，也由人工设定。

#### 前向传播计算过程

##### 单位节点矩阵的计算 

###### 输入
- 2x2x3 局部特征矩阵A
- 节点矩阵深度d
- d 个 2x2x3 过滤器权重矩阵W

###### 输出
- 1x1xd 的单位节点矩阵M

###### 计算公式

对于单位节点矩阵中第i个点，有


```math
M(i) = f(\sum_{x=1}^2\sum_{y=1}^2\sum_{z=1}^3A_{xyz} \times W_{xyz}^i + b^i)
```

其中f()为激活函数，对于该单位节点矩阵用d个Filter对其进行计算，最终得到1x1xd的单位节点矩阵，该过程中，需要训练(2x2x3xd)个参数。下图为M(0)的计算示意图

<center><img width = '300' height ='200' src ="https://img-blog.csdn.net/20160404000501632"/></center>

##### Filter的滑动

###### 简述

Filter会在输入矩阵中根据设定的步长进行滑动，从输入矩阵的左上角自左到右自上到下移动到右下角，以得到输出特征矩阵。

<center><img width = '300' height ='200' src ="https://img-blog.csdn.net/20160404000501632"/></center>

###### Padding

使用Padding即在输入矩阵边缘补0，补0的上限是输出矩阵与输入矩阵维度完全一致，Padding的目的主要有以下几点

- 为了不丢弃原图信息
- 为了让更深层的layer的input依旧保持有足够大的信息量
- 为了实现上述目的，且不做多余的事情，padding出来的pixel的值都是0，不存在噪音问题


<center><img width = '300' height ='200' src ="https://img-blog.csdn.net/20160404000501632"/></center>

###### 输出矩阵

在不进行Padding的情况下，下列公式给出了结果矩阵的大小


```math
out_{length} = [(in_{length}-filter_{length} + 1 / stride_{length}]

out_{width} = [(in_{width}-filter_{width} + 1 / stride_{width}]
```

## 池化层

池化层可以有效地缩小矩阵尺寸，减少最后全连接层的参数，防止过拟合。池化层主要有最大池化层(Max Pooling)与平均池化层(Average Pooling)两种。池化的方法与卷积的步骤类似，池化层中没有参数。

![image](314A0EBA6EBE452D8D1CED9466A9A023)



## 参考文献
- 一句话CNN：如何理解padding的作用和算法 https://zhuanlan.zhihu.com/p/36278093
- Tensorflow 实战 Google深度学习框架
