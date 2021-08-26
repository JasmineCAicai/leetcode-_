# 二叉树的遍历
## 144. Binary Tree Preorder Traversal ‼️‼️‼️
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
## 94. Binary Tree Inorder Traversal ‼️‼️‼️
时间复杂度O(n)，空间复杂度O(n)
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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        stack<const TreeNode*> nodes;
        const TreeNode* p = root;
        
        while (!nodes.empty() || p != nullptr) {
            if (p != nullptr) {
                nodes.push(p);
                p = p->left;
            }
            else {
                p = nodes.top();
                nodes.pop();
                ans.push_back(p->val);
                p = p->right;
            }
        }
        
        return ans;
    }
};
```
## 145. Binary Tree Postorder Traversal
迭代版：用queue更合适，如果用stack，需要处理顺序问题。 \
时间复杂度O(n)，空间复杂度O(1)（？）
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
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        TreeNode* p = root;
        stack<TreeNode*> nodes;
        
        while (!nodes.empty() || p != nullptr) {
            if (p != nullptr) {
                nodes.push(p);
                
                if (p->left != nullptr) p = p->left;
                else p = p->right;
            }
            else {
                p = nodes.top();
                nodes.pop();
                ans.push_back(p->val);
                if (!nodes.empty() && nodes.top()->right != p) p = nodes.top()->right;
                else p = nullptr;
            }
        }
        
        return ans;
    }
};
```
递归版：时间复杂度O(n)，空间复杂度O(n)
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        traverse(root, 1, result);
        return result;
    }
    
    void traverse(TreeNode *root, size_t level, vector<vector<int>> &result) {
        if (!root) return;
        if (level > result.size()) result.push_back(vector<int>());
        result[level-1].push_back(root->val);
        traverse(root->left, level+1, result);
        traverse(root->right, level+1, result);
    }
};
```
