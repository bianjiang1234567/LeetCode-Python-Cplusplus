# 题目

Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

#### Example 1:
```
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```

#### Example 2:
```
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

# Python3 Solution
## 解题思路：
从字符串开始向着两边去数，记录回文串的个数
```
class Solution:
    def countSubstrings(self, s: str) -> int:
        N = len(s)
        res = 0
        for i in range(2*N - 1):
            left = i // 2
            right = (i+1) // 2
            while left>=0 and right<N and s[left] == s[right]:
                res += 1
                left -= 1
                right += 1
        return res
```
