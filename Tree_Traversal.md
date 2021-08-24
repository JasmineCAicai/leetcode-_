# 二叉树的遍历
## 144. Binary Tree Preorder Traversal
此题使用迭代版更合适 \
迭代版：时间复杂度O(n)，空间复杂度O(n)
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        stack<TreeNode*> nodes;
        if (root) nodes.push(root);
        while (!nodes.empty()) {
            TreeNode* tmp = nodes.top();
            nodes.pop();
            ans.push_back(tmp->val);
            if (tmp->right) nodes.push(tmp->right);
            if (tmp->left) nodes.push(tmp->left);
        }
        return ans;
    }
};
```
