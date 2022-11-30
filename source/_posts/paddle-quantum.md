---
title: paddle_quantum
date: 2022-10-12 09:29:16
tags:
- quantum
---

## 量子神经网络--[LOCCNet](https://qml.baidu.com/tutorials/loccnet/loccnet-framework.html)

> 量子纠缠在量子通信、量子计算以及其他量子技术中是一种很重要的资源。因此，能否在这些领域构建出实际的应用，很大程度上取决于我们能否有效地利用量子纠缠这一资源。通过本地操作和经典通讯（LOCC）来完成特定任务，是比全局操作（global operation）更为有效的方式。

### 什么是LOCC

LOCC 指代的是本地操作和经典通讯（local operations and classical communication），即一个多量子比特系统分配给位于不同位置的多个实验室（参与方）。

设计理念：解决量子多体问题、预测蛋白质折叠结构



[变分量子电路编译](https://github.com/PaddlePaddle/Quantum/blob/master/tutorials/qnn_research/VQCC_CN.ipynb)

概念：通过优化参数化量子电路来模拟未知酉算子的过程

两种未知酉算子情况：

1. 给定酉算子的矩阵形式
2. 给定一个实现酉算子的黑箱，可以将其接入电路但不允许访问其内部构造

`量子编译`是将量子算法中的酉变换转化为一系列量子门序列从而实现量子算法的过程。

> 有点类似矩阵分解，将一个大的矩阵运算拆分成给定的已知的矩阵序列

实现：

[QAQC](https://quantum-journal.org/papers/q-2019-05-13-140/)