# Leetcode Record

## 线性表
### [1. 数组](https://github.com/JasmineCAicai/leetcode-_/blob/0d775d9c7731f9e013ac3e312d341111fd360142/Array.md)
### [2. 单链表](https://github.com/JasmineCAicai/leetcode-_/blob/c1920f0187418e80597e54b8056cb6c559b96f67/LinkedList.md)
## 字符串
### 125. Valid Palindrome
```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int len = s.size()-1;
        for (int i=0; i<len; ) {
            if (!isalpha(s[i]) && !isdigit(s[i])) {
                i++;
                continue;
            }
            if (!isalpha(s[len]) && !isdigit(s[len])) {
                len--;
                continue;
            }
            
            if (!isalpha(s[i]) || !isalpha(s[len])) {
                if (s[i] != s[len]) return false;
            }
            else if (tolower(s[i]) != tolower(s[len])) return false;
            
            i++;
            len--;
        }
        
        return true;
    }
};
```
