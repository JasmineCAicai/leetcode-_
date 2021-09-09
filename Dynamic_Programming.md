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
## 5. Longest Palindromic Substring ‼️‼️‼️
时间复杂度O(n^2)，空间复杂度O(1)
```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.size() == 1) return s;
        string ans, str1, str2;
        int i = 0;
        while (i < s.size()) {
            str1 = palindrome(s, i, i);
            str2 = palindrome(s, i, i + 1);
            ans = str1.size() > ans.size() ? str1 : ans;
            ans = str2.size() > ans.size() ? str2 : ans;
            i++;
        }
        return ans;
    }
    
private:
    string palindrome(string s, int l, int r) {
        while (l >= 0 && r < s.size() && s[l] == s[r]) {
            l--;
            r++;
        }
        return s.substr(l + 1, r - l - 1);
    }
};
```
## 22. Generate Parentheses
```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        for (int i = 0; i < n; i++) {
            left.push('(');
            right.push(')');
        }
        generateParenthesis("");
        return ans;
    }
    
private:
    stack<char> left, right;
    vector<string> ans;
    void generateParenthesis(string s) {
        if (left.empty() && right.empty()) {
            ans.push_back(s);
            return;
        }
        
        if (!left.empty()) {
            char l = left.top();
            left.pop();
            generateParenthesis(s + l);
            left.push(l);
        }
        if (!right.empty() && left.size() < right.size()) {
            char r = right.top();
            right.pop();
            generateParenthesis(s + r);
            right.push(r);
        }
    }
};
```
## 72. Edit Distance ‼️‼️‼️
时间复杂度O(m * n)，空间复杂度O(m * n)
```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size() + 1;
        int n = word2.size() + 1;
        int table[m][n];
        
        for (int i = 0; i < m; i++) {
            table[i][0] = i;
        }
        for (int j = 0; j < n; j++) {
            table[0][j] = j;
        }
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (word1[i-1] == word2[j-1]) table[i][j] = table[i-1][j-1];
                else table[i][j] = min(table[i-1][j] + 1, min(table[i-1][j-1] + 1, table[i][j-1] + 1));
            }
        }
        
        return table[m-1][n-1];
    }
};
```
## 887. Super Egg Drop ‼️‼️
这个方法有时候会超时 \
时间复杂度O(k * n * logn)，空间复杂度O(k * n)
```cpp
class Solution {
public:
    int superEggDrop(int k, int n) {
        return calc(k,n);
    }
private:
    map<pair<int,int>,int> table;
    int calc(int k, int n) {
        if (k == 1) return n;
        if (n == 0) return 0;
        if (table.find({k,n}) != table.end()) return table[make_pair(k,n)];
        
        int res = INT_MAX;
        int lo = 1, hi = n, mid, broken, not_broken;
        
        while (lo <= hi) {
            mid = (lo + hi) / 2;
            
            broken = calc(k-1, mid - 1);
            not_broken = calc(k, n - mid);
            
            if (broken > not_broken) {
                hi = mid - 1;
                res = min(res, broken + 1);
            }
            else {
                lo = mid + 1;
                res = min(res, not_broken + 1);
            }
        }
        
        table.insert(make_pair(make_pair(k,n),res));
        return res;
    }
};
```
## 45. Jump Game II ‼️‼️‼️‼️‼️
```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size() - 1, i = 0, pre = 0, cur = 0;
        int steps = 0;
        while (cur < n) {
            steps++;
            pre = cur;
            for (; i <= pre; i++) {
                cur = max(cur, i + nums[i]);
            }
            if (cur == pre) return -1; // cannot reach destination
        }
        return steps;
    }
};
```
## 119. Pascal's Triangle II
```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> ans;
        vector<int> prev;
        
        if (rowIndex == 0) {
            ans.push_back(1);
            return ans;
        }
        if (rowIndex == 1) {
            ans.push_back(1);
            ans.push_back(1);
            return ans;
        }
        
        ans.push_back(1);
        ans.push_back(1);
        
        while (rowIndex >= 2) {
            prev = ans;
            for (int i = 1; i < prev.size(); i++) {
                ans[i] = prev[i-1] + prev[i];
            }
            ans.push_back(1);
            rowIndex--;
        }
        
        return ans;
    }
};
```
## 121. Best Time to Buy and Sell Stock
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;
        int cost = INT_MAX;
        for (int i = 0; i < prices.size(); i++) {
            if (cost > prices[i]) cost = prices[i];
            else if (prices[i] - cost > profit) profit = prices[i] - cost;
        }
        return profit;
    }
};
```
## 338. Counting Bits
```cpp
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> ans;
        
        if (n >= 0) ans.push_back(0);
        if (n >= 1) ans.push_back(1);
        
        int exp = 1, num = 1;
        for (int i = 2; i <= n; i++) {
            if (pow(2,exp) == i) {
                ans.push_back(1);
                exp++;
                num = 1;
            }
            else if (i % 2 != 0) {
                ans.push_back(++num);
            } 
            else {
                num = ans[i/2];
                ans.push_back(ans[i/2]);
            }
        }
        
        return ans;
    }
};
```
## 509. Fibonacci Number
```cpp
class Solution {
public:
    int fib(int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        int f[n+1];
        f[0] = 0;
        f[1] = 1;
        for (int i = 2; i <= n; i++) {
            f[i] = f[i-1] + f[i-2];
        }
        return f[n];
    }
};
```
## 746. Min Cost Climbing Stairs
```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        int ans[n];
        ans[0] = cost[0];
        ans[1] = cost[1];
        for (int i = 2; i < n; i++) {
            ans[i] = cost[i] + min(ans[i-1], ans[i-2]);
        }
        return min(ans[n-1], ans[n-2]);
    }
};
```
## 1137. N-th Tribonacci Number
```cpp
class Solution {
public:
    int tribonacci(int n) {
        if (n == 0) return 0;
        if (n == 1 || n == 2) return 1;
        int ans[n+1];
        ans[0] = 0;
        ans[1] = 1;
        ans[2] = 1;
        for (int i = 3; i <= n; i++) {
            ans[i] = ans[i-1] + ans[i-2] + ans[i-3];
        }
        return ans[n];
    }
};
```
## 392. Is Subsequence
```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        const char *i = s.c_str(), *j = t.c_str();
        while (*i != '\0' && *j != '\0') {
            if (*i == *j) {
                i++;
                j++;
            }
            else j++;
        }
        return *i == '\0';
    }
};
```
## 1646. Get Maximum in Generated Array
```cpp
class Solution {
public:
    int getMaximumGenerated(int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        
        int nums[n+1];
        nums[0] = 0;
        nums[1] = 1;
        for (int i = 2; i <= n; i++) {
            if (i % 2 == 0) nums[i] = nums[i/2];
            else nums[i] = nums[i/2] + nums[i/2 + 1];
        }
        
        int ans = INT_MIN;
        for (int i = 0; i <= n; i++) {
            ans = ans < nums[i] ? nums[i] : ans;
        }
        return ans;
    }
};
```
## 1025. Divisor Game
```cpp
class Solution {
public:
    bool divisorGame(int n) {
        return n % 2 == 0;
    }
};
```
## 85. Maximal Rectangle ‼️❓❓❓❓
```cpp
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int res = 0;
        vector<int> height; // 空的
        for (int i = 0; i < matrix.size(); i++) {
            height.resize(matrix[i].size()); // 相当于对第一次使用的vector赋值0
            for (int j = 0; j < matrix[i].size(); j++) {
                height[j] = matrix[i][j] == '0' ? 0 : (1 + height[j]);
            }
            res = max(res, largestRectangleArea(height));
        }
        return res;
    }
    
    int largestRectangleArea(vector<int>& height) {
        int res = 0;
        stack<int> s;
        height.push_back(0);
        for (int i = 0; i < height.size(); i++) {
            if (s.empty() || height[s.top()] <= height[i]) s.push(i); 
            else {
                int tmp = s.top();
                s.pop();
                res = max(res, height[tmp] * (s.empty() ? i : (i - s.top() - 1)));
                i--;
            }
        }
        return res;
    }
};
```
## 516. Longest Palindromic Subsequence ‼️‼️
```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        vector<vector<int>> ans(n, vector<int>(n, 0));
        for (int i = 0; i < n; i++) {
            ans[i][i] = 1;
        }
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                if (s[i] == s[j]) 
                    ans[i][j] = ans[i + 1][j - 1] + 2;
                else
                    ans[i][j] = max(ans[i + 1][j], ans[i][j - 1]);
            }
        }
        return ans[0][n-1];
    }
};
```
## 877. Stone Game
```cpp
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        int n = piles.size();
        int dp[n][n][2];
        
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                dp[i][j][0] = 0;
                dp[i][j][1] = 0;
            }
        }
        
        for (int i = 0; i < n; i++) {
            dp[i][i][0] = piles[i];
            dp[i][i][1] = 0;
        }
        
        int j, left, right;
        for (int l = 2; l <= n; l++) {
            for (int i = 0; i <= n - l; i++) {
                j = l + i - 1;
                
                left = piles[i] + dp[i+1][j][1];
                right = piles[j] + dp[i][j-1][1];
                
                if (left > right) {
                    dp[i][j][0] = left;
                    dp[i][j][1] = dp[i+1][j][0];
                }
                else {
                    dp[i][j][0] = right;
                    dp[i][j][1] = dp[i][j-1][0];
                }
            }
        }
        return dp[0][n-1][0] > dp[0][n-1][1];
    }
};
```
