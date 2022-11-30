---
title: OJ判断是否二叉搜索树
date: 2022-03-17 14:58:33
tags: algorithm
categories:
- 计算机
- OJ
---

 错误典型代码：

```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (!root) return true;
        if (!root->left && !root->right) return true;
        if (!root->left)  {
            return root->val < root->right->val && isValidBST(root->right);
        } else if (!root->right) {
            return root->val > root->left->val && isValidBST(root->left);
        } else {
            return root->val < root->right->val && root->val > root->left->val && isValidBST(root->left) && isValidBST(root->right);
        }
    }
};
```

案例：[5,4,6,null,null,3,7]

正确的递归写法：

```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return helper(root, LONG_MIN, LONG_MAX);
    }
    
    bool helper(TreeNode* root, long long lower, long long upper) {
        if (root == nullptr) return true;
        if (root->val <= lower || root->val >= upper) return false;
        return helper(root->left, lower, root->val) && helper(root->right, root->val, upper);
    }
}
```



