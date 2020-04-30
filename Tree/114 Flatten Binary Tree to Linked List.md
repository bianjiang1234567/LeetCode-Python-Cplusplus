# 题目
Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:

```
    1
   / \
  2   5
 / \   \
3   4   6

```
The flattened tree should look like:

```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```
# C++ Solution
## 解题思路：
先递归求解左右子树，
每一层递归做的事情是把左子树接到root的右子树，然后沿着右子树往下，最后再接上原先的右子树
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    void flatten(TreeNode* root) {
        if(!root) return;
        if(root->left) flatten(root->left);
        if(root->right) flatten(root->right);
        TreeNode* right = root->right;
        root->right=root->left;
        root->left=NULL;
        while(root->right) root = root->right;
        root->right = right;
    }
};
```

# Python3 Solution
## 解题思路：
同上
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def flatten(self, root: TreeNode) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        if not root:
            return None
        self.flatten(root.left)
        self.flatten(root.right)
        if root.left:
            right = root.right
            root.right = root.left
            root.left = None
            while root.right:
                root = root.right#改这一层root没关系，上一层root不会被修改。
            root.right = right
```
