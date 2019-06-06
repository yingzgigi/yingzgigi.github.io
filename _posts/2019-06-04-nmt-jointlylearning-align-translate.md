---
layout: post
title: "NMT对encoder-decoder的扩展：align和translate"
date: 2019-05-27
category: 论文notes
tags: 
    - NMT
---

## [NEURAL MACHINE TRANSLATION BY JOINTLY LEARNING TO ALIGN AND TRANSLATE] 2015 Bahdanau et al. ##

NMT旨在建立一个可以同时调整并最大化翻译性能的神经网络。一般用的encoder-decoder模型把不同长度的源语言句子encode后得到固定长度的向量，这样对NMT来说不能提高performance。所以用一个能自动对与预测某个target word相关的部分语句soft search的模型，而不用form这部分as a hard segement explicitly（就是硬性细分）。也就是soft alignment。

joint translation model联合翻译模型只用local sentence来预测。discriminatice word lexica：优点global context；缺点no position information。

### learning to align and translate ###

#### decoder部分 ####

论文提到的模型的条件概率是这样的：

$$ p(y_i \mid y_1,...,y_{i-1},x) = g(y_{i-1},s_i,c_i) $$

$s_i$是RNN的hidden state，这里的$c_i$是对于每个target word的context vector，是不同的（一般的RNN模型使用相同的c）。这个context vector来自encoder部分的hidden state序列(sequence of annotations)。

![proposed model](/assets/images/postsimage/0604/birnn_model.jpg)