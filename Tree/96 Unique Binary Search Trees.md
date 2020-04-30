# 题目
Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?


#### Example 1:
```
Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

# Python3 Solution
## 解题思路：
采用动态规划的思想，以i为根节点的树可以由以i-1和i+1为根节点的树的笛卡尔积获取n个值中，每个值都要作为根节点进行计算
```
class Solution:
    def numTrees(self, n: int) -> int:
        g=[1,1]
        for i in range(2,n+1):
            res = 0
            for j in range(len(g)):
                res += g[j]*g[i-1-j]
            g.append(res)
        return g[n]

```
