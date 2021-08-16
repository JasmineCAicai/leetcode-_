# Leetcode Record

## 线性表
### 数组
#### 26. Remove Duplicates from Sorted Array
时间复杂度O(n)，空间复杂度O(1)
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        int k = 0;
        for (int i=1; i<nums.size(); i++) {
            if (nums[k] != nums[i]) {
                nums[++k] = nums[i];
            }
        }
        
        return k+1;
    }
};
```
#### 80. Remove Duplicates from Sorted Array II
时间复杂度O(n)，空间复杂度O(1)
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        int r = 1;
        int index = 0;
        
        for (int i=1; i<nums.size(); i++) {
            if (nums[i] == nums[i-1]) r++;
            else {
                if (r==2) nums[++index] = nums[i-1];
                if (r>2) {
                    index += 2;
                    nums[index-1] = nums[i-1];
                }
                else 
                    index++;
                nums[index] = nums[i];
                r = 1;
            }
            if(i == nums.size()-1 && r==2) nums[++index] = nums[i];
            if(i == nums.size()-1 && r>2) nums[++index] = nums[i-1];
        }
        
        return index + 1;
    }
};
```
更简洁的方法：
时间复杂度O(n)，空间复杂度O(1)
```cpp
class Solution {
  public:
      int removeDuplicates(vector<int>& nums) {
          if (nums.size() <= 2) return nums.size();
          int index = 2;
          for (int i = 2; i < nums.size(); i++){
              if (nums[i] != nums[index - 2])
                  nums[index++] = nums[i];
          }
          return index;
      }
};
```
#### 33. Search in Rotated Sorted Array
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int first = 0, last = nums.size();
          while (first != last) {
              const int mid = first  + (last - first) / 2;
              if (nums[mid] == target)
                  return mid;
              if (nums[first] <= nums[mid]) {
                  if (nums[first] <= target && target < nums[mid])
                      last = mid;
                  else
                      first = mid + 1;
              } else {
                  if (nums[mid] < target && target <= nums[last-1])
                      first = mid + 1;
                  else
                      last = mid;
              } 
         }
         return -1;
    }
};
```
#### 81. Search in Rotated Sorted Array II
```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int index = 0;
        for (int i=1; i<nums.size(); i++) {
            if (nums[i] != nums[i-1])
                nums[++index] = nums[i];
        }
        
        if (index==0) return nums[0]==target;
        
        int last = index + 1;
        int first = 0, mid;
        while (first != last) {
            mid = first + (last - first) / 2;
            if (target == nums[mid]) return true;
            if (nums[first] <= nums[mid]) {
                if (target >= nums[first] && target < nums[mid])
                    last = mid;
                else
                    first = mid + 1;
            } else {
                if (target > nums[mid] && target <= nums[last-1])
                    first = mid + 1;
                else
                    last = mid;
            }
        }
        
        return false;
    }
};
```
时间复杂度和空间复杂度几乎无差别，但逻辑上较好的方法：
```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int first = 0, last = nums.size();
        while (first != last) {
            const int mid = first  + (last - first) / 2;
            if (nums[mid] == target)
                return true;
            if (nums[first] < nums[mid]) {
                if (nums[first] <= target && target < nums[mid])
                    last = mid;
                else
                    first = mid + 1;
            } else if (nums[first] > nums[mid]) {
                if (nums[mid] < target && target <= nums[last-1])
                    first = mid + 1;
                else
                    last = mid;
            } else
                //skip duplicate one
                first++;
        }
        return false;
    }
};
```
#### 4. Median of Two Sorted Arrays
运行时间较好，但内存使用太多。
```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector<int> mergedNum;
        int l1 = 0, l2 = 0;
        double ans = 0.00000;
        while(l1 < nums1.size() && l2 < nums2.size()){
            if (nums1[l1]<=nums2[l2]) {
                mergedNum.push_back(nums1[l1]);
                l1++;
            }
            else {
                mergedNum.push_back(nums2[l2]);
                l2++;
            }
        }
        while(l1 < nums1.size()) {
            mergedNum.push_back(nums1[l1]);
            l1++;
        }
        while(l2 < nums2.size()) {
            mergedNum.push_back(nums2[l2]);
            l2++;
        }
        if (mergedNum.size() == 1) return (double)mergedNum[0];
        if (mergedNum.size()%2==0) {
            ans = (double)mergedNum[mergedNum.size()/2-1] + (double)mergedNum[mergedNum.size()/2];
            ans /= 2;
        }
        else {
            ans = (double)mergedNum[mergedNum.size()/2];
        }
        return ans;
    }
};
```
运行时间较好，但内存使用太多。
```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& A, vector<int>& B) {
        const int m = A.size();
        const int n = B.size();
        int total = m + n;
        if (total & 0x1)
            return find_kth(A.begin(), m, B.begin(), n, total / 2 + 1);
        else
            return (find_kth(A.begin(), m, B.begin(), n, total / 2) + find_kth(A.begin(), m, B.begin(), n, total / 2 + 1)) / 2.0;
    }
    
