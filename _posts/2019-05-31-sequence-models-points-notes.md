---
layout: post
title: "some notes for sequence model"
date: 2019-05-27
category: 散记
tags: 
    - sequence model
    - RNN
    - LSTM
---

##sequenc model##
input x or output y are sequence which could have different length. These problems regards as supervised learning which use label data (x,y) as training set.

Notation:
![Notation](\assets\images\postsimage\0531\sequence_model.jpg)

##RNN##

RNN vs. Standard NN
Standard network has problems:
	- inputs, outputs can be different lengths in different examples，padding不是好的表达方式
	- doesn't share features learned across different positions of text

Standard NN输入如果是10,000维的one-hot向量，如果总的输入大小是最大单词数乘以10,000，那么第一层的权重矩阵就会有巨量的参数。循环神经网络就没有这两个问题。
普通RNN的一个限制是它在某一个时刻的预测仅使用了从序列之前的输入信息并没有使用序列中后部的信息，所以有双向循环神经网络BRNN来解决。

![RNN Unit](\assets\images\postsimage\0531\sequence_model.png)

##GRU##

gated recurrent unit

通过门决定这个时间点上要不要更新某个记忆细胞。比RNN更好捕捉长范围的依赖。

##LSTM##

<https://colah.github.io/posts/2015-08-Understanding-LSTMs/>

##Embedding Matrix##

![embedding matrix](\assets\images\postsimage\0531\embedding_matrix.jpg)

矩阵E包含了词汇表中所有单词的嵌入向量。实际比如Keras有嵌入层，不用对矩阵进行很慢很复杂的乘法运算就可以提取出需要的列。