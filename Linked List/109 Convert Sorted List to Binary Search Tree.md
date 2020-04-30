# 题目

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

# Python3 Solution
## 解题思路：
找到链表中值作为根节点，递归左右子树解决，

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        def convert(ls, start, end):
            if start > end:
                return None
            if start == end:
                return TreeNode(ls[start])
            mid = (start + end) // 2#每次递归左右相差不超过1的深度
            root = TreeNode(ls[mid])
            root.left = convert(ls, start, mid-1)#下标保证了二叉搜索树的正确性
            root.right = convert(ls, mid+1, end)
            return root

        if not head:
            return head
        ls = [head.val]
        while head.next:
            ls.append(head.next.val)
            head = head.next
        root = convert(ls, 0, len(ls)-1)
        return root
```
