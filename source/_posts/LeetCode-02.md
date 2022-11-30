---
title: LeetCode-02
date: 2022-07-15 19:05:48
tags: LeetCode
---

##  引导学习

* [labuladong算法小抄](https://labuladong.github.io/algo/)

#### [152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/)

DP，注意mini计算时使用的是前一个的maxi结果，不是更新后的。

- [x] 08.12 复习

```java
class Solution {
    public int maxProduct(int[] nums) {
        int maxi = nums[0], mini = nums[0];
        int ans = maxi;
        for (int i = 1; i < nums.length; i++) {
            int mx = maxi, mi = mini;
            maxi = Math.max(nums[i], Math.max(mx*nums[i], mi*nums[i]));
            mini = Math.min(nums[i], Math.min(mx*nums[i], mi*nums[i]));
            ans = Math.max(ans, maxi);
        }
        return ans;
    }
}
```

#### [155. 最小栈](https://leetcode.cn/problems/min-stack/)

```java
class MinStack {
    public Deque<Integer> stk;
    public Deque<Integer> minStk;

    public MinStack() {
        stk = new ArrayDeque<>();
        minStk = new ArrayDeque<>();
    }
    
    public void push(int val) {
        int mini = val;
        if (!minStk.isEmpty()) {
            mini = Math.min(minStk.peek(), mini);  // peekFirst
        }
        stk.push(val);
        minStk.push(mini); // addFisrt
    }
    
    public void pop() {
        stk.pop();
        minStk.pop(); // removeFirst
    }
    
    public int top() {
        return stk.peek();
    }
    
    public int getMin() {
        return minStk.peek(); // peekFirst
    }
}
```

#### [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

计数比较有bug，未解决。

```java
class Solution {
    Map<Character, Integer> need = new HashMap<>();
    Map<Character, Integer> window = new HashMap<>();
    public String minWindow(String s, String t) {
        for (int i = 0; i < t.length(); i++) {
            Character c = t.charAt(i);
            need.put(c, need.getOrDefault(c, 0) + 1);
        }
        int cnt = 0;
        int left = 0, right = 0;
        int start = 0, len = s.length() + 1;
        while (right < s.length()) {
            Character c = s.charAt(right);
            right++;
            if (need.containsKey(c)) {
                window.put(c, window.getOrDefault(c, 0) + 1);
                if (window.get(c) == need.get(c)) {
                    cnt++;
                }
            }
            while (check()) {
                if (right - left < len) {
                    start = left;
                    len = right - left;
                }
                Character cL = s.charAt(left);
                left++;
                if (need.containsKey(cL)) {
                    if (window.get(cL) == need.get(cL)) {
                        cnt--;
                    }
                    window.put(cL, window.getOrDefault(cL, 0) - 1);
                }
            }

        }
        if (len == s.length() + 1) return "";
        return s.substring(start, start + len);
    }
    public boolean check() {
        Iterator iter = need.entrySet().iterator();
        while (iter.hasNext()) {
            Map.Entry entry = (Map.Entry) iter.next();
            Character key = (Character) entry.getKey();
            Integer val = (Integer) entry.getValue();
            if (window.getOrDefault(key, 0) < val) {
                return false;
            }
        }
        return true;
    }
}
```

#### [567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)

使用cnt再次失败，还是需要自己判断是否满足条件。

```java
class Solution {
    Map<Character, Integer> need = new HashMap<>();
    Map<Character, Integer> window = new HashMap<>();

    public boolean checkInclusion(String s1, String s2) {     
        for (int i = 0; i < s1.length(); i++) {
            Character c = s1.charAt(i);
            need.put(c, need.getOrDefault(c, 0) + 1);
        }
        int left = 0, right = 0;
        // int cnt = 0;
        while (right < s2.length()) {
            Character c = s2.charAt(right);
            right++;
            if (need.getOrDefault(c, 0) > 0) {
                window.put(c, window.getOrDefault(c, 0) + 1);
                // if (window.get(c) == need.get(c)) {
                //     cnt++;
                // }
            }
            while (right - left == s1.length()) {
                if (check()) {
                    return true;
                }
                
                // if (cnt == need.size()) {
                //     return true;
                // }
                Character d = s2.charAt(left);
                left++;
                if (need.getOrDefault(d, 0) > 0) {
                    // if (window.get(d) == need.get(d)) {
                    //     cnt--;
                    // }
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        return false;
    }
    public boolean check() {
        Iterator iter = need.entrySet().iterator();
        while (iter.hasNext()) {
            Map.Entry entry = (Map.Entry) iter.next();
            Character key = (Character) entry.getKey();
            Integer val = (Integer) entry.getValue();
            if (window.getOrDefault(key, 0) < val) return false;
        }
        return true;
    }
}
```

#### [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

```java
class Solution {
    Map<Character, Integer> need = new HashMap<>();
    Map<Character, Integer> window = new HashMap<>();
    public List<Integer> findAnagrams(String s, String p) {
        for (int i = 0; i < p.length(); i++) {
            Character c = p.charAt(i);
            need.put(c, need.getOrDefault(c, 0) + 1);
        }
        int left = 0, right = 0;
        List<Integer> res = new ArrayList<>();
        while (right < s.length()) {
            Character c = s.charAt(right);
            right++;
            if (need.getOrDefault(c, 0) > 0) {
                window.put(c, window.getOrDefault(c, 0) + 1);
            }
            while (right - left == p.length()) {
                if (check()) {
                    res.add(left);
                }
                Character d = s.charAt(left);
                left++;
                if (need.get(d) != null) {
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        return res;
    }
    public boolean check() {
        Iterator iter = need.entrySet().iterator();
        while (iter.hasNext()) {
            Map.Entry entry = (Map.Entry) iter.next();
            Character key = (Character) entry.getKey();
            Integer val = (Integer) entry.getValue();
            if (window.getOrDefault(key, 0) < val) {
                return false;
            }
        }
        return true;
    }
}
```

#### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> window = new HashMap<>();
        int left = 0, right = 0;
        int ans = 0;
        while (right < s.length()) {
            Character c = s.charAt(right);
            right++;
            window.put(c, window.getOrDefault(c, 0) + 1);
            while (window.getOrDefault(c, 0) > 1) {
                Character d = s.charAt(left);
                left++;
                window.put(d, window.getOrDefault(d, 0) - 1);
            }
            ans = Math.max(ans, right - left);
        }
        return ans;
    }
}
```

#### [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)

从尾部开始合并

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int l = m-1, r = n-1;
        int k = m + n - 1;
        while (l >= 0 && r >= 0) {
            if (nums1[l] >= nums2[r]) {
                nums1[k--] = nums1[l--];
            } else {
                nums1[k--] = nums2[r--];
            }
        }
        while (r >= 0) {
            nums1[k--] = nums2[r--];
        }
        return;
    }
}
```

