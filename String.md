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
## 67. Add Binary
```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        int lenA = a.length();
        int lenB = b.length();
        
        int sign = 0;
        int tmp, valA, valB;
        
        string ans = "";
        
        reverse(a.begin(),a.end());
        reverse(b.begin(),b.end());
        
        for (int i = 0; i < max(lenA,lenB); i++) {
            valA = i < lenA ? a[i] - '0' : 0;
            valB = i < lenB ? b[i] - '0' : 0;
            tmp = (valA + valB + sign) % 2;
            sign = (valA + valB + sign) / 2;
            ans.push_back(tmp + 48);
        }
        
        if (sign == 1) ans.push_back('1');
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```
## 10. Regular Expression Matching ‼️❓❓
```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        return isMatch(s.c_str(),p.c_str());
    }
private:
    bool isMatch(const char *s, const char *p) {
        if (*p == '\0') return *s == '\0';
        // next char is not '*', then must match current character
        if (*(p + 1) != '*') {
            if (*p == *s || (*p == '.' && *s != '\0'))
                return isMatch(s + 1, p + 1);
            else
                return false;
        } else { // next char is '*'
            while (*p == *s || (*p == '.' && *s != '\0')) {
                if (isMatch(s, p + 2))
                    return true;
                s++;
            }
            return isMatch(s, p + 2);
        }
    }
};
```
## 44. Wildcard Matching ‼️❓❓
**递归版** \
会超时，时间复杂度O(n! * m!)，空间复杂度O(n) \
**迭代版** \
时间复杂度O(n * m)，空间复杂度O(1)
```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        const char *s_str = s.c_str();
        const char *p_str = p.c_str();
        
        bool flag = false;
        
        const char *str, *ptr;
        
        for (str = s_str, ptr = p_str; *str != '\0'; str++, ptr++) {
            if (*ptr == '?') continue;
            else if (*ptr == '*') {
                flag = true;
                s_str = str;
                p_str = ptr;
                while (*p_str == '*') p_str++;
                if (*p_str == '\0') return true;
                ptr = p_str - 1;
                str = s_str - 1;
            }
            else if (*ptr != *str) {
                if (!flag) return false;
                s_str++;
                str = s_str - 1;
                ptr = p_str - 1;
            }
        }
        
        while (*ptr == '*') ptr++;
        return *ptr == '\0';
    }
};
```
## 14. Longest Common Prefix
时间复杂度O(n1 + n2 + ... + )，空间复杂度O(n)（？）
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.size() == 1) return strs[0];
        
        string ans = "";
        
        int i = 1, ind = 0;
        while (ind < strs[0].length()) {
            if (ind == strs[i].length() || strs[i][ind] != strs[0][ind]) break;
            i++;
            if (i == strs.size()) {
                ans.push_back(strs[0][ind]);
                i = 1;
                ind++;
            }
        }
        
        return ans;
    }
};
```
## 65. Valid Number
时间复杂度O(n)，空间复杂度O(1)
```cpp
class Solution {
public:
    bool isNumber(string s) {
        if (s.length() == 1) {
            if (isdigit(s[0])) return true;
            else return false;
        }
        
        int i = 0, dotInd = -1, eInd = -1, plusInd = -1, minusInd = -1, countP = 0, countM = 0;
        bool dotFlag = false, eFlag = false, ans = true;
        
        while (i < s.length()) {
            if (s[i] == 'e' || s[i] == 'E') {
                if (eInd != -1 || i == s.length()-1 || s[i+1] == '.') {
                    ans = false;
                    break;
                }
                eInd = i;
                if (eInd == 0 || (plusInd != -1 && eInd - plusInd == 1) || (minusInd != -1 && eInd - minusInd == 1)) {
                    ans = false;
                    break;
                }
            }
            else if (isdigit(s[i])) {
                
            }
            else if (s[i] == '.') {
                if (dotInd != -1) {
                    ans = false;
                    break;
                }
                dotInd = i;
                if (dotInd != s.length()-1 && (!isdigit(s[dotInd+1]) && s[dotInd+1] != 'e' && s[dotInd+1] != 'E')) {
                    ans = false;
                    break;
                }
                if (dotInd == 0 && !isdigit(s[dotInd+1])) {
                    ans = false;
                    break;
                }
                if (dotInd == s.length()-1 && (!isdigit(s[dotInd-1]) || isdigit(s[dotInd+1]))) {
                    ans = false;
                    break;
                }
                if (dotInd != -1 && eInd != -1 && dotInd > eInd) {
                    ans = false;
                    break;
                }
            }
            else if (s[i] == '+' || s[i] == '-') {
                if (i == s.length()-1 || isalpha(s[i+1])) {
                    ans = false;
                    break;
                }
                if (i > 0 && isdigit(s[i-1])) {
                    ans = false;
                    break;
                }
                if (countP == 2 && s[i] == '+') {
                    ans = false;
                    break;
                }
                else if (s[i] == '+') {
                    plusInd = i;
                    countP++;
                }
                else if (countM == 2 && s[i] == '-') {
                    ans = false;
                    break;
                }
                else {
                    minusInd = i;
                    countM++;
                }
                
                if (plusInd != -1 && minusInd != -1 && abs(minusInd-plusInd) == 1) {
                    ans = false;
                    break;
                }
            }
            else {
                ans = false;
                break;
            }
            i++;
        }
        
        return ans;
    }
};
```
另一种方法：使用strtod()，时间复杂度O(n)
