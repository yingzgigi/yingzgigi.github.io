---
layout: post
title: "Seq2Seq结构"
date: 2019-06-01
category: 散记
tags: 
    - Seq2Seq
    - NMT
    - speech translation
---

## [SEQUENCE TO SEQUENCE LEARNING WITH NEURAL NETWORKS] 2014 Stutskever et al. ##

### Sequence to sequence architecture: encoder network - decoder network

![Seq2Seq model](/assets/images/postsimage/0601/Seq2Seq_model.jpg)

机器翻译模型中，绿色是encoder，紫色decoder，decoder跟language model类似。encoder network计算出一系列向量来表示输入的句子，然后从这向量开始decoding，估计翻译句子相对于输入句子的概率 $$P(y^1,...,y^{T_y}|x)$$，然后找到most likely translation，conditional language model sogennant。为了找出概率最大时的y，最通用算法是beam search，不用greedy search是因为它每次挑选最有可能的一个词而不是一次性挑选出整个单词序列，可能导致一些翻译错误，而且可能的组合数量过于巨大。

![encoder-decoder](/assets/images/postsimage/0601/encoder-decoder.jpg)

