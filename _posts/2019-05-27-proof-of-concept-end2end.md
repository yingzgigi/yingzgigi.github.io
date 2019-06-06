---
layout: post
title: "从语音到文本的end-to-end翻译"
date: 2019-05-27
category: 论文notes
tags: 
    - speech translation
    - NMT
---

## [LISTEN AND TRANSLATE: A PROOF OF CONCEPT FOR END-TO-END SPEECH-TO-TEXT TRANSLATION] 2016 Berard et al. ##

>End-to-end speech translation. Def. : the direct translation of an audio signal without intermediate transcription steps.

现在的语音翻译系统整合使用两个主要模型：source language speech recognition（ASR）和 source-to-target text （MT）。在这些方法里，源语言的text transcript（sequence或者graph）用于生成目标语言的text hypothesis。

当时的“encoder-decoder”结构是一种把输入信号的序列投射到连续的低维空间里并且从而生成输出序列。如果不考虑从源语音序列直接翻译到目标词序列或者从平行phone-word序列发现双语语料库，那么从源语音信号到目标语言文本的直接翻译的唯一尝试是Duong的源语音发音和它们的文本翻译之间的校准，以及Anastasopoulos提出用IBM Model 2和dynamic time warping(DTW)来校准源语音和目标文本，但当时Duong及另一论文都没有提出完整的end-to-end翻译系统。

这篇论文首次尝试建立一个end-to-end speech-to-text translation system，并且在学习或者解码过程中不使用源语言文本。减少对源语言转录的需求可以改变语言翻译时的数据收集方式，从收集speech transcription变成双语者看到目标语言文本的表达然后直接以源语言说话。

### Model

比较两个end-to-end模型：标准机器翻译和语音翻译（都使用了attention-based encoder-decoder神经网络）。
 
输入序列($x_1, ..., x_A$), 多层双向的LSTM的encoder生成输出序列h=($h_1, ..., h_A$). decoder初始状态$s_0 = tanh(W_{init} s^{'}_A)$, $s^{'}_A$是encoder最后一个状态。decoder的下一个状态是

$$s_t, o_t = LSTM(s_{t-1}, E_{z_{t-1}})$$

$s_t$是LSTM的新状态，$o_t$是输出。在训练时，$z_{t-1}$是目标序列reference中的前一个标记；评估时，$z_{t-1}$是预测的前一个time-step的标记。E是目标嵌入矩阵，用于word embedding。(在encoder部分也有embedding layer，which represent word using 1-hot encoding。represent word in continuous vector用小维度学习语言模型来找到词的相似性，自动学习描述该词的特征。)

LSTM图如Seq2Seq那篇论文。decoder部分的关键是训练training和评估evaluation（predicting）是分开进行的但是共享参数，也就是说先用training得到的参数用于predicting。training时使用embedding后的target数据作为LSTM单元的输入;evaluation时使用前一个predicting的结果作为输入，没有target数据，只有t-1时的cell state和输出o。

与LSTM的输出$$o_t$$级联的attention vector $d_t$
 
$$d_t = attention(h, s_t)$$

this vector is mapped to a vector of the same size as the target vocabulary,

$$y_t = W_{proj}(o_t \bigoplus d_t) + b_{proj}$$   

to compute a probability distribution over all target words

$$p(w_t|y_{t-1}, s_{t-1}) = softmax(W_{out} y_t + b_{out})$$

每个time-step greedy decoder（每次只选最可能的那一个词）选取概率最大时的$w_t$。

在文本输入时用了注意力机制（attention mechanism）
$$attention(h, s_t) = \sum{a_i^t h_i}$$
$$a_i^t = softmax(u_i^t)$$
$$u_i^t = v^T tanh(W_1 h_i + W_2 s_t + b_2)$$

![example of alignment performed by the attention mechanism during training](\assets\images\postsimage\0528\alignments_performance.jpg)

为了能在语音输入时让attention model记住并且不重复翻译同一部分信号，用了卷积注意力模型（convolutional attention），用convolutional filter来记录前一个time-step的attention weights。这个对于输入语音序列特别长的时候特别有用。

### Experiments

他们生成了一种有语音合成的语音翻译语料库，使用小型平行语料库French-English BTEC corpus。因为这两个语种有相似的语序（SVO-SVO主动宾）。

![Results of machine translation experiments](\assets\images\postsimage\0528\results_text_translation.jpg)

![Results of speech translation experiments](\assets\images\postsimage\0528\results_speech_translation.jpg)

语音文本转换模型比机器翻译模型表现差一点。

### Conclusion

这篇论文提出的end-to-end语音翻译模型的结果建立在小型法-英合成语料库，首次证明了在不使用源语言的情况下进行端到端语音到文本翻译的可能性。
