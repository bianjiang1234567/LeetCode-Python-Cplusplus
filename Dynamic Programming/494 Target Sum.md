# 题目
You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

#### Note

* The length of the given array is positive and will not exceed 20.
* The sum of elements in the given array will not exceed 1000.
* Your output answer is guaranteed to be fitted in a 32-bit integer.

#### Example 1:
```
Input: nums is [1, 1, 1, 1, 1], S is 3.
Output: 5
Explanation:

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

# Python3 Solution
## 解题思路：
动态规划，不断往前递增，dp[i]存储加到数字i一共有几种方法，运用字典存储
```
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        if not nums:
            return 0
        m = {0 : 1}#加到0为1种方法
        for i in nums:#遍历数组全部加完
            memo = collections.defaultdict(int)
            for k in list(m.keys()):#遍历时不能修改字典大小，做一次复制再遍历，上一步的状态
                memo[k+i] = memo[k+i] + m[k]#累加方法数目，此步的方法等于当前方法数目+上一步的方法数目
                memo[k-i] = memo[k-i] + m[k]
            m = memo#修改上一步的状态
        return memo[S]
```
