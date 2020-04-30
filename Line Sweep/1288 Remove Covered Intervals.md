# 题目

Given a list of intervals, remove all intervals that are covered by another interval in the list. Interval [a,b) is covered by interval [c,d) if and only if c <= a and b <= d.

After doing so, return the number of remaining intervals.


#### Example 1:
```
Input: intervals = [[1,4],[3,6],[2,8]]
Output: 2
Explanation: Interval [3,6] is covered by [2,8], therefore it is removed.
```

#### Constraints:

* 1 <= intervals.length <= 1000
* 0 <= intervals[i][0] < intervals[i][1] <= 10^5
* intervals[i] != intervals[j] for all i != j

# Python3 Solution
## 解题思路：
对intervals排序，先按照第一个元素升序排序，再按照第二个元素降序排序，遍历时统计第二个元素小于目前最大的第二个元素的个数即被覆盖的元素，sort和sorted是稳定排序，python的布尔值True和False相当于1和0  0==False为True   1==True为True
```
class Solution:
    def removeCoveredIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key = lambda x : (x[0], -x[1]))
        right = intervals[0][1]
        res = 0
        for x in intervals[1:]:#遍历的时候保证了左边界是升序的
            res += x[1] <= right#若小于等于目前最大的右边界，则计数加1
            right = max(right, x[1])
        return len(intervals) - res#剩余多少不包含的
```
