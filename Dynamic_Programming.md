# 动态规划
## 120. Triangle
时间复杂度O(n^2)，空间复杂度和原数组一样，并没有新增较大空间。
```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        if (triangle.size() == 1) return triangle[0][0];
        
        for (int i = 1; i < triangle.size(); i++) {
            triangle[i][0] += triangle[i-1][0];
        }
        
        for (int i = 1, j = 1; i < triangle.size(); i++) {
            triangle[i][j++] += triangle[i-1][j-1];
        }
        
        for (int i = 2; i < triangle.size(); i++) {
            for (int j = 1; j < triangle[i].size() - 1; j++) {
                triangle[i][j] += min(triangle[i-1][j-1], triangle[i-1][j]);
            }
        }
        
        int ans = INT_MAX;
        for (int i = 0; i < triangle[triangle.size()-1].size(); i++) {
            if (ans > triangle[triangle.size()-1][i]) ans = triangle[triangle.size()-1][i];
        }
        
        return ans;
    }
};
```
