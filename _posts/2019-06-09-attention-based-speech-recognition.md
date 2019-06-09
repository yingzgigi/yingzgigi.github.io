---
layout: post
title: "语音识别下的注意力模型"
date: 2019-06-09
category: 论文notes
mathjax: true
tags: 
    - Speech Recognition
    - attention mechanism
---

## [ATTENTION-BASED MODELS FOR SPEECH RECOGNITION] 2015 Chorowski et al. ##

在将注意力机制用于语音识别时，Bahdanau的NMT模型只能用于跟那些训练过的话语差不多一样长的utterances（发音出来的）话语。所以这篇论文着重在一种给注意力机制添加位置意识（location-awareness）来解决前面问题的方法。

###  ###