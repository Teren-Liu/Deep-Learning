<!-- GFM-TOC -->
* [Seq2Seq](#Seq2Seq)
* [Seq2Seq-Attention](#Seq2Seq-Attention)
* [参考文献](#参考文献)
<!-- GFM-TOC -->

## Seq2Seq

Seq2Seq是一种Encoder-Decoder框架，解码和编码器是RNN，主要应用于机器翻译任务中。

举个例子，先需要将ABCD翻译为XYZ，Seq2Seq的思想为利用一个RNN读取输入句子ABCD，将整个句子的信息压缩到一个固定维度，再利用另一个RNN读取这个编码，解压为XYZ。

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-1.png"/></div></center>

#### Encoder

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-2.png"/></div></center>

Encoder利用RNN将输入数据编码成上下文向量c,c有两种形式。

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-f1.png"/></div></center>

#### Decoder

解码过程中，亦有两种方式

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-3.png"/></div></center>

上图中，每个节点的输入为上一个节点的输出，即

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-f2.png"/></div></center>

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-4.png"/></div></center>

上图中，每个节点的输入都为上下文向量C，即

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-f3.png"/></div></center>


## Seq2Seq-Attention

由于encoder-decoder模型在编码和解码阶段始终由一个不变的语义向量C来联系着，编码器要将整个序列的信息压缩进一个固定长度的向量中去。这就造成了一些问题，在长序列上尤为明显。
- 语义向量无法完全表示整个序列的信息
- 最开始输入的序列容易被后输入的序列给覆盖掉。

### Attention

#### 简述

相比于之前的encoder-decoder模型，attention模型最大的区别就在于它不在要求编码器将所有输入信息都编码进一个固定长度的向量之中。相反，解码器每个节点的输入是编码器对应输出的加权求和。

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-f4.png"/></div></center>

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-5.png"/></div></center>

#### 计算方法

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-6.png"/></div></center>

h(i)表示编码器在第i个单词上的输出，s(j)是解码器在预测第j个单词的状态，e(h,s)为相关度函数，最常见的定义是一个带有单个隐藏层的前馈神经网络，也有直接点乘的方法，α(i,j)以及context(i)计算方法为:

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Seq2Seq-f5.png"/></div></center>

由公式可以发现，在获得第j个状态后，注意力机制计算该状态与编码器所有输出的相关度，相关度越高的得到的context包含该部分输出信息最多。

## 参考文献

- https://www.cnblogs.com/DLlearning/p/7834018.html
- Tensorflow 实战 Google深度学习框架