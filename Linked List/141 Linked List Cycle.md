# 题目

Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

# C++ Solution

## 解题思路：
利用两个指针，一个移动快，一个移动慢，fast移动两步，slow移动一步，
判断fast是否能够追上slow，若能追上则证明有环

```
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
        if(head == nullptr) return false;
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast != nullptr && slow != nullptr)
        {
            fast = fast ->next;
            if(slow == fast)
                return true;
            if(fast != nullptr)
                fast = fast ->next;
            else
                return false;
            slow = slow -> next;
            if(slow == fast)
                return true;
        }

        return false;
    }
};
```

# Python3 Solution
## 解题思路：同上
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if not head:
            return False
        fast = head
        slow = head
        while fast.next:#fast走得快，只需要判断fast是否是空指针即可
            fast = fast.next
            if fast == slow:
                return True
            fast = fast.next
            if not fast:
                return False
            slow = slow.next
        return False
```
