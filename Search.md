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
## 74. Search a 2D Matrix
方法一：先定位数组，再进行二分查找 \
时间复杂度O(logn)，空间复杂度O(1)
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int colSize = matrix[0].size() - 1;
        int row = 0;
        
        while (row < matrix.size()) {
            if (target >= matrix[row][0] && target <= matrix[row][colSize]) break;
            row++;
        }
        
        if (row == matrix.size()) return false;
        
        int first = 0;
        int last = colSize + 1;
        int mid = 0;
        
        while (first < last) {
            mid = first + (last - first) / 2;
            
            if (target < matrix[row][mid]) last = mid;
            else if (target > matrix[row][mid]) first = mid + 1;
            else break;
        }
        
        return first == last ? false : true;
    }
};
```
方法二：直接进行二分查找 ‼️ \
时间复杂度O(logn)，空间复杂度O(1)
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty()) return false;
        const size_t  m = matrix.size();
        const size_t n = matrix.front().size();
        int first = 0;
        int last = m * n;
        while (first < last) {
            int mid = first + (last - first) / 2;
            int value = matrix[mid / n][mid % n];
            if (value == target)
                return true;
            else if (value < target)
                first = mid + 1;
            else
                last = mid;
        }
        return false;
    }
};
```
