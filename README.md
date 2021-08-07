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
