# 二叉查找树
## 96. Unique Binary Search Trees
时间复杂度O(n^2)（？），空间复杂度O(n)
```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<int> bases;
        bases.push_back(1);
        bases.push_back(1);
        bases.push_back(2);
        
        for (int i = 3; i <= n; i++) {
            bases.push_back(0);
            for (int j = 0; j <= i-1; j++) {
                bases[i] += (bases[j] * bases[i-1-j]);
            }
        }
        
        return bases[n];
    }
};
```
## 98. Validate Binary Search Tree
中序遍历递归版 \
运行时间太长，且内存使用太多。
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
    queue<int> inOrder(TreeNode* root, queue<int> vals) {
        if (root == nullptr) return vals;
        
        vals = inOrder(root->left, vals);
        vals.push(root->val);
        vals = inOrder(root->right, vals);
        return vals;
    }
    
    bool isValidBST(TreeNode* root) {
        queue<int> vals = inOrder(root,queue<int> {});
        
        int prev;
        prev = vals.front();
        vals.pop();
        while (!vals.empty()) {
            if (prev < vals.front()) {
                prev = vals.front();
                vals.pop();
            }
            else break;
        }
        
        return vals.empty();
    }
};
```
更好的方法：‼️‼️ \
注意上下限 \
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
    bool isValidBST(TreeNode* root, long long lower, long long upper) {
        if (root == nullptr) return true;
        return root->val > lower && root->val < upper 
            && isValidBST(root->left, lower, root->val) 
            && isValidBST(root->right, root->val, upper);
    }
    
    bool isValidBST(TreeNode* root) {
        return isValidBST(root, LLONG_MIN, LLONG_MAX);
    }
};
```
