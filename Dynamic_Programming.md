# 动态规划
## 120. Triangle
从上往下法：时间复杂度O(n^2)，空间复杂度O(1)
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
更简洁的方法：‼️ \
从下往上法：时间复杂度O(n^2)，空间复杂度O(1)
```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        for (int i = triangle.size() - 2; i >= 0; i--) {
            for (int j = 0; j < triangle[i].size(); j++) {
                triangle[i][j] += min(triangle[i+1][j], triangle[i+1][j+1]);
            }
        }
        return triangle[0][0];
    }
};
```
## 53. Maximum Subarray
方法一：时间复杂度O(n)，空间复杂度O(n)
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.size() == 1) return nums[0];
        vector<int> ans;
        ans.push_back(nums[0]);
        int result = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            ans.push_back(max(nums[i], ans[i-1] + nums[i]));
            result = result < ans[i] ? ans[i] : result;
        }
        
        return result;
    }
};
```
方法二：时间复杂度O(n)，空间复杂度O(1) ‼️
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int prev = nums[0], result = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            prev = max(nums[i], prev + nums[i]);
            result = prev > result ? prev : result;
        }
        
        return result;
    }
};
```
## 132. Palindrome Partitioning II ‼️⁉️⁉️‼️
需要两次动态规划 \
时间复杂度O(n^2)，空间复杂度O(n^2)
```cpp
class Solution {
public:
    int minCut(string s) {
        if (s.size() <= 1) return 0;
        
        const int n = s.size();
        int f[n+1];
        bool p[n][n];
        fill_n(&p[0][0], n * n, false);
        for (int i = 0; i <= n; i++) {
            f[i] = n - 1 - i;
        }
        
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (s[i] == s[j] && ((j - i < 2) || p[i+1][j-1])) {
                    p[i][j] = true;
                    f[i] = min(f[i], f[j+1] + 1);
                }
            }
        }
    
        return f[0];
    }
};
```
## 64. Minimum Path Sum
时间复杂度O(n^2)，空间复杂度O(1)
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int row = grid.size();
        int col = grid[0].size();
        for (int i = 1; i < row; i++) {
            grid[i][0] += grid[i-1][0];
        }
        
        for (int i = 1; i < col; i++) {
            grid[0][i] += grid[0][i-1];
        }
        
        for (int i = 1; i < row; i++) {
            for (int j = 1; j < col; j++) {
                grid[i][j] += min(grid[i-1][j], grid[i][j-1]);
            }
        }
        
        return grid[row-1][col-1];
    }
};
```
## 91. Decode Ways ‼️
时间复杂度O(n)，空间复杂度O(1)
```cpp
class Solution {
public:
    int numDecodings(string s) {
        if (s[0] == '0') return 0;
        
        int prev = 0, cur = 1, tmp = 0;
        for (int i = 1; i <= s.size(); i++) {
            if (s[i-1] == '0') cur = 0;
            if (i < 2 || !(s[i-2] == '1' || (s[i-2] == '2' && s[i-1] <= '6'))) prev = 0;
            tmp = cur;
            cur += prev;
            prev = tmp;
        }
        
        return cur;
    }
};
```
## 118. Pascal's Triangle
```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans;
        for (int i = 1; i <= numRows; i++) {
            ans.push_back(vector<int>(i,1));
        }
        
        for (int i = 2; i < numRows; i++) {
            for (int j = 1; j < ans[i].size()-1; j++) {
                ans[i][j] = ans[i-1][j-1] + ans[i-1][j];
            }
        }
        
        return ans;
    }
};
```
## 62. Unique Paths
方法一：时间复杂度O(m * n)，空间复杂度O(m * n)
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        int arr[m][n];
        for (int i = 0; i < n; i++) {
            arr[0][i] = 1;
        }
        for (int i = 0; i < m; i++) {
            arr[i][0] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                arr[i][j] = arr[i-1][j] + arr[i][j-1];
            }
        }
        return arr[m-1][n-1];
    }
};
```
方法二：时间复杂度O(m * n)，空间复杂度O(n)
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        if (m == 1 || n == 1) return 1;
        
        int arr[n], up[n-1];
        arr[0] = 1;
        
        for (int i = 0; i < n-1; i++) {
            up[i] = 1;
        }
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                arr[j] = arr[j-1] + up[j-1];
                up[j-1] = arr[j];
            }
        }
        return arr[n-1];
    }
};
```
## 63. Unique Paths II
时间复杂度O(m * n)，空间复杂度O(m * n)
```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        
        if (obstacleGrid[m-1][n-1] == 1 || obstacleGrid[0][0] == 1) return 0;
        
        int arr[m][n];
        
        int flag = false;
        for (int i = 0; i < n; i++) {
            if (obstacleGrid[0][i] == 1) flag = true;
            arr[0][i] = flag ? 0 : 1;
        }
        
        flag = false;
        for (int i = 0; i < m; i++) {
            if (obstacleGrid[i][0] == 1) flag = true;
            arr[i][0] = flag ? 0 : 1;
        }
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                arr[i][j] = 0;
                if (obstacleGrid[i-1][j] != 1) arr[i][j] += arr[i-1][j];
                if (obstacleGrid[i][j-1] != 1) arr[i][j] += arr[i][j-1];
            }
        }
        
        return arr[m-1][n-1];
    }
};
```
