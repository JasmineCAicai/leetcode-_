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
