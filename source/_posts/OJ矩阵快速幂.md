---
title: OJ矩阵快速幂
date: 2022-03-21 14:34:57
tags: algorithm
---

参考博客：https://blog.csdn.net/zhangxiaoduoduo/article/details/81807338

## 模版

```c++
struct Mat {
    LL m[101][101];
};
Mat in, out;
Mat Mul(Mat x, Mat y) {
    Mat c;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            c.m[i][j] = 0;
        }
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            for (int k = 1; k <= n; k++) {
                c.m[i][j] = c.m[i][j] % mod + x.m[i][k] * y.m[k][j] % mod;
            }
        }
    }
    return c;
}
Mat pow(Mat x, LL y) {
	Mat c = e;
    while (y) {
        if (y & 1) c = Mul(c, x);
        x = Mul(x, x);
        y >>= 1;
    }
    return c;
}
```

