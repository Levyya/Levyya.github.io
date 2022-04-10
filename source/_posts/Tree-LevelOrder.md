---
title: Tree-LevelOrder
date: 2022-03-02 20:32:14
tags: Algorithm
---

# Tree-LevelOrder

## BFS

```c++
vector<vector<int> > levelOrder(TreeNode* root) {
    vector<vector<int> > level;
    if (!root) return level;
    vector<int> v;
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        int n = q.size();
        v.clear();
        while (n--) {
            TreeNode *p = q.front(); q.pop();
            v.push_back(p->val);
            if (p->left) q.push(p->left);
            if (p->right) q.push(p->right);
        }
        level.push_back(v);
    }
    return level;
}
```

## DFS

```c++
vector<vector<int> > levelOrder(TreeNode* root) {
    vector<vector<int> > level;
    if (!root) return level;
    dfs(root, 0, level);
    return level;
}

void dfs(TreeNode* root, int l, vector<vector<int> > &level) {
    if (!root) return;
    int h = level.size();
    if (l >= h) level.push_back(vector<int> {});
    level[l].append(root->val);
    dfs(root->left, l + 1, level);
    dfs(root->right, l + 1, level);
}
```