#### [13. 罗马数字转整数](https://leetcode.cn/problems/roman-to-integer/)

```java
class Solution {
    public int romanToInt(String s) {
        Map<Character, Integer> mp = new HashMap<>();
        mp.put('I', 1);
        mp.put('V', 5);
        mp.put('X', 10);
        mp.put('L', 50);
        mp.put('C', 100);
        mp.put('D', 500);
        mp.put('M', 1000);
        int sum = 0;
        for (int i = 0; i < s.length(); ) {
            Character c = s.charAt(i);
            int cur = mp.get(c);
            if (i < s.length() - 1) {
                Character d = s.charAt(i+1);
                int next = mp.get(d);
                if (next > cur) {
                    cur = next - cur;
                    i++;
                }
            }
            i++;
            sum += cur;
        }
        return sum;
    }
}
```

#### [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        // dp[n][2]
        // int n = prices.length;
        // int dp0 = 0, dp1 = -prices[0];
        // for (int i = 1; i < n; i++) {
        //     int tmp = dp0;
        //     dp0 = Math.max(dp0, dp1 + prices[i]);
        //     dp1 = Math.max(dp1, tmp - prices[i]);
        // }
        // return dp0;
        // greed
        int ans = 0;
        for (int i = 1; i < prices.length; i++) {
            ans += Math.max(0, prices[i] - prices[i-1]);
        }
        return ans;
    }
}
```

#### [54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return new ArrayList<Integer>();
        }
        int m = matrix.length;
        int n = matrix[0].length;
        boolean[][] visited = new boolean[m][n];
        int[] dx = new int[]{0, 1, 0, -1};
        int[] dy = new int[]{1, 0, -1, 0};
        int k = 0;
        int x = 0, y = 0;
        List<Integer> res = new ArrayList<>();
        while (true) {
            if (x < 0 || y < 0 || x == m || y == n || visited[x][y]) break;
            res.add(matrix[x][y]);
            visited[x][y] = true;
            int nx = x + dx[k], ny = y + dy[k];
            if (nx < 0 || ny < 0 || nx == m || ny == n || visited[nx][ny]) {
                k = (k+1) % 4;
                nx = x + dx[k];
                ny = y + dy[k];
            }
            x = nx;
            y = ny;
        }
        return res;
    }
}
```

