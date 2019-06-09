---
layout: post
title: "关于grammar的注意力模型"
date: 2019-06-06
category: 论文notes
mathjax: true
tags: 
    - Grammar
    - attention model
    - LSTM
---

point: 这篇论文提出一种使用线性解析树的注意力机制模型。

## [GRAMMAR AS A FOREIGN LANGUAGE] 2015 Vinyals et al. ##

在此前对于syntactic constituency parsing句法组成性解析问题研究结果大多是特定领域的，复杂且不高效，这篇论文提出一种domain agnostic且更有效率的增强注意力的Seq2Seq模型: LSTM+A Pasing Model

### LSTM+A Pasing Model ###

用LSTM+A模型对输入句子“Go.”翻译，输入顺序逆向（但不逆向操作parse trees）所以输入序列是[., Go, END]

![LSTM+A model](/assets/images/postsimage/0606/lstm+a.jpg)

3层LSTM，每层256个LSTM单元（即hidden dimensionality是256）。对encoder和decoder的使用不同的LSTM的参数。图里的XX是POS-tag normalization。part-of-speech，词类，指单词怎样聚类，因为这里在句法解析F1评分里不会被评估所以用XX代替。

图里最上层的灰色箭头是attention机制。

#### Model ####

输入x，控制状态h，记忆状态m，LSTM用非线性的元素积计算h序列和m序列。

![LSTM model](/assets/images/postsimage/0606/lstm_model.jpg)

在deep LSTM，每个子序列层之间用前一层的h序列当作这一层的输入序列x。计算出来的在输入序列条件下的输出序列上的概率分布是

![deep LSTM distribution](/assets/images/postsimage/0606/deep_lstm.jpg)

#### Attention Mechanism ####

![attention computing](/assets/images/postsimage/0606/attention_computing.jpg)

v和W都是可以通过模型学习到的参数，u包含输入句子长度信息，第i个u包含了对第i个hidden encoder state的分数h即多少注意力分配给它，a是对u归一化后的attention mask，

#### Linearizing Parsing Trees ####

![linearizing](/assets/images/postsimage/0606/linearization.jpg)

图里展示根据深度优先遍历顺序depth-first traversal order把parse tree线性化成一个序列。

用上面的LSTM+A模型来解析的话，先把句子从左到右的顺序输入（这里逆序），保存生成的向量，然后输出基于这些向量信息的线性化后的parse tree。然后新的d’是跟d级联产生的新的hidden state，用它来预测以及回馈到循环模型的下一个time step。