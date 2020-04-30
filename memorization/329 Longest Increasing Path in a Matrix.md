# 题目

Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).


#### Example 1:
```
Input: nums =
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
]
Output: 4
Explanation: The longest increasing path is [1, 2, 6, 9].
```

#### Example 2:
```
Input: nums =
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
]
Output: 4
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

# Python3 Solution
## 解题思路1：
深度优先遍历，采用递归形式的动态规划，即memorization，自顶向下，因此找寻的是最长下降路径，动态规划数组记录当前路径的最大数量，dp[i][j] = 1 + max(dp[i-1][j],dp[i+1][j],dp[i][j-1],dp[i][j+1])

```
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        def dfs(i, j):
            val = matrix[i][j]
            if not dp[i][j]:#dp[i][j]为空进行计算,不为空可以直接返回,即memorization
                dp[i][j] = 1 + max(
                dfs(i-1,j) if i > 0 and val > matrix[i-1][j] else 0,#由于是自顶向下因此要大于
                dfs(i+1,j) if i < m-1 and val > matrix[i+1][j] else 0,
                dfs(i,j-1) if j > 0 and val > matrix[i][j-1] else 0,
                dfs(i,j+1) if j < n-1 and val > matrix[i][j+1] else 0)
            return dp[i][j]

        if not matrix or not matrix[0]:#输入不合法返回
            return 0
        m, n = len(matrix), len(matrix[0])#m行n列
        dp = [[0]*n  for _ in range(m)]
        return max(dfs(i,j) for i in range(m) for j in range(n))
```

## 解题思路2：
广度优先遍历，利用队列实现，与大水漫灌的思想一样，在队列中遍历完所有元素，统计满足条件的步数最多能走多远，需要先计算每个节点可行的入度，遍历完所有入度后放入队列中，入度代表了递增路径是否可以进入该点，若一直还有入度则可能是其他更远的路径有办法进入该点，即不要放入队列，看最后step能积累多远，即是最长路径
```
def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        if not matrix or not matrix[0]:
            return 0
        m, n = len(matrix), len(matrix[0])
        directions = [(1,0), (-1,0), (0,1), (0,-1)]
        hmap = {}#存储每个节点的可行入度，有可行的入度就可能有更长的路径，就需要走更长的路径，先不入队列，没有可行路径时相当于是地势最低的点
        queue = collections.deque()
        for i in range(m):
            for j in range(n):
                cnt = 0
                for dx, dy in directions:
                    x = i + dx
                    y = j + dy
                    if 0<=x<=m-1 and 0<=y<=n-1 and matrix[x][y]<matrix[i][j]:
                        cnt +=1
                hmap[(i,j)] = cnt
                if cnt==0:
                    queue.append((i,j))
        step = 0
        while queue: #队列为空则没有可行路径
            size = len(queue)
            for k in range(size):
                i, j = queue.popleft()
                for dx, dy in directions:
                    x = i + dx
                    y = j + dy
                    if 0<=x<=m-1 and 0<=y<=n-1 and matrix[x][y]>matrix[i][j]:
                        hmap[(x, y)] -= 1
                        if hmap[(x, y)] == 0:
                            queue.append((x,y))
            step +=1
        return step
```
