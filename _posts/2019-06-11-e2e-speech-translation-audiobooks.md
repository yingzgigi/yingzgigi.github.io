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
#### Speech Encoder ####



#### Character-level decoder ####


### 实验结果分析 ###