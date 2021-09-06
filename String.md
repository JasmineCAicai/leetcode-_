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
## 12. Integer to Roman
时间复杂度O(num)，空间复杂度O(1)
```cpp
class Solution {
public:
    string intToRoman(int num) {
        string roman[13] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        int roInt[13] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        
        string ans;
        int tmp = 0;
        for (int i = 0; i < 13; i++) {
            tmp = num / roInt[i];
            num %= roInt[i];
            for (; tmp > 0; tmp--) {
                ans += roman[i];
            }
        }
        return ans;
    }
};
```
## 13. Roman to Integer
```cpp
class Solution {
public:
    int maps(char s) {
        switch (s) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
        }
        return 0;
    }
    
    int romanToInt(string s) {
        int ans = 0;
        for (int i = s.length()-1; i >= 0; i--) {
            if (i != 0 && maps(s[i-1]) < maps(s[i])) {
                ans += (maps(s[i]) - maps(s[i-1]));
                i--;
            }
            else {
                ans += maps(s[i]);
            }
        }
        
        return ans;
    }
};
```
## 49. Group Anagrams ‼️‼️
用unordered_map
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> group;
        for (auto s : strs) {
            string key = s;
            sort(key.begin(), key.end());
            group[key].push_back(s);
        }
        
        vector<vector<string>> ans;
        for (auto it = group.begin(); it != group.end(); it++) {
            if (it->second.size() > 0) {
                vector<string> ana;
                ana.insert(ana.end(),it->second.begin(),it->second.end());
                ans.push_back(ana);
            }
        }
        return ans;
    }
};
```
## 58. Length of Last Word
```cpp
class Solution {
public:
    int lengthOfLastWord(string s) {
        int i = s.length() - 1;
        while (s[i] == ' ') {
            i--;
        }
        int length = 0;
        while (i >= 0) {
            if (s[i] == ' ') break;
            i--;
            length++;
        }
        return length;
    }
};
```
## 71. Simplify Path
时间复杂度O(n)，空间复杂度O(n)
```cpp
class Solution {
public:
    string simplifyPath(string path) {
        vector<string> directories;
        for (int i = 0; i < path.length(); i++) {
            string tmp = "";
            int t = i;
            while (i < path.length() && path[i] != '/') {
                tmp = tmp + path[i];
                i++;
            }
            if (t != i) i--;
            if (tmp == ".") continue;
            else if (tmp == "..") {
                if (!directories.empty()) {
                    directories.pop_back();
                }
            } 
            else if (tmp != "") directories.push_back(tmp);
        }
        
        string ans = "";
        for (int i = 0; i < directories.size(); i++) {
            ans = ans + '/' + directories[i];
        }
        if (ans.empty()) ans = "/";
        return ans;
    }
};
```
其他方法：运行时间和所需内存都较少 ‼️
时间复杂度O(n)，空间复杂度O(n)
```cpp
class Solution {
public:
    string simplifyPath(string path) {
        vector<string> dirs; 
        for (auto i = path.begin(); i != path.end();) {
            ++i;
            auto j = find(i, path.end(), '/');
            auto dir = string(i, j);
            if (!dir.empty() && dir != ".") {
                if (dir == "..") {
                    if (!dirs.empty())
                        dirs.pop_back();
                    } else dirs.push_back(dir);
            }
            i = j;
        }
        stringstream out;
        if (dirs.empty()) {
            out << "/";
        } else {
            for (auto dir : dirs)
                out << '/' << dir;
        }
        return out.str();
    }
};
```
## 5. Longest Palindromic Substring
时间复杂度O(n^2)，空间复杂度O(1)
```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.size() == 1) return s;
        string ans, str1, str2;
        int i = 0;
        while (i < s.size()) {
            str1 = palindrome(s, i, i);
            str2 = palindrome(s, i, i + 1);
            ans = str1.size() > ans.size() ? str1 : ans;
            ans = str2.size() > ans.size() ? str2 : ans;
            i++;
        }
        return ans;
    }
    
private:
    string palindrome(string s, int l, int r) {
        while (l >= 0 && r < s.size() && s[l] == s[r]) {
            l--;
            r++;
        }
        return s.substr(l + 1, r - l - 1);
    }
};
```
## 22. Generate Parentheses
```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        for (int i = 0; i < n; i++) {
            left.push('(');
            right.push(')');
        }
        generateParenthesis("");
        return ans;
    }
    
private:
    stack<char> left, right;
    vector<string> ans;
    void generateParenthesis(string s) {
        if (left.empty() && right.empty()) {
            ans.push_back(s);
            return;
        }
        
        if (!left.empty()) {
            char l = left.top();
            left.pop();
            generateParenthesis(s + l);
            left.push(l);
        }
        if (!right.empty() && left.size() < right.size()) {
            char r = right.top();
            right.pop();
            generateParenthesis(s + r);
            right.push(r);
        }
    }
};
```
