---
title: OJ剑指offer22. 正则表达式匹配
date: 2022-03-22 20:05:22
tags:
---

```c++
bool isMatch(string s, string p) {
    int m = s.size();
    int n = p.size();
    
    auto matches = [&](int i, int j) {
        if (i == 0) return false;
        if (p[j-1] == '.') return true;
        return s[i-1] == p[j-1];
    };
    
    vector<vector<int> > f(m + 1, vector<int>(n + 1));
    f[0][0] = true;
    for (int i = 0; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (p[j-1] == '*') {
                f[i][j] |= f[i][j-2];
                if (mathes(i, j - 1)) {
                    f[i][j] |= f[i-1][j];
                }
            } else {
                if (matches(i, j)) {
                    f[i][j] |== f[i-1][j-1];
                }
            }
        }
    }
    return f[m][n];
}
```

一种递归实现（参考）

https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/solution/zheng-ze-biao-da-shi-pi-pei-by-leetcode-s3jgn/

```c++
bool isMatch(string s, string p) {
    if (p.empty()) return s.empty();
    bool first_match = !s.empty() && (s[0] == p[0] || p[0] == '.');
    if (p.size() >= 2 && p[1] == '*') {
        return (first_match && isMatch(s.substr(1), p)) || isMatch(s, p.substr(2));
    } else {
        return first_match && isMatch(s.substr(1), p.substr(1));
    }
}
```

