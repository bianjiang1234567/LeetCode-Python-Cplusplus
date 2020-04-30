# 题目
Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

#### Note

* You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.
* 0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100

#### Example 1:
```
Input: [3,1,5,8]
Output: 167
Explanation: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
             coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

# Python3 Solution
## 解题思路：
因为已经知道区间内的所有数字，那么就可以枚举这些数字，找到最优的那个i 。这类问题一般归为区间 dp 。那么现在就是，从最小区间开始计算，通俗的讲，先计算长度2的小区间，然后依次增加区间的长度。每次计算时，小区间就像一个滑动窗口，例如从长度为2的小区间作为一个窗口，从左向右依次滑动，求出所有小区间的最优解保存到dp[l][r]中。dp[left][right] = max(dp[left][right], nums[left] * nums[i] * nums[right] + dp[left][i] + dp[i][right])dp[left][right]，表示区间left-->right的最优解（不包括，left和right 两个边界）nums[left] * nums[i] * nums[right]，表示最后一个被戳破的气球的得分，为什么要乘上两个边界值那？因为它是最后一个被戳破的气球，整体来看，最后就剩下一个，就是乘上两个1，在子区间内，就是乘上两边的边界值了。max(dp[left][right], nums[left] * nums[i] * nums[right] + dp[left][i] + dp[i][right])，表示从区间内开始枚举i，计算出本次区间的最优解。二维动态规划
```
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        nums = [1] + [i for i in nums if i>0] + [1] #i小于0的情况扎破没有硬币
        n = len(nums) # 加上边界以后总的长度
        dp = [[0]*n for _ in range(n)] # 二维dp数组，dp[left][right]，表示区间left-->right的最优解（不包括，left和right 两个边界）只用右上三角
        for k in range(2,n):#区间长度从2开始
            for left in range(0, n-k):
                right = left + k
                for i in range(left+1, right):
                    dp[left][right] = max(dp[left][right], nums[left] * nums[i] * nums[right] + dp[left][i] + dp[i][right])
        return dp[0][n-1]
```