#### [36. 有效的数独](https://leetcode.cn/problems/valid-sudoku/)

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        // check rows and columns
        for (int i = 0; i < 9; i++) {
            Set<Character> row = new HashSet<>();
            Set<Character> column = new HashSet<>();
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    if (!row.add(board[i][j])) return false;
                }
                if (board[j][i] != '.') {
                    if (!column.add(board[j][i])) return false;
                }
            }
            row.clear();
            column.clear();
        }
        // cherck 3*3 areas
        for (int i = 0; i < 9; i += 3) {
            for (int j = 0; j < 9; j += 3) {
                // start (i, j) -> (i+2, j+2)
                if (!checkAreas(board, i, j)) return false;
            }
        }
        return true;
    }
    public boolean checkAreas(char[][] board, int x, int y) {
        Set<Character> record = new HashSet<>();
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (board[x+i][y+j] != '.') {
                    if (!record.add(board[x+i][y+j])) return false;
                }
            }
        }
        return true;
    }
}
```

#### [38. 外观数列](https://leetcode.cn/problems/count-and-say/)

```java
class Solution {
    public String countAndSay(int n) {
        String[] res = new String[n];
        res[0] = "1";
        for (int i = 1; i < n; i++) {
            res[i] = getNext(res[i-1]);
        }
        return res[n-1];
    }
    public String getNext(String pre) {
        StringBuffer bf = new StringBuffer();
        for (int i = 0; i < pre.length(); i++) {
            char c = pre.charAt(i);
            int sum = 1;
            while (i < pre.length() - 1 && c == pre.charAt(i+1)) {
                i++;
                sum++;
            }
            String num = sum + "";
            bf.append(num);
            bf.append(c);
        }
        return bf.toString();
    }
}
```

#### [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] res = new int[n][n];
        boolean[][] visited = new boolean[n][n];
        int[] dx = {0, 1, 0, -1};
        int[] dy = {1, 0, -1, 0};
        int k = 0;
        int x = 0, y = 0;
        int num = 1;
        while (true) {
            if (x < 0 || y < 0 || x == n || y == n || visited[x][y]) break;
            res[x][y] = num++;
            visited[x][y] = true;
            int nx = x + dx[k], ny = y + dy[k];
            if (nx < 0 || ny < 0 || nx == n || ny == n || visited[nx][ny]) {
                k = (k + 1) % 4;
                nx = x + dx[k];
                ny = y + dy[k];
            }
            x = nx;
            y = ny;
        }
        return res;
    }
}
```

#### [61. 旋转链表](https://leetcode.cn/problems/rotate-list/)

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        int n = 0;
        ListNode p = head;
        while (p != null) {
            n++;
            p = p.next;
        }
        if (n == 0 || n == 1) return head;
        p = head;
        k %= n;
        if (k == 0) return head;
        for (int i = 0; i <  n - k - 1; i++) {
            p = p.next;
        }
        ListNode start = p.next;
        p.next = null;
        p = start;
        while (p.next != null) {
            p = p.next;
        }
        p.next = head;
        return start;
    }
}
```

#### [230. 二叉搜索树中第K小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        // inorderTraverse
        // non-recursive
        int cnt = 0;
        Deque<TreeNode> stk = new ArrayDeque<>();
        while (!stk.isEmpty() || root != null) {
            while (root != null) {
                stk.push(root);
                root = root.left;
            }
            cnt++;
            root = stk.pop();
            if (cnt == k) {
                return root.val;
            }
            root = root.right;
        }
        return -1;
    }
}
```

#### [16. 最接近的三数之和](https://leetcode.cn/problems/3sum-closest/)

