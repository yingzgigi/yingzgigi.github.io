---
layout: post
title: "从语音到文本的end-to-end翻译"
date: 2019-05-27
category: 论文notes
tag: speech translation
---

## [LISTEN AND TRANSLATE: A PROOF OF CONCEPT FOR END-TO-END SPEECH-TO-TEXT TRANSLATION] ##

现在的语音翻译系统整合使用两个主要模型：source language speech recognition（ASR）和 source-to-target text （MT）。在这些方法里，源语言的text transcript（sequence或者graph）用于生成目标语言的text hypothesis。

当时的“encoder-decoder”结构是一种把输入信号的序列投射到连续的低维空间里并且从而生成输出序列。如果不考虑从源语音序列直接翻译到目标词序列或者从平行phone-word序列发现双语语料库，那么从源语音信号到目标语言文本的直接翻译的唯一尝试是Duong的源语音发音和它们的文本翻译之间的校准，以及Anastasopoulos提出用IBM Model 2和dynamic time warping(DTW)来校准源语音和目标文本，但当时Duong及另一论文都没有提出完整的end-to-end翻译系统。

这篇论文首次尝试建立一个end-to-end speech-to-text translation system，并且在学习或者解码过程中不使用源语言文本。减少对源语言转录的需求可以改变语言翻译时的数据收集方式，从收集speech transcription变成双语者看到目标语言文本的表达然后直接以源语言说话。

#### Model

#### Experiments

#### Conclusion

这篇论文提出的end-to-end语音翻译模型的结果建立在小型法-英合成语料库。其后续工作是对non-synthetic data做评估分析。
