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
## 102. Binary Tree Level Order Traversal
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        vector<int> group;
        
        if (root == nullptr) return ans;
        
        queue<const TreeNode*> cur_nodes;
        queue<const TreeNode*> children;
        
        cur_nodes.push(root);
        
        int level = 0, num = 0;
        
        while (!children.empty() || !cur_nodes.empty()) {
            
            if (cur_nodes.empty()) cur_nodes.swap(children);
            
            if (cur_nodes.empty()) break;
            
            const TreeNode* tmp = cur_nodes.front();
            cur_nodes.pop();
            group.push_back(tmp->val);
            if (cur_nodes.empty()) {
                ans.push_back(group);
                group.clear();
            }
            
            if (tmp->left != nullptr) children.push(tmp->left);
            if (tmp->right != nullptr) children.push(tmp->right);
        }
        
        return ans;
    }
};
```
递归版：时间复杂度O(n)，空间复杂度O(n) 🔅
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
## 107. Binary Tree Level Order Traversal II
在前一题迭代版的基础上reverse即可（递归版同理）
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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> ans;
        vector<int> group;
        
        if (root == nullptr) return ans;
        
        queue<const TreeNode*> cur_nodes;
        queue<const TreeNode*> children;
        
        cur_nodes.push(root);
        
        int level = 0, num = 0;
        
        while (!children.empty() || !cur_nodes.empty()) {
            
            if (cur_nodes.empty()) cur_nodes.swap(children);
            
            if (cur_nodes.empty()) break;
            
            const TreeNode* tmp = cur_nodes.front();
            cur_nodes.pop();
            group.push_back(tmp->val);
            if (cur_nodes.empty()) {
                ans.push_back(group);
                group.clear();
            }
            
            if (tmp->left != nullptr) children.push(tmp->left);
            if (tmp->right != nullptr) children.push(tmp->right);
        }
        
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```
## 103. Binary Tree Zigzag Level Order Traversal
在之前不需reverse的迭代版方法的基础上，改用stack，并判断加入children栈的顺序该 from left to right 还是 from right to left 即可
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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        vector<int> group;
        
        if (root == nullptr) return ans;
        
        stack<const TreeNode*> cur_nodes;
        stack<const TreeNode*> children;
        
        cur_nodes.push(root);
        
        int level = 0, num = 0;
        
        while (!children.empty() || !cur_nodes.empty()) {
            
            if (cur_nodes.empty()) {
                cur_nodes.swap(children);
                level++;
            }
            
            if (cur_nodes.empty()) break;
            
            const TreeNode* tmp = cur_nodes.top();
            cur_nodes.pop();
            group.push_back(tmp->val);
            if (cur_nodes.empty()) {
                ans.push_back(group);
                group.clear();
            }
            
            if (level % 2 == 0) {
                if (tmp->left != nullptr) children.push(tmp->left);
                if (tmp->right != nullptr) children.push(tmp->right);
            }
            else {
                if (tmp->right != nullptr) children.push(tmp->right);
                if (tmp->left != nullptr) children.push(tmp->left);
            }
        }
        
        return ans;
    }
};
```
递归版：用一个 bool 变量判断，每一层结束就翻转一下 🔅
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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        traverse(root, 1, result, true);
        return result;
    }
    
    void traverse(TreeNode *root, size_t level, vector<vector<int>> &result, bool left_to_right) {
        if (!root) return;
        
        if (level > result.size()) result.push_back(vector<int>());
        
        if (left_to_right) result[level-1].push_back(root->val);
        else result[level-1].insert(result[level-1].begin(), root->val);
        
        traverse(root->left, level+1, result, !left_to_right);
        traverse(root->right, level+1, result, !left_to_right);
    }
};
```
## 99. Recover Binary Search Tree ‼️❓❓❓❓
Morris 中序遍历，不需要使用栈，因此空间复杂度为O(1)，可用于这题 \
时间复杂度O(n)，空间复杂度O(1)
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
    void recoverTree(TreeNode* root) {
        pair<TreeNode*, TreeNode*> broken;
        TreeNode* prev = nullptr;
        TreeNode* cur = root;
        
        while (cur != nullptr) {
            if (cur->left == nullptr) {
                detect(broken, prev, cur);
                prev = cur;
                cur = cur->right;
            } else {
                auto node = cur->left;
                while (node->right != nullptr && node->right != cur)
                    node = node->right;
                if (node->right == nullptr) {
                    node->right = cur;
                    //prev = cur; 不能有这句！因为cur还没被访问
                    cur = cur->left;
                } else {
                    detect(broken, prev, cur);
                    node->right = nullptr;
                    prev = cur;
                    cur = cur->right;
                } 
            }
        }
        
        swap(broken.first->val, broken.second->val);
    }
    
    void detect(pair<TreeNode*, TreeNode*>& broken, TreeNode* prev, TreeNode* current) {
        if (prev != nullptr && prev->val > current->val) {
            if (broken.first == nullptr) {
                broken.first = prev;
            } 
            // 不能用 else，例如 {0, 1}，会导致最后 swap 时，second 为 nullptr，
            // 会 Runtime Error
            broken.second = current;
        } 
    }
};
```