排序+双指针

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int res = 0x3f3f3f3f;
        int n = nums.length;
        quickSort(nums, 0, n-1);
        // Arrays.sort(nums);
        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i-1]) continue;
            int j = i + 1, k = n - 1;
            while (j < k) {
                int num = nums[i] + nums[j] + nums[k];
                if (num == target) return target;
                if (Math.abs(num - target) < Math.abs(res - target)) {
                    res = num;
                }
                if (num > target) {
                    int nk = k - 1;
                    while (j < nk && nums[nk] == nums[nk+1]) nk--;
                    k = nk;
                } else {
                    int nj = j + 1;
                    while (nj < k && nums[nj] == nums[nj-1]) nj++;
                    j = nj;
                }
            }
        }
        return res;
    }
    public void quickSort(int[] nums, int l, int r) {
        if (l >= r) return;
        int loc = partition(nums, l, r);
        quickSort(nums, l, loc-1);
        quickSort(nums, loc+1, r);
    }
    public int partition(int[] nums, int l, int r) {
        // int loc = l + (int)(Math.random() * (r-l+1));
        // int tmp = nums[loc];
        // nums[loc] = nums[l];
        // nums[l] = tmp;
        int key = nums[l];
        int i = l, j = r;
        while (i < j) {
            while (i < j && nums[j] >= key) j--;
            nums[i] = nums[j];
            while (i < j && nums[i] <= key) i++;
            nums[j] = nums[i];
        }
        nums[i] = key;
        return i;
    }
}
```

#### [43. 字符串相乘](https://leetcode.cn/problems/multiply-strings/)

```java
class Solution {
    public String multiply(String num1, String num2) {
        // 1234 * 567
        // 4 * 7 = 28, 28 % 10 = 8, 28 / 10 = 2
        if (num1.equals("0") || num2.equals("0")) return "0";
        List<String> nums = new ArrayList<>();
        int k = 0; // 尾部0个数
        int n1 = num1.length(), n2 = num2.length();
        int accu = 0;
        for (int i = n1-1; i >= 0; i--) {
            StringBuffer bf = new StringBuffer();
            char c1 = num1.charAt(i);
            for (int j = n2-1; j >= 0; j--) {
                char c2 = num2.charAt(j);
                int x = c1 - '0';
                int y = c2 - '0';
                x = x * y + accu;
                int last = x % 10;
                accu = x / 10;
                bf.append(last + "");
            }
            if (accu != 0) {
                bf.append(accu + "");
                accu = 0;
            }
            bf = bf.reverse();
            for (int j = 0; j < k; j++) {
                bf.append("0");
            }
            String cur = bf.toString();
            nums.add(cur);
            k++;
        }
        // sum
        StringBuffer ans = new StringBuffer("0");
        for (int i = 0; i < nums.size(); i++) {
            String cur = stringSum(nums.get(i), ans.toString());
            ans = new StringBuffer(cur);
        }
        return ans.toString();
    }
    public String stringSum(String num1, String num2) {
        int n1 = num1.length();
        int n2 = num2.length();
        int i = 0, j = 0;
        int accu = 0;
        int last = 0;
        StringBuffer bf = new StringBuffer();
        while (i < n1 && j < n2) {
            char c1 = num1.charAt(n1-1-i);
            char c2 = num2.charAt(n2-1-j);
            int x = c1 - '0';
            int y = c2 - '0';
            x = x + y + accu;
            last = x % 10;
            accu = x / 10;
            bf.append(last + "");
            i++;
            j++;
        }
        while (i < n1) {
            char c1 = num1.charAt(n1-1-i);
            int x = c1 - '0';
            x = x + accu;
            last = x % 10;
            accu = x / 10;
            bf.append(last + "");
            i++;
        }
        while (j < n2) {
            char c2 = num2.charAt(n2-1-j);
            int x = c2 - '0';
            x = x + accu;
            last = x % 10;
            accu = x / 10;
            bf.append(last + "");
            j++;
        }
        if (accu != 0) {
            bf.append(accu + "");
        }
        bf = bf.reverse();
        return bf.toString();
    }
}
```

#### [6127. 优质数对的数目](https://leetcode.cn/problems/number-of-excellent-pairs/)

```java
class Solution {
    public long countExcellentPairs(int[] nums, int k) {
        HashMap<Integer, Set<Integer>> record = new HashMap<>();
        for (int num: nums) {
            int cnt = Integer.bitCount(num);
            Set<Integer> tmp = record.putIfAbsent(cnt, new HashSet<Integer>());
            record.get(cnt).add(num);
        }
        long res = 0L;
        for (Map.Entry<Integer, Set<Integer>> entry1: record.entrySet()) {
            for (Map.Entry<Integer, Set<Integer>> entry2: record.entrySet()) {
                int cnt1 = entry1.getKey();
                int cnt2 = entry2.getKey();
                if (cnt1 + cnt2 >= k) {
                    int val1 = entry1.getValue().size();
                    int val2 = entry2.getValue().size();
                    res += val1 * val2;
                }
            }
        }
        return res;
    }
}
```

#### [6126. 设计食物评分系统](https://leetcode.cn/problems/design-a-food-rating-system/)

```java
class Solution {
    public long countExcellentPairs(int[] nums, int k) {
        HashMap<Integer, Set<Integer>> record = new HashMap<>();
        for (int num: nums) {
            int cnt = Integer.bitCount(num);
            Set<Integer> tmp = record.putIfAbsent(cnt, new HashSet<Integer>());
            record.get(cnt).add(num);
        }
        long res = 0L;
        for (Map.Entry<Integer, Set<Integer>> entry1: record.entrySet()) {
            for (Map.Entry<Integer, Set<Integer>> entry2: record.entrySet()) {
                int cnt1 = entry1.getKey();
                int cnt2 = entry2.getKey();
                if (cnt1 + cnt2 >= k) {
                    int val1 = entry1.getValue().size();
                    int val2 = entry2.getValue().size();
                    res += val1 * val2;
                }
            }
        }
        return res;
    }
}
```

#### [18. 四数之和](https://leetcode.cn/problems/4sum/)

```java
class Solution {
	public List<List<Integer>> fourSum(int[] nums, int target) {
		List<List<Integer>> res = new ArrayList<>();
		if (nums == null) return res;
		if (nums.length < 4) return res;
		Arrays.sort(nums);
		int n = nums.length;
		for (int i = 0; i < n - 3; i++) {
			if (i > 0 && nums[i] == nums[i-1]) continue;
			long prev = (long) nums[i] + nums[i+1] + nums[i+2] + nums[i+3];
			long last = (long) nums[i] + nums[n-3] + nums[n-2] + nums[n-1];
			if (prev > target) break;
			if (last < target) continue;
			for (int j = i + 1; j < n - 2; j++) {
				if (j > i + 1 && nums[j] == nums[j-1]) continue;
				long prevJ = (long) nums[i] + nums[j] + nums[j+1] + nums[j+2];
				long lastJ = (long) nums[i] + nums[j] + nums[n-2] + nums[n-1];
				if (prevJ > target) break;
				if (lastJ < target) continue;
				int l = j + 1, r = n - 1;
				while (l < r) {
					int sum = nums[i] + nums[j] + nums[l] + nums[r];
					if (sum == target) {
						res.add(Arrays.asList(nums[i], nums[j], nums[l], nums[r]));
						while (l < r && nums[l] == nums[l+1]) l++;
						l++;
						while (l < r && nums[r] == nums[r-1]) r--;
						r--;
					} else if (sum < target) {
						l++;
					} else {
						r--;
					}
				}
			}
		}
		return res;
	}
}
```

#### [15. 三数之和](https://leetcode.cn/problems/3sum/)

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        if (nums == null || nums.length < 3) return res;
        int n = nums.length;
        Arrays.sort(nums);
        for (int i = 0; i < n - 2; i++) {
            if (i > 0 && nums[i] == nums[i-1]) continue;
            long prev = (long) nums[i] + nums[i+1] + nums[i+2];
            long last = (long) nums[i] + nums[n-2] + nums[n-1];
            if (prev > 0) break;
            if (last < 0) continue;
            int l = i + 1, r = n - 1;
            while (l < r) {
                int sum = nums[i] + nums[l] + nums[r];
                if (sum == 0) {
                    res.add(Arrays.asList(nums[i], nums[l], nums[r]));
                    while (l < r && nums[l] == nums[l+1]) l++;
                    l++;
                    while (l < r && nums[r] == nums[r-1]) r--;
                    r--;
                } else if (sum < 0) {
                    l++;
                } else {
                    r--;
                }
            }
        }
        return res;
    }
}
```

