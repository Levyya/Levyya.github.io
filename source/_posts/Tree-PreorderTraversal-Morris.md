---
title: Tree--PreorderTraversal(Morris)
date: 2022-03-02 15:59:07
tags: Algorithm
---

# Tree-PreorderTraversal (Morris)

```c++
/**
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
*/
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (root == nullptr) return vector<int>(0);
        vector<int> preorder;
        TreeNode *cur = root;
        TreeNode *mostRight = nullptr;
        while (cur) {
            mostRight = cur->left;
            if (mostRight) {
                while (mostRight->right != nullptr && mostRight->right != cur) mostRight = mostRight->right;
                if (mostRight->right == nullptr) {
                    preorder.push_back(cur->val);
                    mostRight->right = cur;
                    cur = cur->left;
                    continue;
                } else mostRight->right = nullptr;
            } else {
                preorder.push_back(cur->val);
            }
            cur = cur->right;
        }
        return preorder;
    }
};
```

