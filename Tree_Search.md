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
