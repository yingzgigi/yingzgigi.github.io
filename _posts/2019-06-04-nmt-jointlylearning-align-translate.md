---
layout: post
title: "NMT联合align和translate即attention由来"
date: 2019-06-04
category: 论文notes
mathjax: true
tags:
    - NMT
    - attention mechanism
    - BiRNN
    - align
---

point：提出注意力机制，attention mechanism的开篇之作。

## [NEURAL MACHINE TRANSLATION BY JOINTLY LEARNING TO ALIGN AND TRANSLATE] 2015 Bahdanau et al. ##

NMT旨在建立一个可以同时调整并最大化翻译性能的神经网络。一般用的encoder-decoder模型把不同长度的源语言句子encode后得到固定长度的向量，这样对NMT来说不能提高performance。所以用一个能自动对与预测某个target word相关的部分语句soft search的模型，而不用form这部分as a hard segement explicitly（就是硬性细分）。也就是soft alignment。

joint translation model联合翻译模型只用local sentence来预测。discriminatice word lexica：优点global context；缺点no position information。

### learning to align and translate ###

#### decoder部分 ####

论文提到的模型的条件概率是这样的：

$$ p(y_i \mid y_1,...,y_{i-1},x) = g(y_{i-1},s_i,c_i) $$

$s_i$是RNN的hidden state，这里的$c_i$是对于每个target word的context vector，是不同的（一般的RNN模型使用相同的c）。这个context vector来自encoder部分的hidden state序列(sequence of annotations)。它是对annotation的h序列的权重和。每个annotation的weights其实是energe的概率。energy又是由alignment model计算对decoder的hidden state和输入序列的annotation h的评分得到的。alignment model就是图里的a，说明j位置附近的输入还有i位置附近的输出的匹配程度。

他们把alignment模型参数化成跟系统里其他部分联合训练的FFNN。alignment模型直接计算soft alignment，并让代价函数的gradient反向传播，从而联合训练alignment model和translation model。probability $\alpha_{ij}$和相关联的energy $e_{ij}$反应的是，annotation $h_j$根据隐藏状态$s_{i-1}$对决定$s_i$和输出$y_i$影响多大，即在翻译输出第i个目标词时第j个源语言词的影响力。这是最早实现在decoder中的attention机制方法，这样decoder可以决定注意力放在哪部分源句上（其实是放多少问题，哪部分值得注意。）用注意力机制可以不用把所有的源语句里的信息都encode成一个固定长度的向量，由decoder选择性地检索哪些信息应该在注释序列中相关联。

![proposed model](/assets/images/postsimage/0604/birnn_model.jpg)

#### encoder部分 ####

上图的模型的encoder部分使用了BiRNN，双向是因为对每个词的annotation不仅加上了前面的所有词forward的hidden states，也加上了后面的词backward的hidden states。

### 实验结果 ###

![four sample alignments](/assets/images/postsimage/0604/sample_alignments.jpg)

从图d可以看出，attention机制对一个源词有不同翻译结果时选择哪一个更好非常有帮助，通过关注the和man两个词来解决the应该翻译成法语里的le还是la还是les还是l'，最后根据the后面的词man决定翻译成l'。

并且soft alignment能处理源语句和目标语句不同长度的问题，不用counter-intuitive的那种把词映射到[NULL]或者一个词映射自[NULL]。