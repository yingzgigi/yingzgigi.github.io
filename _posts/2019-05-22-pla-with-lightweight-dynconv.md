---
layout: post
title: "关于light weight和dynamic convolution"
date: 2019-05-16
category: 论文notes
tag: machine translation
---
* [PAY LESS ATTENTION WITH LIGHTWEIGHT AND DYNAMIC CONVOLUTGIONS]

    这篇论文关注通过lightweight convolutions和dynamica convolutions来解决MT中较长序列问题，降低网络参数，提升训练速度。


    * Transformer

        ![Transformer](/assets/images/postsimage/transformer.png)

        <!-- <img src="https://github.com/yingzgigi/yingzgigi.github.io/blob/master/_posts/postsimage/transformer.png" alt="Transformer" title="Transformer" width="50" height="50" /> -->

        Transformer可以用来解决自然语言不定长问题，跟CNN类似，设定输入的最大长度，用Padding填充不够的部分，使模型输入定长。Transformer为了保留输入句子单词之间的相对位置信息，用位置函数进行位置编码，在输入端输入位置信息。Transformer的第一层是Multi-head self attention。

    * Self Attention

        ![Self Attention](/assets/images/postsimage/self attention.jpg)

        <!-- <img src="https://github.com/yingzgigi/yingzgigi.github.io/blob/master/_posts/postsimage/self%20attention.jpg" alt="Self Attention" title="Self Attention" width="50" height="50" /> -->

        Self Attention 会让当前输入单词和句子中任意单词发生关系，然后集成到一个embedding向量里，因为这个特性self attention可以解决NLP句子中长距离依赖特征问题。相比较，RNN需要通过隐层节点往后传，CNN需要通过增加网络深度来捕获远距离特征。

    * Lightweight Convolution

    * Dynamic Convolution
  
    * Experiment

    * Results

    * Conclustion



