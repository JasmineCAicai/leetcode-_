# leetcode-journey

## 线性表
### 数组
#### 26. Remove Duplicates from Sorted Array
时间复杂度O(n)，空间复杂度O(1)
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        int k = 0;
        for (int i=1; i<nums.size(); i++) {
            if (nums[k] != nums[i]) {
                nums[++k] = nums[i];
            }
        }
        
        return k+1;
    }
};
```
#### 80. Remove Duplicates from Sorted Array II
时间复杂度O(n)，空间复杂度O(1)
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        int r = 1;
        int index = 0;
        
        for (int i=1; i<nums.size(); i++) {
            if (nums[i] == nums[i-1]) r++;
            else {
                if (r==2) nums[++index] = nums[i-1];
                if (r>2) {
                    index += 2;
                    nums[index-1] = nums[i-1];
                }
                else 
                    index++;
                nums[index] = nums[i];
                r = 1;
            }
            if(i == nums.size()-1 && r==2) nums[++index] = nums[i];
            if(i == nums.size()-1 && r>2) nums[++index] = nums[i-1];
        }
        
        return index + 1;
    }
};
```
更简洁的方法：
时间复杂度O(n)，空间复杂度O(1)
```cpp
class Solution {
  public:
      int removeDuplicates(vector<int>& nums) {
          if (nums.size() <= 2) return nums.size();
          int index = 2;
          for (int i = 2; i < nums.size(); i++){
              if (nums[i] != nums[index - 2])
                  nums[index++] = nums[i];
}
          return index;
      }
};
```
