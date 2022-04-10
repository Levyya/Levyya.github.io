---
title: OJ卡特兰数
date: 2022-03-17 14:34:17
tags: algorithm
tags: math
---

1, 2, 5, 14, 42, 132, ...

卡特兰数满足：

$$G(n)=\sum_{i=1}^nG(i-1)G(n-i)$$

递推关系：

$$h(n)=\frac{C_{2n}^n}{n+1}=\frac{2(2n-1)}{n+1}h(n-1)$$

常见问题：

* n个节点能构成多少种不同的二叉树

* 一个栈([无穷大](https://baike.baidu.com/item/无穷大))的进栈序列为1,2,3,..n,有多少个不同的出栈序列?

* 形如这样的直角三角形网格，从左上角开始，只能向右走和向下走，问总共有多少种走法？

  ![image-20220317144019464](/Users/levy/Library/Application Support/typora-user-images/image-20220317144019464.png)

* 矩阵连乘：

  ![img](https://bkimg.cdn.bcebos.com/formula/66805a78efb4dd54c1bcae93843bafea.svg)

   ，共有（n+1）项，依据[乘法结合律](https://baike.baidu.com/item/乘法结合律)，不改变其顺序，只用括号表示成对的乘积，试问有几种括号化的方案？或者说：有n对括号，可以并列或嵌套排列，共有多少种情况？

* n个0和n个1组成一个2n位的2进制数，要求从左到右扫描时，1的累计数始终都**小于等于**0的累计数，求满足条件的数有多少？
