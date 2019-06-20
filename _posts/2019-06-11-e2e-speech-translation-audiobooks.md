---
layout: post
title: "e2e语音翻译"
date: 2019-06-11
category: 论文notes
mathjax: true
tags: 
    - Speech Translation
    - End-to-end model
    - pyramidal encoder
    - character-level decoder
    - multi-task
---

point：

## [END-TO-END AUTOMATIC SPEECH TRANSLATION OF AUDIOBOOKS] 2018 Berard et al. ##

之前的在语音到文本的直接翻译的论文考虑极端情况时指的是在学习和decoding过程中没有源语言转录。但是这篇论文作者们采用一种midway方式：就是source language transcription能在训练模型的过程中用，这样的话模型就可以训练成一次性解码源语音到目标文本。这篇论文有三个task：ASR，AST，MT。

### audiobooks corpus ###
#### augmented LibriSpeech ####
large sclae study（相比前一篇论文用的小型合成语料库）。

#### 比较e2e直接翻译和结合MT和ASR翻译这两种方法 ####

### end-to-end models ###

这里使用的是multi-task training，也就是在任务过程中share部分模型，即ASR和AST使用相同的encoder结构，AST和MT使用相同的decoder结构。因为前两个语音输入需要用之前一篇论文的attention模型，后两者都是文本输出可以用同一个结构。

#### Speech Encoder ####

speech encoder使用一种混合模型，混合了Seq2Seq[Weiss, 2017]的卷积模型和PoC E2E[Berard, 2016]的encoder模型。encoder部分一共有七层，前面2层用tanh处理输入的语音frame,得到的feature大小为n', 这两层处理方式类同他前一篇论文（2016）。然后通过2层convolutional layer，每层使用16个3x3的filter，第1层的depth是1，第2层的depth是16，步长为（2，2），所以每经过一层减少长度和大小，经过两层得到的就是$(T_x/4, n'/4, 16)$。 最后是三层LSTM层。在LSTM层之间使用了time pooling，即pyramidal encoder金字塔形状，减少特征大小和时间长度，加快训练。最后结果相比他的前一篇论文输出更短序列。

#### Character-level decoder ####

字符级别decoder由一个条件LSTM组成，然后全连接层dense layer。参考toolkit那篇论文和vanilla golbal注意力机制论文。在toolkit里面提到跟前面的2015Bahdanau的论文里相比较的update，look和generate分别有什么新的东西。并且这里亮点是生成器生成的字符是目标词库里分数最高的符号。

### 实验结果分析 ###
实验结果说明他们提出的这个端到端直接翻译模型表现不如级联ASR和MT模型。2019April的语音到语音直接翻译模型更好，之后看。