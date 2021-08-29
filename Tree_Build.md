# 二叉树的构建 ‼️‼️❓❓
## 105. Construct Binary Tree from Preorder and Inorder Traversal
递归版：时间复杂度O(n)，空间复杂度O(logn)
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
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return buildTree(begin(preorder), end(preorder), begin(inorder), end(inorder));
    }
    
    template<typename InputIterator>
    
    TreeNode* buildTree(InputIterator prefirst, InputIterator prelast, InputIterator infirst, InputIterator inlast) {
        if (prefirst == prelast) return nullptr;
        if (infirst == inlast) return nullptr;
        
        auto root = new TreeNode(*prefirst);
        
        auto inRootPos = find(infirst, inlast, *prefirst);
        auto leftSize = distance(infirst, inRootPos);
        
        root->left = buildTree(next(prefirst), next(prefirst, leftSize + 1), infirst, next(infirst, leftSize));
        root->right = buildTree(next(prefirst, leftSize + 1), prelast, next(inRootPos), inlast);
        
        return root;
    }
};
```
## 106. Construct Binary Tree from Inorder and Postorder Traversal
递归版：时间复杂度O(n)，空间复杂度O(logn)
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
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        return buildTree(begin(inorder), end(inorder), begin(postorder), end(postorder));
    }
    
    template<typename BidiIt>
    
    TreeNode* buildTree(BidiIt inFirst, BidiIt inLast, BidiIt postFirst, BidiIt postLast) {
        if (inFirst == inLast) return nullptr;
        if (postFirst == postLast) return nullptr;
        
        const auto val = *prev(postLast);
        TreeNode* root = new TreeNode(val);
        
        auto inRootPos = find(inFirst, inLast, val);
        auto leftSize = distance(inFirst, inRootPos);
        auto postLeftLast = next(postFirst, leftSize);
        
        root->left = buildTree(inFirst, inRootPos, postFirst, postLeftLast);
        root->right = buildTree(next(inRootPos), inLast, postLeftLast, prev(postLast));
        
        return root;
    }
};
```
