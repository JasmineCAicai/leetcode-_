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
## 41. First Missing Positive
```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int i = 0;
        sort(nums.begin(), nums.end());
        unique(nums.begin(), nums.end());
        while (i < nums.size() && nums[i] <= 0) {
            i++;
        }
        
        int positive = 1;
        while (i < nums.size()) {
            if (nums[i] != positive) break;
            i++;
            positive++;
        }
        
        return positive;
    }
};
```
## 75. Sort Colors
捷径版不符合规则
```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        sort(nums.begin(), nums.end());
    }
};
```
符合规则版
```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int pos;
        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] < nums[i-1]) {
                pos = findPos(nums, i, nums[i]);
                int j = i;
                int tmp = nums[i];
                while (j > pos) {
                    nums[j] = nums[j-1];
                    j--;
                }
                nums[pos] = tmp;
            }
        }
    }
private:
    int findPos(vector<int>& nums, int last, int val) {
        int i = 0;
        for (; i < last; i++) {
            if (val < nums[i]) break;
        }
        return i;
    }
};
```
特别的方法 \
时间复杂度O(n)，空间复杂度O(1)
```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int count[3] = {0};
        for (int i = 0; i < nums.size(); i++) {
            count[nums[i]]++;
        }
        for (int i = 0, j = -1; i < 3; i++) {
            while (count[i] > 0) {
                nums[++j] = i;
                count[i]--;
            }
        }
    }
};
```
