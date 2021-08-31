# 二叉树的递归
## 111. Minimum Depth of Binary Tree
递归版一
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
    int minDepth(TreeNode* root) {
        if (root == nullptr) return 0;
        
        if (root->left == nullptr) return min(minDepth(root->left) + INT_MAX, minDepth(root->right) + 1);
        if (root->right == nullptr) return min(minDepth(root->left) + 1, minDepth(root->right) + INT_MAX);
        
        return min(minDepth(root->left) + 1, minDepth(root->right) + 1);
    }
};
```
更简洁的递归版二 \
时间复杂度O(n)，空间复杂度O(logn)
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
    int minDepth(TreeNode* root) {
        return minDepth(root, false);
    }
private:
    static int minDepth(const TreeNode *root, bool hasbrother) {
        if (!root) return hasbrother ? INT_MAX : 0;
        return 1 + min(minDepth(root->left, root->right != NULL), minDepth(root->right, root->left != NULL));
    }
};
```
迭代版 \
时间复杂度O(n)，空间复杂度O(logn)
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
    int minDepth(TreeNode* root) {
        if (root == nullptr) return 0;
        int result = INT_MAX;
        stack<pair<TreeNode*, int>> s;
        s.push(make_pair(root, 1));
        while (!s.empty()) {
            auto node = s.top().first;
            auto depth = s.top().second;
            s.pop();
            if (node->left == nullptr && node->right == nullptr) 
                result = min(result, depth);
            if (node->left && result > depth) 
                s.push(make_pair(node->left, depth + 1));
            if (node->right && result > depth) 
                s.push(make_pair(node->right, depth + 1));
        }
        return result;
    }
};
```
## 104. Maximum Depth of Binary Tree
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
    int maxDepth(TreeNode* root) {
        if (root == nullptr) return 0;
        
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
```
## 112. Path Sum ‼️
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
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (root == nullptr) return false;
        if (root->left == nullptr && root->right == nullptr) return targetSum == root->val;
        return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);
    }
};
```
## 113. Path Sum II ‼️‼️‼️
时间复杂度O(n)，空间复杂度O(logn) \
需要group.pop_back() ‼️
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
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<int> group;
        vector<vector<int>> ans;
        pathSum(root, targetSum, group, ans);
        return ans;
    }
private:
    void pathSum(TreeNode* root, int targetSum, vector<int> &group, vector<vector<int>> &ans) {
        if (root == nullptr) return;
        group.push_back(root->val);
        if (root->left == nullptr && root->right == nullptr) {
            if (root->val == targetSum) ans.push_back(group);
        }
        
        pathSum(root->left, targetSum - root->val, group, ans);
        pathSum(root->right, targetSum - root->val, group, ans);
        
        group.pop_back();
    }
};
```
## 124. Binary Tree Maximum Path Sum ‼️‼️‼️‼️‼️
时间复杂度O(n)，空间复杂度O(logn)
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
    int maxPathSum(TreeNode* root) {
        max_sum = INT_MIN;
        dfs(root);
        return max_sum;
    }
    
private:
    int max_sum;
    int dfs(TreeNode* root) {
        if (root == nullptr) return 0;
        int l = dfs(root->left);
        int r = dfs(root->right);
        
        int sum = root->val;
        
        if (l > 0) sum += l;
        if (r > 0) sum += r;
        max_sum = max(max_sum, sum);
        
        return max(r, l) > 0 ? max(r, l) + root->val : root->val;
    }
};
```
