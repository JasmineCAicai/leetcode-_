# 查找
## 35. Search Insert Position
时间复杂度O(logn)，空间复杂度O(1)
```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int last = nums.size();
        int mid = 0;
        int first = 0;
        
        while (first < last) {
            mid = first + (last - first) / 2;
            
            if (target < nums[mid]) last = mid;
            else if (target > nums[mid]) first = mid + 1;
            else break;
        }
        
        return first == last ? first : mid;
    }
};
```
