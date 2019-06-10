---
layout: post
title: "语音识别下的注意力模型"
date: 2019-06-09
category: 论文notes
mathjax: true
tags: 
    - Speech Recognition
    - attention mechanism
    - ARSG
---

points：提出一种运用于语音识别的添加了位置意识的基于注意力机制的循环序列生成器。

## [ATTENTION-BASED MODELS FOR SPEECH RECOGNITION] 2015 Chorowski et al. ##

在将注意力机制用于语音识别时，Bahdanau的NMT模型只能用于跟那些训练过的话语差不多一样长的utterances（发音出来的）话语。所以这篇论文着重在一种给注意力机制添加位置意识（location-awareness）来解决前面问题的方法。

### 一般框架 ###

ARSG（attention based recurrent sequence generator）基于注意力机制的循环序列生成器用的是RNN循环网络随机生成输出序列y（这里音素序列），输入到encoder部分的输入序列是x（这里是特征向量序列，取自音频帧的一个个小的overlapping window）。encoder部分是一个deep BiRNN输出长度跟输入序列一致的representation h。

这个框架生成输出序列y的公式:

![generic attention](\assets\images\postsimage\0609\generic_attention.jpg)

$\alpha$ 是attention weights，也就是NMT那篇论文提到的alignment，previous $\alpha$则暗示了位置信息，就是这篇论文着重提到的location awareness。g是所谓glimpse。每个step都计算一个新的生成器状态$s_i$。LSTM和GRU过程如图，这里只表现了2 steps。

![generall ARSG](\assets\images\postsimage\0609\generall_arsg.jpg)

作者还区分了基于位置的、基于内容的、两者混合的注意力机制。content-based就是在没有前一个$\alpha$的情况下计算，location-based就是在没有h的情况下计算。

> Attend in Eq. (1) describes the most generic, hybrid attention. If the term $\alpha_{i-1}$ is dropped from Attend arguments, i.e., $\alpha_i = Attend(s_{i-1},h)$, we call it content-based.
In this case, Attend is often implemented by scoring each element in h separately and normalizing
the scores:

![content-based](\assets\images\postsimage\0609\content_based.jpg)

比如在这种基于内容的计算中会引起similar speech fragments问题，就是说无论h位置在哪，相同或相似的h都得分相等。encoder部分用BiRNN或者deep CNN就可以解决这个问题。因为h的容量问题，这两个方法也只能在一定程度上用上下文消歧。

单纯基于位置的计算在语音识别处理中就要求必须只用s来预测结果产生的音素之间的距离，因为数量上有很多可能，所以这个操作很难实现。

所以对于语音识别，hybrid attention mechanism是最好的方法。

### ARSG with convolutional Features ###

为了加入位置信息，作者在每个位置j对前一个$\alpha$用F矩阵卷积提取出f，

$$f_i = F * \alpha_{i-1}$$

然后在评分机制e计算中加入f:

![convolving score](\assets\images\postsimage\0609\convolution_score.jpg)

### 归一化score：sharpening & Smoothing ###

![decoding performance](\assets\images\postsimage\0609\result_smooth.jpg)

从decoding的表现来看，如果没有用较大的beam search width那么错误率就比较高（decoding如果生成/<eos/>失败，也被视为其一）。但是使用smooth之后，模型在用beam size=1时表现也很好。
