# 排序
## 88. Merge Sorted Array
方法一 \
sort()的时间复杂度为O(nlogn)
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        for (int i = 0; i < n; i++) {
            nums1[i + m] = nums2[i];
        }
        sort(nums1.begin(), nums1.end());
    }
};
```
方法二 \
时间复杂度O(m + n)，空间复杂度O(1)
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int ia = m - 1, ib = n - 1, icur = m + n - 1;
        while(ia >= 0 && ib >= 0) {
            nums1[icur--] = nums1[ia] >= nums2[ib] ? nums1[ia--] : nums2[ib--];
        }
        while(ib >= 0) {
            nums1[icur--] = nums2[ib--];
        }
    }
};
```
## 21. Merge Two Sorted Lists
时间复杂度O(min(m,n))，空间复杂度O(1)
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
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode ans(-1);
        ListNode* res = &ans;
        while (l1 && l2) {
            if (l1->val > l2->val) {
                res->next = l2;
                l2 = l2->next;
            }
            else {
                res->next = l1;
                l1 = l1->next;
            }
            res = res->next;
        }
        res->next = l1 ? l1 : l2;
        return ans.next;
    }
};
```
