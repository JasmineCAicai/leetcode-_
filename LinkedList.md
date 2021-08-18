# 单链表
## 2. Add Two Numbers
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
## 86. Partition List
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
## 83. Remove Duplicates from Sorted List
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
## 82. Remove Duplicates from Sorted List II
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
## 61. Rotate List
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
## 19. Remove Nth Node From End of List
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
其他方法：设两个指针p,q，让q先走n步，然后p和q一起走，直到q走到尾节点，删除p->next即可。\
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
## 24. Swap Nodes in Pairs ‼️
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
## 141. Linked List Cycle
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
## 142. Linked List Cycle II
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
## 143. Reorder List ‼️‼️
时间复杂度O(n)，空间复杂度O(1) \
先对半切，后半部分翻转完，再合并。\
**翻转不需递归！！** 🔅
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
    
    ListNode* reverse(ListNode* head) {
        if (!head || !head->next) return head;
        
        ListNode* prev = head;
        for (ListNode *curr = head->next, *next = curr->next; curr; prev = curr, curr = next, next = next ? next->next : nullptr) {
            curr->next = prev;
        }
        
        head->next = nullptr;
        
        return prev;
    }
    
    void reorderList(ListNode* head) {
        
        if (!head->next) return;
        
        ListNode *slow = head, *fast = head, *prev = nullptr;
        
        while (fast && fast->next) {
            prev = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        
        prev->next = nullptr;
        
        slow = reverse(slow);
        
        // merge two lists
        ListNode *curr = head;
        while (curr->next) {
            ListNode *tmp = curr->next;
            curr->next = slow;
            slow = slow->next;
            curr->next->next = tmp;
            curr = tmp;
        }
        curr->next = slow;
    }
};
```
