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
## 108. Convert Sorted Array to Binary Search Tree
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
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return sortedArrayToBST(nums, 0, nums.size());
    }
    
    TreeNode* sortedArrayToBST(vector<int> nums, int first, int last) {
        if (first >= last) return nullptr;
        
        int mid = (last - first) / 2;
        TreeNode* root = new TreeNode(nums[first + mid]);
        root->left = sortedArrayToBST(nums, first, first + mid);
        root->right = sortedArrayToBST(nums, first + mid + 1, last);
        return root;
    }
};
```
## 109. Convert Sorted List to Binary Search Tree ‼️
在上一题的基础上，先将list里的值存入vector
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
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
    TreeNode* sortedListToBST(ListNode* head) {
        if (!head) return nullptr;
        
        vector<int> vals;
        while (head) {
            vals.push_back(head->val);
            head = head->next;
        }
        
        return sortedListToBST(vals, 0, vals.size());
    }
    
    TreeNode* sortedListToBST(vector<int> vals, int first, int last) {
        if (first >= last) return nullptr;
        
        int mid = (last - first) / 2;
        mid += first;
        
        TreeNode* root = new TreeNode(vals[mid]);
        root->left = sortedListToBST(vals, first, mid);
        root->right = sortedListToBST(vals, mid + 1, last);
        
        return root;
    }
};
```
更好的方法：自底向上 \
时间复杂度O(n)，空间复杂度O(logn) \
运行时间比上个方法短很多，内存使用也比上个方法少很多很多
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
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
    TreeNode* sortedListToBST(ListNode* head) {
        int len = 0;
        ListNode *p = head;
        while (p) {
            len++;
            p = p->next; 
        }
        return sortedListToBST(head, 0, len - 1);
    }
    
private:
    TreeNode* sortedListToBST(ListNode*& list, int start, int end) {
        if (start > end) return nullptr;
        int mid = start + (end - start) / 2;
        TreeNode *leftChild = sortedListToBST(list, start, mid - 1);
        TreeNode *parent = new TreeNode(list->val);
        parent->left = leftChild;
        list = list->next;
        parent->right = sortedListToBST(list, mid + 1, end);
        return parent;
    }
};
```
