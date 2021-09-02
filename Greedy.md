# 贪心法
## 55. Jump Game ‼️‼️‼️
方法一：从左往右
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int reach = 1;
        for (int i = 0; i < reach && reach < nums.size(); i++) {
            reach = max(reach, i + nums[i] + 1);
        }
        
        return reach >= nums.size();
    }
};
```
方法二：从右往左（类似于下楼梯，最下能到多少层）
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int left_most = nums.size() - 1;
        for (int i = nums.size() - 2; i >= 0; i--) {
            if (i + nums[i] >= left_most) left_most = i;
        }
        
        return left_most == 0;
    }
};
```
