---
title: LeetCode--01
date: 2022-07-12 09:30:15
tags: leetcode
---

#### [8. 字符串转换整数 (atoi)](https://leetcode.cn/problems/string-to-integer-atoi/)

```java
class Solution {
	public int myAtoi(String str) {
		int len = str.length();
		char[] charArray = str.toCharArray();
		int index = 0;
		while (index < len && charArray[index] == ' ') {
			index++;
		}
		if (index == len) {
			return 0;
		}
		int sign = 1;
		char firstChar = charArray[index];
		if (firstChar == '+') {
			index++;
		} else if (firstChar == '-') {
			index++;
			sign = -1;
		}
		int res = 0;
		while (index < len) {
			char curChar = charArray[index];
			if (curChar > '9' || curChar < '0') {
				break;
			}
			int cur = curChar - '0';
			if (res > Integer.MAX_VALUE / 10 || (res == Integer.MAX_VALUE / 10 && cur > Integer.MAX_VALUE % 10)) {
				return Integer.MAX_VALUE;
			}
			if (res < Integer.MIN_VALUE / 10 || (res == Integer.MIN_VALUE / 10 && cur > -1 * (Integer.MIN_VALUE % 10))) {
				return Integer.MIN_VALUE;
			}
			res = res * 10 + cur * sign;
			index++;
		}
		return res;
	}
}
```

#### [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int l = 0, r = n - 1;
        int res = 0;
        int lH = 0, rH = 0;
        while (l <= r) {
            lH = Math.max(lH, height[l]);
            rH = Math.max(rH, height[r]);
            if (height[l] < height[r]) {
                res += lH - height[l];
                l++;
            } else {
                res += rH - height[r];
                r--;
            }
        }
        return res;
    }
}
```

#### [48. 旋转图像](https://leetcode.cn/problems/rotate-image/)

```java
class Solution {
    public void rotate(int[][] matrix) {
        // 上下翻折 + 按主对角线对称交换
        // 1. 上下翻折
        int n = matrix.length;
        for (int i = 0; i < n / 2; i++) {
            for (int j = 0; j < n; j++) {
                swap(matrix, i, j, n-1-i, j);
            }
        }
        // 2. 按主对角线交换
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                swap(matrix, i, j, j, i);
            }
        }
    }
    public void swap(int[][] matrix, int i, int j, int x, int y) {
        int tmp = matrix[i][j];
        matrix[i][j] = matrix[x][y];
        matrix[x][y] = tmp;
    }
}
```

#### [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            char[] array = str.toCharArray();
            Arrays.sort(array);
            String key = new String(array);
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
}
```

#### [114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)

```java
class Solution {
    public void flatten(TreeNode root) {
        TreeNode cur = root;
        while (cur != null) {
            if (cur.left != null) {
                TreeNode next = cur.left;
                TreeNode post = next;
                while (post.right != null) {
                    post = post.right;
                }
                post.right = cur.right;
                cur.left = null;
                cur.right = next;
            }
            cur = cur.right;
        }
    }
}
```

#### [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

```java
class Solution {
    int ans = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) return 0;
        ans = 1;
        height(root);
        // ans = Math.max(ans, hL + hR + 1);
        // diameterOfBinaryTree(root.left);
        // diameterOfBinaryTree(root.right);
        return ans-1;
    }
    public int height(TreeNode root) {
        if (root == null) return 0;
        int hL = height(root.left);
        int hR = height(root.right);
        ans = Math.max(ans, hL + hR + 1);
        return Math.max(hL, hR) + 1;
    }
}
```

#### [538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)

```java
class Solution {
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if (root == null) return null;
        convertBST(root.right);
        sum += root.val;
        root.val = sum;
        convertBST(root.left);
        return root;
    }
}
```

Morris 遍历

```java
class Solution {
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if (root == null) return null;
        int sum = 0;
        TreeNode node = root;
        while (node != null) {
            if (node.right == null) {
                sum += node.val;
                node.val = sum;
                node = node.left;
            } else {
                TreeNode succ = getSuccessor(node);
                if (succ.left == null) {
                    succ.left = node;
                    node = node.right;
                } else {
                    succ.left = null;
                    sum += node.val;
                    node.val = sum;
                    node = node.left;
                }
            }
        }
        return root;
    }
    public TreeNode getSuccessor(TreeNode node) {
        TreeNode succ = node.right;
        while (succ.left != null && succ.left != node) {
            succ = succ.left;
        }
        return succ;
    }
}
```

