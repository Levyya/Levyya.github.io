---
title: OJ丑数
date: 2022-03-29 11:10:02
tags: algorithm
---

* 最小堆

  ```c++
  int nthUglyNum(int n) {
      vector<int> factors = {2, 3, 5};
      unordered_set<long> seen;
      priority_queue<long, vector<long>, greater<long>> heap;
      seen.insert(1L);
      heap.push(1L);
      int ugly = 0;
      for (int i = 0; i < n; i++) {
          long cur = heap.top();
          heap.pop();
          ugly = (int)cur;
          for (int factor : facotrs) {
              long next = cur * factor;
              if (!seen.count(next)) {
                  seen.insert(next);
                  heap.push(next);
              }
          }
      }
      return ugly;
  }
  ```

  
