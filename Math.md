# 数学类
## 60. Permutation Sequence
```cpp
class Solution {
public:
    string getPermutation(int n, int k) {
        string res;
        string num = "123456789";
        vector<int> f(n, 1);
        for (int i = 1; i < n; ++i) f[i] = f[i - 1] * i;
        --k;
        for (int i = n; i >= 1; --i) {
            int j = k / f[i - 1];
            k %= f[i - 1];
            res.push_back(num[j]);
            num.erase(j, 1);
        }
        return res;
    }
};
```
## 204. Count Primes ‼️‼️‼️
好巧的方法
```cpp
class Solution {
public:
    int countPrimes(int n) {
        int res = 0;
        vector<bool> prime(n, true);
        for (int i = 2; i < n; ++i) {
            if (!prime[i]) continue;
            ++res;
            for (int j = 2; i * j < n; ++j) {
                prime[i * j] = false;
            }
        }
        return res;
    }
};
```
## 231. Power of Two
```cpp
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if (n != 1 && n % 2 != 0) return false;
        long long tmp = 1;
        while (tmp < n) {
            tmp *= 2;
        }
        return tmp == n;
    }
};
```
## 268. Missing Number
```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int i = 0, n = nums.size(), ans = 0;
        while (i < n) {
            if (ans != nums[i]) break;
            ans++;
            i++;
        }
        return ans;
    }
};
```
## 326. Power of Three
```cpp
class Solution {
public:
    bool isPowerOfThree(int n) {
        if (n <= 0) return false;
        
        long long a = 1;
        
        while (a < n) {
            a *= 3;
        }
            
        return a == n;
    }
};
```
## 367. Valid Perfect Square
```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {
        for (int i = 1; i <= num / i; i++) {
            if (i * i == num) return true;
        }
        return false;
    }
};
```
## 342. Power of Four
```cpp
class Solution {
public:
    bool isPowerOfFour(int n) {
        long long exp = 1;
        while (exp < n) {
            exp *= 4;
        }
        return exp == n;
    }
};
```
## 441. Arranging Coins
```cpp
class Solution {
public:
    int arrangeCoins(int n) {
        int i = 1;
        long long total = 1;
        while (total < n) {
            i++;
            total += i;
        }
        
        if (total == n) return i;
        return i - 1;
    }
};
```