#### [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

* 哈希+分组（AB， CD）

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        // sort 4 nums
        // num1 + num2 = val
        // map
        Map<Integer, Integer> sumAB = new HashMap<>();
        for (int a: nums1) {
            for (int b: nums2) {
                sumAB.put(a+b, sumAB.getOrDefault(a+b, 0) + 1);
            }
        }
        int ans = 0;
        for (int c: nums3) {
            for (int d: nums4) {
                if (sumAB.containsKey(-c-d)) {
                    ans += sumAB.get(-c-d);
                }
            }
        }
        return ans;
    }
}
```



#### [207. 课程表](https://leetcode.cn/problems/course-schedule/)

拓扑排序

```java
class Solution {
    List<List<Integer>> edges;
    int[] visited;
    boolean valid = true;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        edges = new ArrayList<>();
        visited = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            edges.add(new ArrayList<>());
        }
        for (int[] edge: prerequisites) {
            int u = edge[1];
            int v = edge[0];
            edges.get(u).add(v);
        }
        for (int i = 0; i < numCourses && valid; i++) {
            if (visited[i] == 0) {
                dfs(i);
            }
        }
        return valid;
    }
    public void dfs(int u) {
        visited[u] = 1;
        for (int v: edges.get(u)) {
            if (visited[v] == 1) {
                valid = false;
                return;
            } else if (visited[v] == 0) {
                dfs(v);
            }
        }
        visited[u] = 2;
    }
}
```

#### [210. 课程表 II](https://leetcode.cn/problems/course-schedule-ii/)

拓扑排序：dfs

```java
class Solution {
    List<List<Integer>> edges;
    int[] results;
    int[] visited;
    boolean valid = true;
    int last;

    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // dfs
        int n = numCourses;
        edges = new ArrayList<>();
        results = new int[n];
        visited = new int[n];
        last = n - 1;
        for (int i = 0; i < n; i++) {
            edges.add(new ArrayList<>());
        }
        for (int[] edge: prerequisites) {
            int u = edge[1];
            int v = edge[0];
            edges.get(u).add(v);
        }
        for (int i = 0; i < n && valid; i++) {
            if (visited[i] == 0) {
                dfs(i);
            }
        }
        if (!valid) {
            return new int[0];
        }
        return results;
    }
    public void dfs(int u) {
        visited[u] = 1;
        for (int v: edges.get(u)) {
            if (visited[v] == 1) {
                valid = false;
                return;
            } else if (visited[v] == 0) {
                dfs(v);
            }
        }
        visited[u] = 2;
        results[last--] = u;
    }
}
```

拓扑排序：bfs

```java
class Solution {
    List<List<Integer>> edges;
    int[] in;
    int[] results;

    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int n = numCourses;
        edges = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            edges.add(new ArrayList<>());
        }
        in = new int[n];
        results = new int[n];
        for (int[] edge: prerequisites) {
            int u = edge[1];
            int v = edge[0];
            edges.get(u).add(v);
            in[v]++;
        }
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (in[i] == 0) q.offer(i);
        }
        int k = 0;
        while (!q.isEmpty()) {
            int u = q.poll();
            results[k++] = u;
            for (int v: edges.get(u)) {
                in[v]--;
                if (in[v] == 0) q.offer(v);
            }
        }
        if (k == n) return results;
        else return new int[0];
    }
}
```

#### [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

DP

```java

