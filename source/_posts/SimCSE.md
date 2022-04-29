---
title: SimCSE
date: 2022-04-25 17:59:40
tags: 论文
---

[论文地址](https://arxiv.org/pdf/2104.08821.pdf)

## 摘要

SimCSE，一种简单的对抗学习框架，提升句子词嵌入到SOTA。首先描述了一种无监督方法，仅使用标准dropout作为扰动，在对抗任务中输入句子并预测它自己。这种简单的方法效果出奇的好，甚至与对应的监督方法相当。作者发现dropout作用是数据增强，移除它会导致表示坍塌。还提出一种监督方法，从NLI数据集中引入注释句子对，用蕴含对作为正例，用矛盾对作为反例。作者评估SimCSE在文本语义相似度（Semantic textual similarity, STS）任务上，使用BERT_base实现很好的结果。作者从理论和实验上展示对抗学习令预训练词嵌入空间更uniform，有监督标签时能更好对齐正例句子对。

## 对抗学习

数据增强

* Crop 裁剪
* Word deletion 删除单词
* Synonym replacement 同义词替换
* MLM
