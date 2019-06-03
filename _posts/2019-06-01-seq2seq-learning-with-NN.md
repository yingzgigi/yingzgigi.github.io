---
layout: post
title: "Seq2Seq结构"
date: 2019-06-01
category: 论文notes
tags: 
    - Seq2Seq
    - NMT
    - speech translation
---

## [SEQUENCE TO SEQUENCE LEARNING WITH NEURAL NETWORKS] 2014 Stutskever et al. ##

### Sequence to sequence architecture: encoder network - decoder network

![Seq2Seq model](/assets/images/postsimage/0601/Seq2Seq_model.jpg)

机器翻译模型中，绿色是encoder，紫色decoder，decoder跟language model类似。encoder network计算出一系列向量来表示输入的句子，然后从这向量开始decoding，估计翻译句子相对于输入句子的概率 $$P(y^1,...,y^{T_y} \mid x)$$，然后找到most likely translation，conditional language model sogennant。为了找出概率最大时的y，最通用算法是beam search，不用greedy search是因为它每次挑选最有可能的一个词而不是一次性挑选出整个单词序列，可能导致一些翻译错误，而且可能的组合数量过于巨大。

![encoder-decoder](/assets/images/postsimage/0601/encoder-decoder.jpg)

encoder network是一个RNN结构，RNN unit可以是GRU或者LSTM。每次向该网络输入一个法语单词（Fr-En），输入序列接收完毕后，RNN输出一个向量代表输入序列，decoder network以encoder的输出为输入，训练后每次输出得到一个翻译后的单词，直到输出序列的句子结尾标记。每次生成的标记传递到下一个单元进行预测。

### LSTM ###

DNN要求输入和输出的维数已知且确定。而RNN虽然是对于序列的前向NN的一般化方法，它可以在提前知道aligenment的情况下很容易地把序列映射到序列，但是RNN不能用于解决输入输出序列有不同的长度和复杂非单调关系。LSTM可以用于long range temporal dependencies远距离时间相关，因为一般RNN对long term dependency时的backpropagation难以发现跨度太大的关联性，因为RNN用gradient会产生梯度消失问题。

这篇论文中用LSTM来估计条件概率$$p(y_1,...,y_T'\mid x_1, ..., x_T)$$ d.i.Wahrscheinlichkeit der Ausgabesequenz gegeben Eingabesequenz。这里输出和输入序列长度可以不一样。LSTM通过先观察由上一个LSTM的隐藏状态获得的输入序列的固定维度v来计算条件概率，然后用标准LSTM-LM计算输出y序列概率

![LSTM-LM formulatioin](/assets/images/postsimage/0601/lstm-lm-formulation.jpg)

每个$$p(y_t \mid v, y_1, ..., y_{t-1})$$就是softmax over all the words in the vocabulary。<EOS>使模型可以在任意长度的序列上定义distribution。

![LSTM model](/assets/images/postsimage/0601/LSTM.jpg)

图中LSTM先计算ABC\<EOS\>的representation再用这个representation计算WXYZ\<EOS\>的概率。输入序列和输出序列分别用不同的LSTM，因为可以在极小的计算成本下增加模型参数，又可以让LSTM在多语言对multiple language pairs上同时训练。

并且在LSTM的输入序列上，他们通过实验发现选择句子逆向输入有更好的BLEU分数，还能降低test perplexity。虽然没有完整的解释为什么逆向输入额能改变性能，大概原因猜测是数据集里有很多短距离依赖：逆向序列输入并不改变源语句和目标语句中每个相应词的平均距离，但是它使源语句最开头的几个词和目标语句对应的词更近，从而降低了minimal time lag最小时间滞后。然后在backpropagation的时候更容易在源语句和目标语句之间establishing communication。