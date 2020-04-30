# 题目

Given a singly linked list, return a random node's value from the linked list. Each node must have the same probability of being chosen.

Follow up:
What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?


#### Example 1:
```
// Init a singly linked list [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
solution.getRandom();
```

# Python3 Solution
## 解题思路：
简单的可以生成随机数，然后对链表长度取余数，选择节点，在节点长度未知的情况下，可以采用水塘采样的思想，即维持一个水塘，遍历节点，新来的节点以1/n的概率加入水塘，n为加上新来的节点总共遍历的节点数

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:

    def __init__(self, head: ListNode):
        """
        @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node.
        """
        self.head = head

    def getRandom(self) -> int:
        """
        Returns a random node's value.
        """
        i = 2
        res = self.head.val
        self.cur = self.head.next
        while(self.cur):
            j = random.randint(0, sys.maxsize) % i
            if(j == 0):
                res = self.cur.val
            i += 1
            self.cur = self.cur.next
        return res

# Your Solution object will be instantiated and called as such:
# obj = Solution(head)
# param_1 = obj.getRandom()
```

# C++ Solution
## 解题思路：
同上

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
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        head1 = head;
    }

    /** Returns a random node's value. */
    int getRandom() {
        int res = head1->val;
        ListNode* cur = head1->next;
        int i = 2;
        while(cur)
        {
            int j = rand() % i;
            if(j==0)
                res = cur->val;
            cur = cur -> next;
            i++;
        }
        return res;
    }
private:
    ListNode* head1;
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(head);
 * int param_1 = obj->getRandom();
 */
```
