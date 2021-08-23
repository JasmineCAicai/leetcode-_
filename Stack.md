# 栈
## 20. Valid Parentheses
```cpp
class Solution {
public:
    bool isValid(string s) {
        vector<char> stack;
        
        int i = 0;
        
        while (i < s.length()) {
            if (!stack.empty()) {
                if (s[i] == ')' && stack[stack.size()-1] == '(') stack.pop_back();
                else if (s[i] == '}' && stack[stack.size()-1] == '{') stack.pop_back();
                else if (s[i] == ']' && stack[stack.size()-1] == '[') stack.pop_back();
                else stack.push_back(s[i]);
            }
            else stack.push_back(s[i]);
            i++;
        }
        
        return stack.empty();
    }
};
```
更直接方法：使用stack \
时间复杂度O(n)，空间复杂度O(n)，和上一种一样。
```cpp
class Solution {
public:
    bool isValid(string s) {
        string left = "([{";
        string right = ")]}";
        stack<char> stk;
        for (auto c : s) {
            if (left.find(c) != string::npos) {
                stk.push (c);
            } else {
                if (stk.empty () || stk.top () != left[right.find (c)])
                    return false;
                else
                    stk.pop ();
            } 
        }
        return stk.empty();
    }
};
```
