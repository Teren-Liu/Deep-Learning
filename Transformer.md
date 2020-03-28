<!-- GFM-TOC -->
* [概述](#概述)
* [预备知识](#预备知识)
* * [Attention](#Attention)
* * [Self-Attention](#Self-Attention)
* * [Multi-Head-Attention](#Multi-Head-Attention)
* * [Position-Embedding](#Position-Embedding)
* * [Mask](#Mask)
* [模型结构](#模型结构)
* * [Encoder](#Encoder)
* * [Decoder](#Decoder)
* * [Output](#Output)
* [参考文献](#参考文献)
<!-- GFM-TOC -->

## 概述

Transformer最初的应用就是机器翻译，可以将其想象成黑盒，输入一种语言将它翻译成其它语言。

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-1.png"/></div></center>

具体来说，模型分作Encoder和Decoder两个部分，每个部分中有6个子模型（按照论文设置）。

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-2.png"/></div></center>

## 预备知识

在进一步了解Transformer内部构成前，为了更好地理解，需要了解以下预备知识。

##### Attention

在《Attention iss All You Need》中，Google详细定义了最早出现在Seq2Seq模型中的Attention机制。

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-3.png"/></div></center>

对应到Seq2Seq模型中，Q为Decoder每个时刻节点输出的集合矩阵，K和V为Encoder每个时刻节点的输出的集合矩阵。由于基于RNN的Seq2Seq模型是具有时序性的，矩阵Q理论上是无法在输入的时候就获得的。但若把此Attention公式的Q矩阵拆解，便是Seq2Seq中Attention的定义

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-4.png"/></div></center>


其中qt为decoder在t时刻的输出，K,V均为Encoder输出矩阵。

直观上来说，Attention机制可视为以Q矩阵与K矩阵相关度为权重的V矩阵加权输出。最终得到的输出维度为(Q的数量,V的特征数量)。

##### Self-Attention

所谓Self-Attention，实际上是

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-5.png"/></div></center>


目的是为了学习特征矩阵内部的关系

##### Multi-Head-Attention

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-6.png"/></div></center>


多头注意力机制，即通过参数矩阵映射后进行h个Attention，再拼接起来，试图从多个角度捕获Q与K-V直接的关系。

具体实现上，如下公式

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-7.png"/></div></center>

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-8.png"/></div></center>


##### Position-Embedding

根据Attention定义，如果将K,V打乱顺序，Attention结果还是一样的，本质上说，这样的Attention机制只是一个巧妙的词袋模型。

为记录QKV的序列信息，Google提出了位置向量的定义，本质上类似于给每个向量编号，但Google提出的位置向量具备运算功能，即位置向量的运算是有意义的。公式为：

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-9.png"/></div></center>

这里的意思是将id为p的位置映射为一个dpos维的位置向量，这个向量的第i个元素的数值就是PEi(p)。举个例子，当p=0，dpos=4时

<center>PE(0) = [PE0(0),PE1(0),PE2(0),PE3(0)]</center>

##### Mask

mask是一般为一个向量，用于掩盖输入特征中需要被隐藏的信息。例如一个经过padding的输入特征。

<center>x=[1,0,3,4,5,0,0,0]</center>

此时我们用这样一个mask

<center>m=[1,1,1,1,1,0,0,0]</center>

## 模型结构

在具备以上知识后，便可以介绍模型结构

##### Encoder

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-9.png"/></div></center>

Encoder的输入向量与位置向量进行按位求和，输入到Self-Attention中，经过正则化后输入到一个全连接网络，再输入到第二个Encoder。

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-1.gif"/></div></center>

最终得到Context特征矩阵，该向量也是与decoder交互的Attention的K和V矩阵。

##### Decoder

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-10.png"/></div></center>

Decoder的输出特征是有时序的，即每一时刻仅输出一个单词的预测结果，但在训练的过程中，Decoder的输入仍然是正确的翻译结果，为了不让decoder预见到未来的信息，此处会用到一个Masked。举个例子

假设Encoder的输入为

<center>X_input = [我,爱,中国]</center>

t=0的时候，Decoder的输入为

<center>X_output = [I,love,China]</center>
<center>mask = [0,0,0]</center>

t=1的时候，Decoder的输入为

<center>X_output = [I,love,China]</center>
<center>mask = [1,0,0]</center>

在实现的过程中这里会用到一个大型的Mask矩阵，形状类似下图

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-11.png"/></div></center>

在经过Masked-Self-Attention后，与Encoder的上下文矩阵进行Mult-Attention，其中Q为t时刻的Decoder输出向量，K和V都为Encoder的上下文矩阵，最后正则化后输入到全连接网络再输入到输出层。

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-2.gif"/></div></center>

##### Output

<center><div align=center><img src ="https://github.com/Teren-Liu/Deep-Learning/blob/master/image/Trans-12.png"/></div></center>

输出层中，输入为Decoder的向量，最终经过Softmax层后映射到维度为词典长度的向量中，向量中的每个值对应的是该词的概率。

这便是Transformer的总体流程


## 参考文献

- 《Attention is All You Need》浅读（简介+代码） https://kexue.fm/archives/4765
- https://jalammar.github.io/illustrated-transformer/
