# 题目
Convert a BST to a sorted circular doubly-linked list in-place. Think of the left and right pointers as synonymous to the previous and next pointers in a doubly-linked list.

We want to transform this BST into a circular doubly linked list. Each node in a doubly linked list has a predecessor and successor. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

The figure below shows the circular doubly linked list for the BST above. The "head" symbol means the node it points to is the smallest element of the linked list.

Specifically, we want to do the transformation in place. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. We should return the pointer to the first element of the linked list.

The figure below shows the transformed BST. The solid line indicates the successor relationship, while the dashed line means the predecessor relationship.

# C++ Solution
## 解题思路：
中序遍历
每次遍历采用两个指针作为记录，
head指向最开始，pre在中序遍历的时候作为节点与根节点建立连接关系,pre指的是中序遍历的前一个节点，node是本节点
```
class Solution {
public:
    Node* treeToDoublyList(Node* root) {
        if (!root) return NULL;
        Node *head = NULL, *pre = NULL;
        inorder(root, pre, head);
        pre->right = head;
        head->left = pre;
        return head;
    }
    void inorder(Node* node, Node*& pre, Node*& head) {
        if (!node) return;
        inorder(node->left, pre, head);
        if (!head) {
            head = node;
            pre = node;
        } else {
            pre->right = node;
            node->left = pre;
            pre = node;
        }
        inorder(node->right, pre, head);
    }
};

```
