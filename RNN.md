<!-- GFM-TOC -->
* [循环神经网络RNN](#循环神经网络RNN)
* [长短时记忆网络LSTM](#长短时记忆网络LSTM)
* [参考文献](#参考文献)
<!-- GFM-TOC -->

## 循环神经网络RNN

### 简述

循环神经网络具备记忆序列中信息的能力，并利用记忆的信息影响后面节点的输出。它主要的用途是处理和预测序列数据，被广泛应用于语音识别，语言模型，机器翻译以及时序分析等问题中。

### 网络结构

#### 图示

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/RNN-1.png"/></div></center>

上图展示了一个典型的循环神经网络，在每一个时刻t，RNN会根据该时刻的输入结合当前模型的状态给出一个输出，并更新模型状态。<br>
每个时刻模型的输入，对于语言模型，可能是某个单词的向量，对于时间序列数据，可能是该个时刻的销量。

#### 公式

- 输入特征向量X(i)维度为x
- 状态特征向量S(i)维度为n
- 输出特征向量O(i)维度为x
- 输入层权重矩阵U维度为(n+x) x n
- 输入层偏置矩阵B维度为1 x n
- 输出层权重矩阵V为n x x

S(i)状态更新公式

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/RNN-f1.png"/></div></center>

O(i)输出公式

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/RNN-f2.png"/></div></center>

下图展示了输入特征维度为1，状态特征维度为2的RNN的详细计算过程

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/RNN-2.png"/></div></center>

## 长短时记忆网络LSTM

**RNN 存在的问题**

RNN受限于短期记忆问题。如果一个序列足够长，那它们很难把信息从较早的时间步传输到后面的时间步。因此，如果你尝试处理一段文本来进行预测，RNN可能在开始时就会遗漏重要信息。

在反向传播过程中，RNN中存在梯度消失问题。梯度是用于更新神经网络权重的值，梯度消失问题是指随着时间推移，梯度在传播时会下降，如果梯度值变得非常小，则不会继续学习。

#### 简述

为了让神经网络具备选择性保留或遗忘某些信息的能力，引入单元状态和门结构。整体的计算细节如下图所示。

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/RNN-3.png"/></div></center>


##### 遗忘门

遗忘门的作用是让网络忘记没有用的信息。

遗忘门通过前一个输出状态h(t-1)和当前输入特征x(t)得到遗忘门向量。该向量用于判断过去的状态信息中哪些需要被遗忘。

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/RNN-f3.png"/></div></center>

![image](https://pic1.zhimg.com/v2-b9845aa5c463cb7aeb826a42845f2370_b.webp)

##### 输入门

在忘记部分信息后，需要从当前输入补充新的信息。

输入门通过前一个输出状态h(t-1)和当前输入特征x(t)得到输入门向量。该向量用于判断输入的状态信息中哪些需要被保留

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/RNN-f4.png"/></div></center>

![image](https://pic2.zhimg.com/v2-1167da9a6386278cda018b3da01f006d_b.webp)

##### 单元状态

在计算得到输入门和遗忘门后，可获取目前节点的单元状态C(t)

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/RNN-f5.png"/></div></center>

![image](https://pic4.zhimg.com/v2-726bf910e01dea258ccd6bc7ad96e5fb_b.webp)

##### 输出门

在计算得到新的状态后需要产生当前时刻的输出。

输出门会根据上一时刻的输出h(t-1)和当前的输入x(t)来决定输出门向量。该向量用于判断状态哪些信息需要表达。

结合当前单元状态c(t)以及输出门向量得到此时的输出向量h(t)

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/RNN-f6.png"/></div></center>

![image](https://pic4.zhimg.com/v2-4bd9fc13ff4cc2f65473a35bd1112933_b.webp)


## 参考文献
- 一文搞懂RNN https://zhuanlan.zhihu.com/p/30844905
- 图解LSTM和GRU https://zhuanlan.zhihu.com/p/46981722