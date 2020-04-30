# 题目

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

# Python3 Solution
## 解题思路：
分三步解题
判断是否有环，
在有环后，判断环多长记为L，
两个指针从头开始，一个走L步，当再次相遇时，就是环的入口
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        if not head:
            return None
        fast = head
        slow = head
        while fast.next:#fast快只判断fast即可
            fast = fast.next
            if fast == slow:#只判断一次即可，若不相等，再走一步也不相等
                break
            fast = fast.next
            if not fast:
                return None
            slow = slow.next
        if not fast.next:
            return None
        L = 1#环中的元素个数
        fast = fast.next
        while fast != slow:
            L = L + 1
            fast = fast.next
        slow = head
        fast = head
        while L:
            fast = fast.next
            L = L - 1
        while fast != slow:
            fast = fast.next
            slow = slow.next
        return fast
```
