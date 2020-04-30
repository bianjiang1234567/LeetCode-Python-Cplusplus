# 题目
Return the largest possible k such that there exists a_1, a_2, ..., a_k such that:

* Each a_i is a non-empty string;

* Their concatenation a_1 + a_2 + ... + a_k is equal to text;

* For all 1 <= i <= k,  a_i = a_{k+1 - i}.

#### Example 1:
Input: text = "ghiabcdefhelloadamhelloabcdefghi"

Output: 7

Explanation: We can split the string on "(ghi)(abcdef)(hello)(adam)(hello)(abcdef)(ghi)".

#### Example 2:
Input: text = "merchant"

Output: 1

Explanation: We can split the string on "(merchant)".

#### Example 3:
Input: text = "antaprezatepzapreanta"

Output: 11

Explanation: We can split the string on "(a)(nt)(a)(pre)(za)(tpe)(za)(pre)(a)(nt)(a)".

#### Example 4:
Input: text = "aaa"

Output: 3

Explanation: We can split the string on "(a)(a)(a)".

#### Constraints:

* text consists only of lowercase English characters.
* 1 <= text.length <= 1000

# Python3 Solution
## 解题思路：
利用rolling hash的思想，
每一个字符串以26为基，建立哈希值，比较哈希值是否相等判断字符串是否相等。
用两个指针，一个从前一个从后移动，并计算哈希值。
哈希值可以模一个质数，质数越大，冲突可能性越小，在哈希值相等的时候可以进行逐个字符串检查的工作来判断。

```
class Solution:
    def longestDecomposition(self, text: str) -> int:
        l, h = 0, len(text)-1#两个指针
        prime = 5003
        lenhash = 0
        hash1, hash2 = 0, 0
        res = 0
        while l < h:
            hash1 = (26 * hash1 + ord(text[l])-97) % prime
            hash2 = ((ord(text[h])-97) * pow(26,lenhash) + hash2) % prime#计算哈希值
            lenhash += 1
            if hash1 == hash2:
                flag = True
                for i in range(lenhash):
                    if text[l-i] != text[h+lenhash-1-i]:#为了防止哈希冲突，逐个字符比较
                        flag = False
                if flag:
                    res += 2
                    lenhash = 0
                    hash1 = 0
                    hash2 = 0
            l += 1
            h -= 1
        if lenhash>0 or lenhash==0 and l==h:#lenhash>0退出证明中间有字符或者字符串，第二种情况是类似"aaa",两侧正好相等
            res += 1
        return res
```
