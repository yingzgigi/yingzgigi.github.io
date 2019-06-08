---
layout: post
title: "关于grammar的注意力模型"
date: 2019-06-06
category: 论文notes
mathjax: true
tags: 
    - Grammar
    - attention model
---

point: 这篇论文提出一种使用线性解析树的注意力机制模型。

## [GRAMMAR AS A FOREIGN LANGUAGE] 2015 Vinyals et al. ##

在此前对于syntactic constituency parsing句法组成性解析问题研究结果大多是特定领域的，复杂且不高效，这篇论文提出一种domain agnostic且更有效率的增强注意力的Seq2Seq模型: LSTM+A Pasing Model

### LSTM+A Pasing Model ###

用LSTM+A模型对输入句子“Go.”翻译，输入顺序逆向（但不逆向操作parse trees）所以输入序列是[., Go, END]

![LSTM+A model](/assets/images/postsimage/0606/lstm+a.jpg)

3层LSTM，每层256个LSTM单元。对encoder和decoder的使用不同的LSTM的参数。图里的XX是POS-tag normalization。part-of-speech，词类，指单词怎样聚类，因为这里在句法解析F1评分里不会被评估所以用XX代替。

图里最上层的灰色箭头是attention机制。

#### Model ####

输入x，控制状态h，记忆状态m，LSTM用非线性的元素积计算h序列和m序列。

![LSTM model](/assets/images/postsimage/0606/lstm_model.jpg)

在deep LSTM，每个子序列层之间用前一层的h序列当作这一层的输入序列x。计算出来的在输入序列条件下的输出序列上的概率分布是

![deep LSTM distribution](/assets/images/postsimage/0606/deep_lstm.jpg)

#### Attention Mechanism ####

![attention computing](/assets/images/postsimage/0606/attention_computing.jpg)

#### Linearizing Parsing Trees ####