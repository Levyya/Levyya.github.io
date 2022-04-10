---
title: Tree-InorderTraversal
date: 2022-03-02 16:48:09
tags: Algorithm
---

# Tree-InorderTraversal

1. 递归

   ```c++
   vector<int> inorderTraversal(TreeNode* root) {
       if (root == nullptr) return vector<int>(0);
       vector<int> inorder;
       inorderHelper(inorder, root);
       return inorder;
   }
   
   void inorderHelper(vector<int> &inorder, TreeNode* root) {
       if (root == nullptr) return;
       inorderHelper(inorder, root->left);
       inorder.push_back(root->val);
       inorderHelper(inorder, root->right);
   }
   ```

   

2. 非递归（栈）

   ```c++
   vector<int> inorderTraversal(TreeNode* root) {
       if (root == nullptr) return vector<int>(0);
       vector<int> inorder;
       stack<int> s;
       while (root || !s.empty()) {
           while (root) {
               s.push(root);
               root = root->left;
           }
           root = s.top(); s.pop();
           inorder.push_back(top->val);
           root = root->right;
       }
       return inorder;
   }
   ```

   

3. 非递归（Morris）

```c++
vector<int> inorderTraversal(TreeNode* root) {
    if (root == nullptr) return vector<int>(0);
    vector<int> inorder;
    TreeNode *cur = root;
    TreeNode *pre;
    while (cur) {
        if (cur->left == nullptr) {
            inorder.push_back(cur->val);
            cur = cur->right;
        } else {
            pre = cur->left;
            while (pre->right && pre->right != cur) pre = pre->right;
            if (pre->right == nullptr) {
                pre->right = cur;
                cur = cur->left;
            } else {
                inorder.push_back(cur->val);
                pre->right = nullptr;
                cur = cur->right;
            }
        }
    }
    return inorder;
}
```

