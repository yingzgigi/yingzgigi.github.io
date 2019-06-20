---
layout: post
title: "e2e语音翻译"
date: 2019-06-11
category: 论文notes
mathjax: true
tags: 
    - Speech Translation
    - End-to-end model
    - LibriSpeech
---

point：

## [END-TO-END AUTOMATIC SPEECH TRANSLATION OF AUDIOBOOKS] 2018 Berard et al. ##

之前的在语音到文本的直接翻译的论文考虑极端情况时指的是在学习和decoding过程中没有源语言转录。但是这篇论文作者们采用一种midway方式：就是source language transcription能在训练模型的过程中用，这样的话模型就可以训练成一次性解码源语音到目标文本。这篇论文有三个task：ASR，AST，MT。

### audiobooks corpus ###
#### augmented LibriSpeech ####

#### 比较e2e直接翻译和结合MT和ASR翻译这两种方法 ####


### end-to-end models ###

这里使用的是multi-task training，也就是在任务过程中share部分模型，即ASR和AST使用相同的encoder结构，AST和MT使用相同的decoder结构。因为前两个语音输入需要用之前一篇论文的attention模型，后两者都是文本输出可以用同一个结构。

#### Speech Encoder ####

speech encoder使用一种混合模型，混合了Seq2Seq[Weiss, 2017]的卷积模型和PoC E2E[Berard, 2016]的encoder模型。这里的模型在LSTM层之间使用了time pooling，减少特征大小和时间长度，加快训练。最后结果输出短序列(pyramidal encoder金字塔形状)。

#### Character-level decoder ####

字符级decoder由一个卷积LSTM组成，由密度层dense layer。

### 实验结果分析 ###