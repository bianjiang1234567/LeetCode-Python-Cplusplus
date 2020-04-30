# 题目

Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

#### Example 1:
```
Input:nums = [1,1,1], k = 2
Output: 2
```

#### Note:

* The length of the array is in range [1, 20,000].
* The range of numbers in the array is [-1000, 1000] and the range of the integer k is [-1e7, 1e7].

# Python3 Solution
## 解题思路：
遍历一次，如果两组从0开始的连续和相减等于k，则证明存在一组连续和为k，字典的下标为和
```
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        if not nums:
            return 0
        Count, res, cur = collections.defaultdict(int), 0, 0#cur表示累加，
        Count[0] = 1#如果差等于k表明存在一组，因此Count初始化为{0:1}
        for i in nums:
            cur += i
            res += Count.get(cur-k, 0)#没有key 返回0
            Count[cur] += 1 #Count[cur] = count.get(cur, 0) + 1
        return res
```
