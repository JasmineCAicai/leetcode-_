# 分治法
## 50. Pow(x, n) ‼️
x^n = x^(n/2) * x^(n/2) * x^(n%2)
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