##### Tips

```
int x = 1;
if (x & 1) // error!
if (x & 1 == 1) // error!
ans += x & 1;
```

#### [448. 找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)

```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        int n = nums.length;
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            int index = Math.abs(nums[i])-1;
            nums[index] = -Math.abs(nums[index]);
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] > 0) res.add(i+1);
        }
        return res;
    }
}
```

#### [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)

```java
class LRUCache {
    class DLinkedNode {
        int val;
        int key;
        DLinkedNode prev;
        DLinkedNode next;
        public DLinkedNode() {};
        public DLinkedNode(int _key, int _val) {
            key = _key;
            val = _val;
        }
    }
    private Map<Integer, DLinkedNode> cache;
    private int capacity;
    private int size;
    private DLinkedNode head;
    private DLinkedNode tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        cache = new HashMap<>();
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        if (cache.containsKey(key)) {
            DLinkedNode node = cache.get(key);
            removeNode(node);
            addHead(node);
            // cache.put(key, node);
            return node.val;
        } else {
            return -1;
        }
    }
    
    public void put(int key, int value) {
        DLinkedNode node = cache.get(key);
        if (node != null) {
            node.val = value;
            moveToHead(node);
        } else {
            node = new DLinkedNode(key, value);
            if (size == capacity) {
                DLinkedNode tail = removeTail();
                cache.remove(tail.key);
                size--;
            }
            addHead(node);
            size++;
        }
        cache.put(key, node);
    }
    public void addHead(DLinkedNode node) {
        head.next.prev = node;
        node.next = head.next;
        head.next = node;
        node.prev = head;
    }
    public void moveToHead(DLinkedNode node) {
        removeNode(node);
        addHead(node);
    }
    public void removeNode(DLinkedNode node) {
        node.next.prev = node.prev;
        node.prev.next = node.next;
    }
    public DLinkedNode removeTail() {
        DLinkedNode prev = tail.prev;
        removeNode(prev);
        return prev;
    }
}
```

#### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        // fast & slow
        if (head == null || head.next == null) return null;
        ListNode fast = head;
        ListNode slow = head;
        do {
            if (fast == null || fast.next == null) {
                return null;
            }
            fast = fast.next.next;
            slow = slow.next;
        } while (fast != slow);
        slow = head;
        while (fast != slow) {
            slow = slow.next;
            fast = fast.next;
        }
        return fast;
    }
}
```

#### [338. 比特位计数](https://leetcode.cn/problems/counting-bits/)

```java
class Solution {
    // public int[] countBits(int n) {
    //     int[] ans = new int[n+1];
    //     for (int i = 0; i <= n; i++) {
    //         int cnt = 0;
    //         int num = i;
    //         while (num > 0) {
    //             cnt++;
    //             num = (num-1) & num;
    //         }
    //         ans[i] = cnt;
    //     }
    //     return ans;
    // }
    public int[] countBits(int n) {
        int[] dp = new int[n+1];
        for (int i = 1; i <= n; i++) {
            // dp[i] = dp[i & (i-1)] + 1;
            dp[i] = dp[i>>1] + (i&1);
        }
        return dp;
    }
}
```

#### [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)

```java
class Solution {
    public int rob(TreeNode root) {
        int[] res = dfs(root);
        return Math.max(res[0], res[1]);
    }
    public int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[]{0, 0};
        }
        int[] left = dfs(root.left);
        int[] right = dfs(root.right);
        int selected = root.val + left[1] + right[1];
        int unselected = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        return new int[]{selected, unselected};
    }
}
```

#### [322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount+1];
        Arrays.fill(dp, 10001);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (int j = 0; j < coins.length; j++) {
                if (coins[j] > i) continue;
                dp[i] = Math.min(dp[i], dp[i-coins[j]] +1 );
            }
        }
        return dp[amount] == 10001 ? -1 : dp[amount];
    }
}
```

