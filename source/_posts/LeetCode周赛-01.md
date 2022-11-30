---
title: LeetCode周赛-01
date: 2022-07-17 12:32:35
tags: LeetCode
---

[比赛 link](https://leetcode.cn/contest/weekly-contest-302)

#### [6120. 数组能形成多少数对](https://leetcode.cn/problems/maximum-number-of-pairs-in-array/)

```java
class Solution {
    public int[] numberOfPairs(int[] nums) {
        Map<Integer, Integer> mp = new HashMap<>();
        int n = nums.length;
        for (int num: nums) {
            mp.put(num, mp.getOrDefault(num, 0) + 1);
        }
        Iterator iter = mp.entrySet().iterator();
        int pair = 0;
        int cnt = 0;
        while (iter.hasNext()) {
            Map.Entry entry = (Map.Entry) iter.next();
            int key = (Integer) entry.getKey();
            int val = (Integer) entry.getValue();
            pair += val / 2;
            cnt += val % 2;
        }
        return new int[]{pair, cnt};
    }
}
```

Map遍历

```java
for (Map.Entry<Integer, Integer> entry: mp.entrySet()) {
    int key = entry.getKey();
    int value = entry.getValue();
}
```

#### [6164. 数位和相等数对的最大和](https://leetcode.cn/problems/max-sum-of-a-pair-with-equal-sum-of-digits/)

```java
class Solution {
    public int sumNum(int num) {
        int sum = 0;
        while (num != 0) {
            sum += num % 10;
            num /= 10;
        }
        return sum;
    }
    public int maximumSum(int[] nums) {
        Map<Integer, List<Integer>> mp = new HashMap<>();
        for (int num: nums) {
            int sum = sumNum(num);
            List<Integer> cur = mp.getOrDefault(sum, new ArrayList<Integer>());
            cur.add(num);
            mp.put(sum, cur);
        }
        Iterator iter = mp.entrySet().iterator();
        int ans = -1;
        while (iter.hasNext()) {
            Map.Entry entry = (Map.Entry) iter.next();
            List<Integer> list = (List<Integer>) entry.getValue();
            if (list.size() > 1) {
                list.sort((a, b)->{return b-a;});
                ans = Math.max(list.get(0)+list.get(1), ans);
            }
        }
        return ans;
    }
}
```

#### [6121. 裁剪数字后查询第 K 小的数字](https://leetcode.cn/problems/query-kth-smallest-trimmed-number/)

```java
class Solution {
    public int fun(String[] nums, int trim, int k) {
        int n = nums.length;
        String[] trims = new String[n];
        for (int i = 0; i < n; i++) {
            int sLen = nums[i].length();
            trims[i] = nums[i].substring(sLen - trim, sLen);
        }
        class Node {
            int id;
            String s;
            Node() {}
            Node(int _id, String _s) {
                id = _id;
                s = _s;
            }
        }
        Node[] sorted = new Node[n];
        for (int i = 0; i < n; i++) {
            sorted[i] = new Node(i, trims[i]);
        }
        List<Node> collect = Arrays.stream(sorted).sorted((a, b) -> {
            return a.s.compareTo(b.s);
        }).collect(Collectors.toList());
        return collect.get(k-1).id;
    }

    public int[] smallestTrimmedNumbers(String[] nums, int[][] queries) {
        int n = queries.length;
        int[] res = new int[n];
        for (int i = 0; i < n; i++) {
            res[i] = fun(nums, queries[i][1], queries[i][0]);
        }
        return res;
    }
}
```

#### [6122. 使数组可以被整除的最少删除次数](https://leetcode.cn/problems/minimum-deletions-to-make-array-divisible/)

```java
class Solution {
    public boolean check(int[] nums, int x) {
        for (int num: nums) {
            if (num % x != 0) return false;
        }
        return true;
    }
    public int minOperations(int[] nums, int[] numsDivide) {
        int ans = -1;
        Arrays.sort(nums);
        Map<Integer, Integer> mp = new TreeMap<>();
        for (int num: nums) {
            mp.put(num, mp.getOrDefault(num, 0) + 1);
        }
        int sum = 0;
        Iterator iter = mp.entrySet().iterator();
        while (iter.hasNext()) {
            Map.Entry entry = (Map.Entry) iter.next();
            int key = (Integer) entry.getKey();
            if (check(numsDivide, key)) {
                return sum;
            }
            int value = (int) entry.getValue();
            sum += value;
        }
        return ans;
    }
}
```

学习引用：

```java
class Solution {
    public int minOperations(int[] nums, int[] numsDivide) {
        var g = 0;
        for (var x : numsDivide)
            g = gcd(g, x);
        Arrays.sort(nums);
        for (var i = 0; i < nums.length; i++)
            if (g % nums[i] == 0) return i;
        return -1;
    }

    int gcd(int a, int b) {
        return a == 0 ? b : gcd(b % a, a);
    }
}

作者：endlesscheng
链接：https://leetcode.cn/problems/minimum-deletions-to-make-array-divisible/solution/zhuan-huan-tan-xin-pythonjavacgo-by-endl-vdgw/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