class Solution {
    public boolean canPartition(int[] nums) {
        int n = nums.length;
        if (n == 1) return false;
        int sum = 0;
        int maxi = 0;
        for (int num: nums) {
            sum += num;
            maxi = Math.max(maxi, num);
        }
        if (sum % 2 != 0) return false;
        int target = sum / 2;
        if (target < maxi) return false;
        boolean[][] dp = new boolean[n][target+1];
        dp[0][nums[0]] = true;
        for (int i = 1; i < n; i++) {
            for (int j = 0; j <= target; j++) {
                if (j >= nums[i]) {
                    dp[i][j] = dp[i-1][j] | dp[i-1][j-nums[i]];
                } else {
                    dp[i][j] = dp[i-1][j];
                }
            }
        }
        return dp[n-1][target];
    }
}
```

#### [23. 合并K个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

```java
class Solution {
    class Node implements Comparable<Node> {
        int val;
        ListNode ptr;
        Node(int val, ListNode ptr) {
            this.val = val;
            this.ptr = ptr;
        }
        public int compareTo(Node node) {
            return this.val - node.val;
        }
    }

    PriorityQueue<Node> pq = new PriorityQueue<>();

    public ListNode mergeKLists(ListNode[] lists) {
        for (ListNode node: lists) {
            if (node != null) {
                pq.offer(new Node(node.val, node));
            }
        }
        ListNode head = new ListNode();
        ListNode tail = head;
        while (!pq.isEmpty()) {
            Node node = pq.poll();
            tail.next = node.ptr;
            tail = tail.next;
            ListNode next = node.ptr.next;
            if (next != null) {
                pq.offer(new Node(next.val, next));
            }
        }
        return head.next;
    }
}
```

#### [350. 两个数组的交集 II](https://leetcode.cn/problems/intersection-of-two-arrays-ii/)

* 双指针
* hash

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        int n1 = nums1.length;
        int n2 = nums2.length;
        List<Integer> res = new ArrayList<>();
        Map<Integer, Integer> cntNums1 = new HashMap<>();
        Map<Integer, Integer> cntNums2 = new HashMap<>();
        for (int num: nums1) {
            cntNums1.put(num, cntNums1.getOrDefault(num, 0) + 1);
        }
        for (int num: nums2) {
            cntNums2.put(num, cntNums2.getOrDefault(num, 0) + 1);
        }
        for (Map.Entry<Integer, Integer> entry: cntNums1.entrySet()) {
            int key = entry.getKey();
            int val = entry.getValue();
            if (cntNums2.containsKey(key)) {
                int k = Math.min(val, cntNums2.get(key));
                for (int i = 0; i < k; i++) {
                    res.add(key);
                }
            }
        }
        int[] ans = res.stream().mapToInt(Integer::valueOf).toArray();
        return ans;
    }
}
```

