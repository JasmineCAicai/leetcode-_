# 分治法
## 50. Pow(x, n) ‼️
x^n = x^(n/2) * x^(n/2) * x^(n%2) \
时间复杂度O(logn)，空间复杂度O(1)
```cpp
class Solution {
public:
    double myPow(double x, int n) {
        if (x == 0.00000) return x;
        if (x == 1.00000 || n == 0) return 1.00000;
        
        double sum = power(x,abs(n));
        if (n < 0) return 1 / sum;
        return sum;
    }
private:
    double power(double x, int n) {
        if (n == 0) return 1;
        
        double v = power(x, n/2);
        if (n % 2 == 0) return v * v;
        return v * v * x;
    }
};
```
## 69. Sqrt(x)
不是分治法
```cpp
class Solution {
public:
    int mySqrt(int x) {
        if (x == 0 || x == 1) return x;
        
        unsigned int i = 1;
        for (; i <= x / 2; i++) {
            if (i * i > x) break;
        }
        return i - 1;
    }
};
```
真正的分治法 \
时间复杂度O(logn)，空间复杂度O(1)
```cpp
class Solution {
public:
    int mySqrt(int x) {
        int left = 1, mid, right = x / 2;
        int ans;
        
        if (x < 2) return x;
        
        while (left <= right) {
            mid = left + (right - left) / 2;
            if (x / mid > mid) {
                left = mid + 1;
                ans = mid;
            }
            else if (x / mid < mid) right = mid - 1;
            else return mid;
        }
        
        return ans;
    }
};
```
