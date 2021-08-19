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
## 8. String to Integer (atoi)
```cpp
class Solution {
public:
    int myAtoi(string s) {
        int num = 0;
        int sign = 1;
        const int len = s.length();
        int i = 0;
        
        while (i < len && s[i] == ' ') {
            i++;
        }
        
        if (s[i] == '+') i++;
        else if (s[i] == '-') {
            i++;
            sign = -1;
        }
        
        for (; i < len; i++) {
            if (!isdigit(s[i])) break;
            
            if (num > INT_MAX / 10 || (num == INT_MAX / 10 && (s[i] - '0') > INT_MAX % 10)) 
                return sign == -1 ? INT_MIN : INT_MAX;
            
            num = num * 10 + (s[i] - '0');
        }
        
        return num * sign;
    }
};
```
