# 题目
Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

#### Example 1:
```
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
```

# Python3 Solution
## 解题思路：
对每一行用84 Largest Rectangle in Histogram的解法去求解，
```
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix:
            return 0
        heights = [0 for _ in range(len(matrix[0]))]
        heights.append(0)#加入0，方便最后的时候计算单调递增的矩形的面积
        res = 0
        for row in matrix:
            for j in range(len(matrix[0])):
                heights[j] = heights[j] + 1 if row[j] == '1' else 0
            res = max(res, self.largestRectangleArea(heights))
        return res

    def largestRectangleArea(self, heights):
        stack = [0]#保存索引从0开始
        res = 0
        for i in range(1, len(heights)):#从1到heights index -1
            if(heights[i]>=heights[stack[-1]]):#最大矩形的右边界不确定
                stack.append(i)#存储当前矩形下标，为当前最高矩形下标
            else:
                while stack and heights[i]<heights[stack[-1]]:
                    h = heights[stack[-1]]#当前最高矩形的高度
                    stack.pop()#这些矩形下标没用了需要弹出
                    w = i if not stack else i-1-stack[-1]#计算宽度找右边界。如果栈为空了，w=i最低矩形高度乘以总宽度
                    res = max(res, w*h)
                stack.append(i)
        return res
```
