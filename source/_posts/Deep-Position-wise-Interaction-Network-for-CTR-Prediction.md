---
title: Deep Position-wise Interaction Network for CTR Prediction
date: 2022-04-26 21:21:39
tags: 论文
categories: 
- 计算机
- 搜索
---

[论文地址](https://arxiv.org/pdf/2106.05482v2.pdf)

论文作者：美团 Jianqiang Huang

了解来源：百度飞浆论文复现赛

关键字：搜索广告；CTR预估；

### 摘要

点击通过率（CTR，Click-through rate）在网络广告和推荐系统中扮演很重要的角色。实际上，训练CTR模型取决于点击数据，模型会偏向更高位置，实际上更高位置确实有更高的CTR。现在的方法可以通过【IPW, inverse propensity weighting】方法在一定程度上缓解这个偏差问题。然而，位置信息在训练时和推理时的不同处理方式会影响线上性能。而且，点击率是【】和【】的乘积的假设过于简单。这里提出DPIN（Deep Position-wise Interaction Network）去结合标的物和位置信息去估计每个位置的CTR，实现balabala的一致性和模拟xx之间的深度非线性交互作用。这里还提出一种新的评估尺度，PAUC（postion-wise AUC），适合评估给定位置的排序质量。通过实验验证了方法的有效性。

### 引言

1段：CTR很重要。

2段：然而，位置偏差影响了模型性能。

3段：现有方法处理位置偏差问题。【位置作为特征、PAL框架、IPW、知识蒸馏、对抗NN、成对训练】

4段：点击伯努力变量C（click Bernoulli variable C）：
$$
p(C=1|u,c,i,k)=p(E=1|k,[s])p(R=1|u,c,i)
$$

```python
'''
u: user; c: context; i: item i; k: k-th position;
C: click; E: examined; R: relevent; [s]: subset of context;
'''
```

5段：examination assumption 过于简单。

6段：论文贡献点：

* DPIN
* DPIN is the first method...
* PAUC

