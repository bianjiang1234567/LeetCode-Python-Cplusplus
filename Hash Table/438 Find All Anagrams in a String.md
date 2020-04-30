# 题目

Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.

Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.

The order of output does not matter.

#### Example 1:
```
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

#### Example 2:
```
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

# Python3 Solution
## 解题思路：
利用collections.Counter函数，可以返回一个字典，key为元素，value为出现次数 或者使用collections.defaultdict
```
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        res = []
        pC = collections.Counter(p)
        sC = collections.Counter(s[:len(p)-1])
        for i in range(len(p)-1, len(s)):
            sC.update(s[i])#有默认值1  或者使用 sC[s[i]] += 1
            if pC == sC:
                res.append(i-len(p)+1)
            sC[s[i-len(p)+1]] -= 1
            if sC[s[i-len(p)+1]] == 0:
                sC.pop(s[i-len(p)+1])
        return res
```
