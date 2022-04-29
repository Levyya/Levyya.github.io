---
title: DEBERTA
date: 2022-04-28 20:31:35
tags: 论文
---

## 更新

[DeBERTaV3](https://arxiv.org/abs/2111.09543)



## 摘要

预训练模型在很多NLP任务上效果很好。作者提出DeBERTa（**De**coding-enhenced **BERT** with disentangled **a**ttention）：使用两种新技术改进BERT和RoBERTa。第一，**分解注意力机制**（disentangled attention mechanism），每个词语使用两个向量表示，分别编码词语的内容和位置，注意力权重基于词语和相对位置的矩阵分别计算。第二，**增强的掩码解码器**用来预测掩码标记，包含绝对位置信息。另外，提出一种新的虚拟对抗训练方法，用来微调和提升模型的生成能力。实验在NLU和NLG任务上表现很好。

## CODE

https://github.com/microsoft/DeBERTa

https://huggingface.co/models?filter=deberta

## 引言

RNN -> Transformers -> GPT -> BERT -> RoBERTa -> XLNet -> UniLM -> ELECTRA -> T5 -> ALUM -> StructBERT -> ERNIE

这篇论文提出两种改进技术：

### Disentangled attention

使用两个向量编码一个词语，分别表示内容和位置；注意力权重也分别有内容和位置矩阵计算得到。

### Enhanced mask decoder

预训练任务仍使用掩语言模型（MLM）。在分解注意力机制中，DeBERTa使用上下文的内容和相对位置信息，但是没有使用绝对位置信息。“a new store opened beside the new mall“，store和mall语义上相同，但是语法角色不同，store是主语。这种语义关系依赖于词语的绝对位置。DeBERTa通过聚合上下文的词语内容和位置嵌入向量来引入绝对位置编码。