private:
    static int find_kth(std::vector<int>::const_iterator A, int m, std::vector<int>::const_iterator B, int n, int k) {
        //always assume that m is equal or smaller than n
        if (m > n) return find_kth(B, n, A, m, k);
        if (m == 0) return *(B + k - 1);
        if (k == 1) return min(*A, *B);
        //divide k into two parts
        int ia = min(k / 2, m), ib = k - ia;
        if (*(A + ia - 1) < *(B + ib - 1))
            return find_kth(A + ia, m - ia, B, n, k - ia);
        else if (*(A + ia - 1) > *(B + ib - 1))
            return find_kth(A, m, B + ib, n - ib, k - ib);
        else
            return A[ia - 1];
    }
};
```
#### 128. Longest Consecutive Sequence
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if(nums.empty()) return 0;
        sort(nums.begin(),nums.end());
        nums.erase(unique(nums.begin(),nums.end()),nums.end());
        int total = 1, tmp = 1;
        for(int i=1; i<nums.size(); i++){
            if(nums[i]==(nums[i-1]+1)) tmp++;
            else {
                if(total < tmp) total = tmp;
                tmp = 1;
            }
        }
        if(total < tmp) total = tmp;
        return total;
    }
};
```
#### 1. Two Sum
运行时间较长，但内存使用较少。
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> tmp;
        vector<int> ans;
        for (int i=0; i<nums.size(); i++) {
            tmp.push_back(target-nums[i]);
        }
        for (int i=0; i<tmp.size(); i++) {
            vector<int>::iterator index = find(nums.begin(), nums.end(), tmp[i]);
            if (index != nums.end() && distance(nums.begin(), index) != i) {
                ans.push_back(i);
                ans.push_back(distance(nums.begin(), index));
                break;
            }
        }
        
        return ans;
    }
};
```
运行时间较短，但内存使用较多
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> mapping;
        vector<int> result;
        for (int i = 0; i < nums.size(); i++) {
            mapping[nums[i]] = i;
        }
        for (int i = 0; i < nums.size(); i++) {
            const int gap = target - nums[i];
            if (mapping.find(gap) != mapping.end() && mapping[gap] > i) {
                result.push_back(i);
                result.push_back(mapping[gap]);
                break;
            }
        }
        return result;
    }
};
```
#### 15. 3Sum
左右夹逼
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        if (nums.size() < 3) return result;
        sort(nums.begin(), nums.end());
        const int target = 0;
        
        auto last = nums.end();
        for (auto i = nums.begin(); i < last-2; i++) {
            auto j = i + 1;
            if (i > nums.begin() && *i == *(i-1)) continue;
            auto k = last - 1;
            while (j < k) {
                if (*i + *j + *k < target) {
                    j++;
                    while (*j == *(j-1) && j < k) j++;
                } else if (*i + *j + *k > target) {
                    k--;
                    while (*k == *(k+1) && k > j) k--;
                } else {
                    result.push_back({*i, *j, *k});
                    j++;
                    k--;
                    while (*j == *(j-1) && *k == *(k+1) && j < k) j++;
                }
            }
        }
        return result;
    }
};
```
#### 16. 3Sum Closest
```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int gap, tmp, finalAns, result;
        auto last = nums.end();
        for (auto i = nums.begin(); i < last-2; i++) {
            auto j = i + 1;
            if (i > nums.begin() && *i == *(i-1)) continue;
            auto k = last - 1;
            if (i == nums.begin()) {
                result = *i + *j + *k;
                gap = abs(result - target);
                finalAns = result;
            }
            while (j < k) {
                result = *i + *j + *k;
                tmp = abs(result - target);
                if (tmp < gap) {
                    gap = tmp;
                    finalAns = result;
                }
                if (result < target) {
                    j++;
                    while (*j == *(j-1) && j < k) j++;
                } else if (result > target) {
                    k--;
                    while (*k == *(k+1) && k > j) k--;
                } else {
                    break;
                }
            }
            if (result == target) {
                finalAns = result;
                break;
            }
        }
        return finalAns;
    }
};
```
更简洁的方法：
```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        int result = 0;
        int min_gap = INT_MAX;
        sort(nums.begin(), nums.end());
        for (auto a = nums.begin(); a != prev(nums.end(), 2); ++a) {
            auto b = next(a);
            auto c = prev(nums.end());
            while (b < c) {
                const int sum = *a + *b + *c;
                const int gap = abs(sum - target);
                if (gap < min_gap) {
                    result = sum;
                    min_gap = gap;
                }
                if (sum < target) ++b;
                else --c; 
            }
        }
        return result;
    }
};
```
#### 27. Remove Element
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        if (nums.empty()) return 0;
        sort(nums.begin(), nums.end());
        int count = 0;
        for (int i=0; i<nums.size(); i++) {
            if (nums[i] == val) {
                nums[i] = 200;
                count++;
            }
        }
        sort(nums.begin(), nums.end());
        return nums.size() - count;
    }
};
```
#### 36. Valid Sudoku
```cpp
class Solution {
public:
    bool flag[9];
    
    bool isValidSudoku(vector<vector<char>>& board) {
        for (int i=0; i<9; i++) {
            // Check row
            fill(flag, flag + 9, false);
            for (int j=0; j<9; j++) {
                if (!check(board[i][j])) return false;
            }
            
            // Check column
            fill(flag, flag + 9, false);
            for (int j=0; j<9; j++) {
                if (!check(board[j][i])) return false;
            }
        }
        
        // Check square
        for (int r=0; r < 3; r++) {
            for (int c=0; c < 3; c++) {
                fill(flag, flag + 9, false);
                for (int i=r*3; i<r*3+3; i++) {
                    for (int j=c*3; j<c*3+3; j++) {
                        if (!check(board[i][j])) return false;
                    }
                }
            }
        }
        return true;
    }
    
    bool check(char ch) {
        if (ch == '.') return true;
        if (flag[ch - '1']) return false;
        flag[ch - '1'] = true;
        return flag[ch - '1'];
    }
};
```
#### 42. Trapping Rain Water
好巧妙的方法...
时间复杂度 O(n)，空间复杂度 O(1)
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.size() <= 2) return 0;
        
        const int s = height.size();
        int maxInd = 0;
        for (int i=1; i<s; i++) {
            if (height[i] > height[maxInd]) maxInd = i;
        }
        
        int total = 0;
        
        for (int i=0, high=0; i<maxInd; i++) {
            if (height[i] > high) high = height[i];
            else total += (high - height[i]);
        }
        for (int i=s-1, high=0; i>maxInd; i--) {
            if (height[i] > high) high = height[i];
            else total += (high - height[i]);
        }
        
        return total;
    }
};
```
#### 48. Rotate Image
```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix[0].size();
        int tmp;
        
        /*
        * 交换列
        * C11 C12 C13         C13 C12 C11
        * C21 C22 C23    =>   C23 C22 C21
        * C31 C32 C33         C33 C32 C31
        */
        for (int i=0; i<n/2; i++) {
            for (int j=0; j<n; j++) {
                tmp = matrix[j][i];
                matrix[j][i] = matrix[j][n-i-1];
                matrix[j][n-i-1] = tmp;
            }
        }
        
        /*
        * 交换斜行(斜行长度等于n)
        * C13 C12 C11         C31 C21 C11
        * C23 C22 C21    =>   C32 C22 C12
        * C33 C32 C31         C33 C23 C13
        */
        for (int i=0; i<n-1; i++) {
            for (int j=0; j<n-i; j++) {
                tmp = matrix[j][i];
                matrix[j][i] = matrix[n-i-1][n-j-1];
                matrix[n-i-1][n-j-1] = tmp;
            }
        }
        
        return;
    }
};
```
#### 66. Plus One
```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int dec = 0;
        for (int i=digits.size()-1; i>=0; i--) {
            if (i == digits.size()-1 && digits[i]+1 < 10) digits[i]++;
            else if (i != digits.size()-1 && digits[i]+dec < 10) {
                digits[i] += dec;
                dec = 0;
            }
            else {
                digits[i] = 0;
                dec = 1;
            }
        }
        if (dec == 1) digits.insert(digits.begin(), 1);
        return digits;
    }
};
```
#### 70. Climbing Stairs ‼️
递归太慢，用迭代的方法
```cpp
class Solution {
public:
    int climbStairs(int n) {
        int prev = 0, tmp = 0, cur = 1;
        for (int i = 1; i <= n; i++) {
            tmp = cur;
            cur += prev;
            prev = tmp;
        }
        return cur;
    }
};
```
#### 73. Set Matrix Zeroes ‼️
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        const size_t m = matrix.size();
        const size_t n = matrix[0].size();
        vector<bool> row(m, false); 
        vector<bool> col(n, false); 
        for (size_t i = 0; i < m; ++i) {
            for (size_t j = 0; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    row[i] = col[j] = true;
                } 
            }
        }
        for (size_t i = 0; i < m; ++i) {
            if (row[i]) fill(&matrix[i][0], &matrix[i][0] + n, 0);
        }
        for (size_t j = 0; j < n; ++j) {
            if (col[j]) {
                for (size_t i = 0; i < m; ++i) {
                    matrix[i][j] = 0;
                }
            } 
        }
    }
};
```
#### 134. Gas Station ‼️
```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int total = 0;
        int j = -1;
        for (int i=0, sum=0; i<gas.size(); i++) {
            sum += gas[i] - cost[i];
            total += gas[i] - cost[i];
            if (sum < 0) {
                j = i;
                sum = 0;
            }
        }
        return total >= 0 ? j + 1 : -1;
    }
};
```
#### 135. Candy ‼️
从头 & 从尾
```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        const int n = ratings.size();
        vector<int> increment(n);
        
        for (int i = 1, inc = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1])
                increment[i] = max(inc++, increment[i]);
            else
                inc = 1;
        }
        
        for (int i = n - 2, inc = 1; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1])
                increment[i] = max(inc++, increment[i]);
            else
                inc = 1;
        }
        
        return accumulate(&increment[0], &increment[0]+n, n);
    }
};
```
#### 136. Single Number
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        if (nums.size()==1) return nums[0];
        sort(nums.begin(),nums.end());
        int i;
        for (i=1; i<=nums.size(); i+=2) {
            if (i == nums.size()-1 || nums[i-1] != nums[i]) break;
        }
        if (i == nums.size()-1) return nums[i];
        return nums[i-1];
    }
};
```
更好的方法：异或
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int x = 0;
        for (auto i : nums) {
            x ^= i;
        }
        return x;
    }
};
```
#### 137. Single Number II ‼️（没懂）❓❓❓
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int one = 0, two = 0, three = 0;
        for (auto i : nums) {
            two |= (one & i);
            one ^= i;
            three = ~(one & two);
            one &= three;
            two &= three;
        }
        return one;
    }
};
```
### 单链表
#### 2. Add Two Numbers
for循环的运算、条件和边界设置的很巧妙
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode ans(-1); // 头节点
        ListNode* sum = &ans;
        int tmp = 0;
        
        for (ListNode* pa = l1, *pb = l2; pa != nullptr || pb != nullptr; pa = pa == nullptr ? 0 : pa->next, pb = pb == nullptr ? 0 : pb->next, sum = sum->next) {
            const int v1 = pa == nullptr ? 0 : pa->val;
            const int v2 = pb == nullptr ? 0 : pb->val;
            sum->next = new ListNode((v1 + v2 + tmp) % 10);
            tmp = (v1 + v2 + tmp) / 10;
        }
        
        if (tmp > 0) {
            sum->next = new ListNode(tmp);
        }
        return ans.next;
    }
};
```
#### 86. Partition List
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode left(-1);
        ListNode right(-1);
        
        ListNode* l_cur = &left;
        ListNode* r_cur = &right;
        
        for (ListNode* cur = head; cur != nullptr; cur = cur->next) {
            if (cur->val < x) {
                l_cur->next = cur;
                l_cur = cur;
            }
            else {
                r_cur->next = cur;
                r_cur = cur;
            }
        }
        l_cur->next = right.next;
        r_cur->next = nullptr;
        return left.next;
    }
};
```
#### 83. Remove Duplicates from Sorted List
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) return head;
        int num;
        ListNode ans(-1);
        ListNode* rev = &ans;
        num = head->val;
        rev->next = head;
        rev = head;
        for (head = head->next; head; head = head->next) {
            if (head->val != num) {
                rev->next = head;
                rev = head;
                num = rev->val;
            }
        }
        rev->next = nullptr;
        return ans.next;
    }
};
```
迭代版：运行时间比上一种方法长，所需内存也差不多
空间复杂度为O(1)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) return head;
        
        for (ListNode* prev = head, *cur = head->next; cur; cur = prev->next) {
            if (prev->val == cur->val) {
                prev->next = cur->next;
                delete cur;
            }
            else {
                prev = cur;
            }
        }
        return head;
    }
};
```
#### 82. Remove Duplicates from Sorted List II
时间复杂度O(n)，空间复杂度也是O(n)（？）
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        //if (!head) return head;
        
        ListNode ans(-1);
        ListNode* filtered = &ans;
        
        for (ListNode* prev = head, *cur = head; cur; cur = cur->next) {
            if (cur->val != prev->val) prev = cur;
            
            if (!prev->next) {
                filtered->next = prev;
                filtered = prev;
            }
            else if (prev->next->val != prev->val) {
                filtered->next = prev;
                filtered = prev;
            }
        }
        
        filtered->next = nullptr;
        return ans.next;
    }
};
```
更好的方法：递归，空间复杂度O(1)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next) return head;
        
        ListNode *p = head->next;
        if (head->val == p->val) {
            while (p && head->val == p->val) {
                ListNode *tmp = p;
                p = p->next;
                delete tmp;
            }
            delete head;
            return deleteDuplicates(p);
        } else {
            head->next = deleteDuplicates(head->next);
            return head;
        }
    }
};
```
#### 61. Rotate List
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        
        if (!head || k == 0) return head;
            
        ListNode* tmp = head;
        int len = 0;
        for (; tmp; tmp = tmp->next) {
            len++;
        }
        
        k %= len;
        
        if (k == 0 || len == 1) return head;
        
        tmp = head;
        ListNode* newHead, *newTail;
        for (int i=1; i <= (len - k); i++) {
            newTail = tmp;
            tmp = tmp->next;
        }

        newHead = tmp;
        
        while (tmp->next!=nullptr) {
            tmp = tmp->next;
        }
        
        newTail->next = nullptr;
        
        tmp->next = head;
        
        return newHead;
    }
};
```
更好的方法：空间复杂度O(1)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (head == nullptr || k == 0) return head;
        int len = 1;
        ListNode* p = head;
        while (p->next) { 
            len++;
            p = p->next; 
        }
        k = len - k % len;
        p->next = head; 
        for(int step = 0; step < k; step++) {
            p = p->next; 
        }
        head = p->next; 
        p->next = nullptr; 
        return head;
    }
};
```
#### 19. Remove Nth Node From End of List
空间复杂度O(1)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* tmp = head;
        int len = 0;
        for (; tmp; tmp = tmp->next) {
            len++;
        }
        if (len == 1) return nullptr;
        n = len - n;
        
        ListNode ans(-1);
        tmp = &ans;
        
        for (int i = 1; i <= n; i++, head = head->next) {
            tmp->next = head;
            tmp = head;
        }
        tmp->next = head->next;
        delete head;
        
        return ans.next;
    }
};
```
其他方法：设两个指针p,q，让q先走n步，然后p和q一起走，直到q走到尾节点，删除p->next即可。
空间复杂度O(1)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode dummy{-1, head};
        ListNode *p = &dummy, *q = &dummy;
        
        for(int i = 0; i < n; i++) {
            q = q->next;
        }
        
        while(q->next) { 
            p = p->next;
            q = q->next; 
        }
        
        ListNode *tmp = p->next;
        p->next = p->next->next;
        delete tmp;
        
        return dummy.next;
    }
};
```
#### 24. Swap Nodes in Pairs ‼️
时间复杂度O(n)，空间复杂度O(1)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (!head || !head->next) return head;
        
        ListNode ans(-1);
        ans.next = head;
        
        for (ListNode *prev = &ans, *cur = prev->next, *next = cur->next; next; prev = cur, cur = cur->next, next = cur ? cur->next : nullptr) {
            prev->next = next;
            cur->next = next->next;
            next->next = cur;
        }
        
        return ans.next;
    }
};
```
#### 141. Linked List Cycle
时间复杂度O(n)，空间复杂度O(n)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (!head) return head;
        map<ListNode*,bool> flag;
        
        int i = 0;
        while (head->next) {
            if (flag.find(head->next) != flag.end()) return true;
            flag.insert({head,true});
            head = head->next;
        }
        return false;
    }
};
```
更好的方法：时间复杂度O(n)，空间复杂度O(1)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;
        
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) return true;
        }
        return false;
    }
};
```
#### 142. Linked List Cycle II
时间复杂度O(n)，空间复杂度O(n)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        map<ListNode*,bool> flag;
        
        while (head && head->next) {
            if (flag.find(head) != flag.end()) return head;
            flag.insert({head,true});
            head = head->next;
        }
        return nullptr;
    }
};
```
**更好的方法：时间复杂度O(n)，空间复杂度O(1) ‼️⁉️** \
当 fast 与 slow 相遇时， slow 肯定没有遍历完链表，而 fast 已经在环内循环了 n 圈（1 <= n）。\
假设 slow 走了 s 步，则 fast 走了 2s 步（ fast 步数还等于 s 加上在环上多转的 n 圈），设环长为 r ，则：\
    2s = s + nr \
    s = nr \
设整个链表长 L ，环入口点与相遇点距离为 a ，起点到环入口点的距离为 x ，则：\
    x + a = nr = (n - 1)r + r = (n - 1)r + L - x \
    x = (n - 1)r + (L - x - a) \
L - x - a 为相遇点到环入口点的距离。\
由此可知，从链表头到环入口点等于 n - 1 圈内环加相遇点到环入口点，于是可以从 head 开始另设一个指针 slow2 ，两个慢指针每次前进一步，它们一定会在环入口点相遇。
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                ListNode *slow2 = head;
                while (slow2 != slow) {
                    slow2 = slow2->next;
                    slow = slow->next;
                }
                return slow2;
            }
        }
        return nullptr;
    }
};
```
