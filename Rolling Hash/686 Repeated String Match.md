# 题目
Given two strings A and B, find the minimum number of times A has to be repeated such that B is a substring of it. If no such solution, return -1.

For example, with A = "abcd" and B = "cdabcdab".

Return 3, because by repeating A three times (“abcdabcdabcd”), B is a substring of it; and B is not a substring of A repeated two times ("abcdabcd").

Note:
The length of A and B will be between 1 and 10000.

# Python3 Solution
## 解题思路：
理论的下界是ceil(len(B)/len(A))，先检查是否在此，若不在则必在ceil(len(B)/len(A))+1中，因为左边和右边各多补出一个A，若再加A则是重复。

可以用到rolling hash的思想是在查找子串的过程中，在原字符串中，利用字符重叠，快速计算哈希值来比较是否相等。

```
class Solution:
    def repeatedStringMatch(self, A: str, B: str) -> int:
        lowerbound = ceil(len(B)/len(A))
        for i in range(2):
            if B in A*(lowerbound+i):
                return lowerbound+i
        return -1
```
