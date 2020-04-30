# 题目
Given a non-empty binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root.


#### Example 1:
```
Input: [1,2,3]

       1
      / \
     2   3

Output: 6
```

#### Example 2:
```
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
```

# Python3 Solution
## 解题思路：
递归求解，
要组成路径，递归函数返回单边路径中最大的，
在当前节点中判断最大值存储到全局变量，如果有负值就加零变成单边路径

```
class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
        self.res = -2**31
        def maxpath(root):
            if not root:
                return 0
            left = max(maxpath(root.left), 0)#负值不加
            right = max(maxpath(root.right), 0)
            self.res = max(self.res, left+right+root.val)#加上当前节点组成路径,如果有负值就加零变成单边路径
            return max(left, right)+root.val#递归函数返回值就可以定义为以当前结点为根结点，到叶节点的最大路径之和，必须是单条路径
        maxpath(root)
        return self.res
```
#参数传递时，不可变对象要新建新的对象，不会更改原来对象
#在python中用self全局对象代替C++中的引用传递，或者列表等可变对象

# C++ Solution
## 解题思路：
同上
```
class Solution {
public:
    int maxPathSum(TreeNode* root) {
        int res = INT_MIN;
        helper(root, res);
        return res;
    }
    int helper(TreeNode* node, int& res) {
        if (!node) return 0;
        int left = max(helper(node->left, res), 0);
        int right = max(helper(node->right, res), 0);
        res = max(res, left + right + node->val);
        return max(left, right) + node->val;
    }
};

```
