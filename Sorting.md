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
