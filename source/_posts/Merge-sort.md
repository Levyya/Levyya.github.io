---
title: Merge sort
date: 2022-03-02 15:05:59
tags: Algorithm
---

# Merge sort

用途：计算数组中的[逆序对](https://www.nowcoder.com/practice/96bd6684e04a44eb80e6a68efc0ec6c5?tpId=295&tqId=23260&ru=/practice/96bd6684e04a44eb80e6a68efc0ec6c5&qru=/ta/format-top101/question-ranking)

```c++
class Solution {
private:
    const int mod = 1e9 + 7;
public:
    int InversePairs(vector<int> data) {
        int sum = 0;
        vector<int> temp(data.size());
        mergeSort(data, 0, data.size()-1, sum, temp);
    }
    
    void mergeSort(vector<int> &data, int l, int r, int &sum, vector<int> &temp) {
        if (l >= r) return;
        int mid = l + ((r - l) >> 1);
        mergeSort(data, l, mid, sum, temp);
        mergeSort(data, mid+1, r, sum, temp);
        merge(data, l, mid, r, sum, temp);
    }
    
    void merge(vector<int> &data, int l, int mid, int r, int &sum, vector<int> &temp) {
        int i = l, j = mid + 1, k = 0;
        while (i <= mid && j <= r) {
            if (data[i] >= data[j]) {
                temp[k++] = data[j++];
                sum = (sum + mid - i + 1) % mod;
            } else {
                temp[k++] = data[i++];
            }
        }
        while (i <= mid) temp[k++] = data[i++];
        while (j <= r) temp[k++] = data[j++];
        for (i=l, k=0; i <= r; i++, k++) {
            data[i] = temp[k];
        }
    }
};
```



