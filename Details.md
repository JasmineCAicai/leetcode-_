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
