# 题目

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

# Python3 Solution
## 解题思路：
利用python中的优先队列，放入优先队列按照优先级弹出
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        self._queue = []
        self._index = 0
        for n in lists:
            while n:
                heapq.heappush(self._queue, (n.val, self._index, n))#_index在相同优先级的情况下可以进行比较，按照插入的顺序排序
                self._index += 1
                n = n.next
        dummy = node = ListNode(0)
        for i in range(len(self._queue)):
            node.next = heapq.heappop(self._queue)[2]#每次弹出最小的元素
            node = node.next
        return dummy.next
```
