# 细节实现题
## 7. Reverse Integer
注意对 x 翻转后取值范围的判断
```cpp
class Solution {
public:
    int reverse(int x) {
        long long positive = abs(x);
        long long ans = 0;
        while (positive > 0) {
            ans += (positive % 10);
            positive /= 10;
            if (positive != 0) ans *= 10;
        }
        
        bool sign = x > 0 ? false: true;
        if (ans > 2147483647 || (sign && ans > 2147483648)) {
              return 0;
         } else {
            if (sign) {
                return -ans;
            } 
            else {
                return ans;
            } 
        }
    }
};
```
## 9. Palindrome Number
方法一
```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) return false;
        if (x > 0 && x < 10) return true;
        
        vector<int> nums;
        
        while (x > 0) {
            nums.push_back(x % 10);
            x /= 10;
        }
        
        int l = 0, r = nums.size()-1;
        while (l <= r) {
            if (nums[l] != nums[r]) return false;
            l++;
            r--;
        }
        
        return true;
    }
};
```
方法二：
```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) return false;
        if (x >= 0 && x < 10) return true;
        
        int d = 1;
        while (x / d >= 10) {
            d *= 10;
        }
        
        int q = 0, r = 0;
        while (x > 0) {
            r = x % 10;
            q = x / d;
            if (r != q) return false;
            x %= d;
            x /= 10;
            d /= 100;
        }
        return true;
    }
};
```