#### [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        if (root == null) return 0;
        int ans;
        ans = dfs(root, targetSum);
        ans += pathSum(root.left, targetSum);
        ans += pathSum(root.right, targetSum);
        return ans;
    }
    public int dfs(TreeNode root, int targetSum) {
        int cnt = 0;
        if (root == null) {
            return 0;
        }
        int val = root.val;
        if (val == targetSum) {
            cnt++;
        }
        cnt += dfs(root.left, targetSum-val);
        cnt += dfs(root.right, targetSum-val);
        return cnt;
    }
    // int ans = 0;
    // public int pathSum(TreeNode root, int targetSum) {
    //     dfs(root, targetSum, 0);
    //     return ans;
    // }
    // public void dfs(TreeNode root, int t, int sum) {
    //     if (root == null) return;
    //     if (root.val + sum == t) {
    //         ans++;
    //     }
    //     dfs(root.left, t, sum + root.val);
    //     dfs(root.left, t, 0);
    //     dfs(root.right, t, 0);
    //     dfs(root.right, t, sum + root.val);
    // }
}
```

#### [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        // hash record
        // O(1)
        ListNode pA = headA;
        ListNode pB = headB;
        while (pA != pB) {
            if (pA == null) {
                pA = headB;
            }
            if (pB == null) {
                pB = headA;
            }
            if (pA == pB) return pA;
            pA = pA.next;
            pB = pB.next;
        }
        return pA;
    }
}
```

#### [169. 多数元素](https://leetcode.cn/problems/majority-element/)

```java
class Solution {
    public int majorityElement(int[] nums) {
        int cnt = 1;
        int num = nums[0];
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == num) cnt++;
            else {
                cnt--;
                if (cnt == 0) {
                    cnt = 1;
                    num = nums[i];
                }
            }
        }
        return num;
    }
}
```

#### [309. 最佳买卖股票时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        // dp[3][n+1]: 有股票，无股票（冷冻期），无股票
        // init: 0
        int n = prices.length;
        int[][] dp = new int[3][n+1];
        dp[0][0] = -prices[0];
        for (int i = 1; i < n; i++) {
            dp[0][i] = Math.max(dp[0][i-1], dp[2][i-1] - prices[i]); // 购买
            dp[1][i] = dp[0][i-1] + prices[i]; // 抛出
            dp[2][i] = Math.max(dp[1][i-1], dp[2][i-1]);
        }
        return Math.max(dp[1][n-1], dp[2][n-1]);
    }
}
```

#### [234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        // findHalfNext + reverseListNode
        ListNode next = findHalfNext(head);
        next = reverseListNode(next.next);
        while (next != null) {
            if (head.val != next.val) {
                return false;
            }
            head = head.next;
            next = next.next;
        }
        return true;
    }
    public ListNode findHalfNext(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode fast = head;
        ListNode slow = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
    public ListNode reverseListNode(ListNode node) {
        ListNode prev = new ListNode();
        while (node != null) {
            ListNode next = node.next;
            node.next = prev.next;
            prev.next = node;
            node = next;
        }
        return prev.next;
    }
}
```

#### [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return null;
        if (root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) return root;
        else if (left == null) return right;
        else return left;
    }
}
```

#### [494. 目标和](https://leetcode.cn/problems/target-sum/)

```java
class Solution {
    int cnt = 0;
    public int findTargetSumWays(int[] nums, int target) {
        int n = nums.length;
        int sum = 0;
        for (int num: nums) sum += num;
        int diff = sum - target;
        if (diff < 0 || diff % 2 == 1) return 0;
        int neg = diff >> 1;
        int[][] dp = new int[n+1][neg+1];
        dp[0][0] = 1;
        for (int i = 1; i <= n; i++) {
            int num = nums[i-1];
            for (int j = 0; j <= neg; j++) {
                dp[i][j] = dp[i-1][j];
                if (j >= num) dp[i][j] += dp[i-1][j-num];
            }
        }
        return dp[n][neg];
        // dfs
        // dfs(nums, target, 0, 0);
        // return cnt;
    }
    // public void dfs(int[] nums, int t, int k, int sum) {
    //     if (k == nums.length) {
    //         if (sum == t) {
    //             cnt++;
    //         }
    //     } else {
    //         dfs(nums, t, k + 1, sum + nums[k]);
    //         dfs(nums, t, k + 1, sum - nums[k]);
    //     }
    // }
}
```