#### [371. 两整数之和](https://leetcode.cn/problems/sum-of-two-integers/)

```java
class Solution {
    public int getSum(int a, int b) {
        while (b != 0) {
            int carry = (a & b) << 1;
            a = a ^ b;
            b = carry;
        }
        return a;
    }
}
```

#### [84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)

单调栈，分别记录从左开始的最小高度和从右开始的最小高度；如果栈为空，使用哨兵（-1，n）记录。

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Deque<Integer> monoStack = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            while (!monoStack.isEmpty() && heights[monoStack.peek()] >= heights[i]) {
                monoStack.pop();
            }
            left[i] = monoStack.isEmpty() ? -1 : monoStack.peek();
            monoStack.push(i);
        }
        monoStack.clear();
        for (int i = n-1; i >= 0; i--) {
            while (!monoStack.isEmpty() && heights[monoStack.peek()] >= heights[i]) {
                monoStack.pop();
            }
            right[i] = monoStack.isEmpty() ? n : monoStack.peek();
            monoStack.push(i);
        }
        int res = 0;
        for (int i = 0; i < n; i++) {
            res = Math.max(res, (right[i] - left[i] - 1) * heights[i]);
        }
        return res;
    }
}
```

#### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

2022.09.10

1. 优先队列

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> {
            if (a[0] != b[0]) {
                return b[0] - a[0];
            }
            return b[1] - a[1];
        });
        for (int i = 0; i < k; i++) {
            pq.offer(new int[] {nums[i], i});
        }
        int[] res = new int[n-k+1];
        res[0] = pq.peek()[0];
        for (int i = k; i < n; i++) {
            pq.offer(new int[] {nums[i], i});
            while (pq.peek()[1] <= i - k) {
                pq.poll();
            }
            res[i-k+1] = pq.peek()[0];
        }
        return res;
    }
}
```

2. 单调队列

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        Deque<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < k; i++) {
            while (!queue.isEmpty() && nums[i] >= nums[queue.peekLast()]) {
                queue.pollLast();
            }
            queue.offerLast(i);
        }
        int[] res = new int[n-k+1];
        res[0] = nums[queue.peekFirst()];
        for (int i = k; i < n; i++) {
            while (!queue.isEmpty() && nums[i] >= nums[queue.peekLast()]) {
                queue.pollLast();
            }
            queue.offerLast(i);
            while (queue.peekFirst() <= i - k) {
                queue.pollFirst();
            }
            res[i-k+1] = nums[queue.peekFirst()];
        }
        return res;
    }
}
```

