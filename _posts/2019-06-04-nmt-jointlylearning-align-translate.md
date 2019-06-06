---
layout: post
title: "NMT对encoder-decoder的扩展：align和translate"
date: 2019-05-27
category: 论文notes
tags: 
    - NMT
---

## NEURAL MACHINE TRANSLATION BY JOINTLY LEARNING TO ALIGN AND TRANSLATE ##

NMT旨在建立一个可以同时调整并最大化翻译性能的神经网络。一般用的encoder-decoder模型用于固定长度的句子，encode句子后得到固定长度的向量，这样对NMT来说不能提高performance。所以用一个能自动对与预测某个target word相关的部分语句soft search的模型，而不用form这部分as a hard segement explicitly（就是硬性细分）。也就是soft alignment。

joint translation model联合翻译模型只用local sentence来预测。discriminatice word lexica：优点global context；缺点no position information。

