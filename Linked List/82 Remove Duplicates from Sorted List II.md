# 题目

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

Return the linked list sorted as well.

# Python3 Solution
## 解题思路：
通过3个指针preNode,curNode,nextNode进行删除重复节点操作

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head:
            return head
        dummy = preNode = ListNode(0)#顺序是最右边赋值给dummy，dummy再赋值给preNode
        preNode.next = head#防止头部删除，preNode自身会改变，也会修改ListNode(0)的next
        curNode = head#初始化  引用的关系
        while curNode.next:
            nextNode = curNode.next
            if curNode.val != nextNode.val:
                preNode = curNode
                curNode = nextNode
            else:
                while nextNode and curNode.val == nextNode.val:#从左往右判断，若第一个表达式为空则不执行第二个表达式
                    nextNode = nextNode.next
                if not nextNode:#走到末尾了 删完返回
                    preNode.next = None
                    return dummy.next
                preNode.next = nextNode#重新连接pre的下一个节点
                curNode = nextNode#重新初始化curNode
        return dummy.next
```
