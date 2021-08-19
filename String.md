# 字符串
## 125. Valid Palindrome
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
## 28. Implement strStr()
暴力方法：时间复杂度O(M * N)，空间复杂度O(1)
```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        if (needle.empty()) return 0;
        const int N = haystack.size() - needle.size() + 1;
        for (int i = 0; i < N; i++) {
            int j = i;
            int k = 0;
            while (j < haystack.size() && k < needle.size() && haystack[j] == needle[k]) {
                j++;
                k++; 
            }
            if (k == needle.size()) return i;
        }
        return -1;
    }
};
```
KMP：时间复杂度O(M + N)，空间复杂度O(1)
