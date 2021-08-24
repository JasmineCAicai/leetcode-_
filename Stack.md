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
## 150. Evaluate Reverse Polish Notation
用了 vector
```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        int ans = 0, i = 0, left = 0, right = 0;
        vector<int> num;
        while (i < tokens.size()) {
            if (tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/") num.push_back(atoi(tokens[i].c_str()));
            else {
                right = num[num.size()-1];
                num.pop_back();
                left = num[num.size()-1];
                num.pop_back();
                if (tokens[i] == "+") num.push_back(left + right);
                else if (tokens[i] == "-") num.push_back(left - right);
                else if (tokens[i] == "*") num.push_back(left * right);
                else num.push_back(left / right);
            }
            i++;
        }
        
        return num[0];
    }
};
```
用了 stack
```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        int ans = 0, i = 0, left = 0, right = 0;
        stack<int> num;
        while (i < tokens.size()) {
            if (tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/") num.push(atoi(tokens[i].c_str()));
            else {
                right = num.top();
                num.pop();
                left = num.top();
                num.pop();
                if (tokens[i] == "+") num.push(left + right);
                else if (tokens[i] == "-") num.push(left - right);
                else if (tokens[i] == "*") num.push(left * right);
                else num.push(left / right);
            }
            i++;
        }
        
        return num.top();
    }
};
```
## 32. Longest Valid Parentheses ‼️‼️
时间复杂度O(n)，空间复杂度O(n)
```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        int top = 0, i = 0, len = 0, tmp = 0;
        
        stack<int> brakets;
        
        brakets.push(-1);
        for (; i < s.length(); i++) {
            if (s[i] == '(') brakets.push(i);
            else {
                brakets.pop();
                if (brakets.empty()) brakets.push(i);
                tmp = i - brakets.top();
                len = len < tmp ? tmp : len;
            }
        }
        
        return len;
    }
};
```
