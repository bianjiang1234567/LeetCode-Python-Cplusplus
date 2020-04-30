# 题目

Given a function rand7 which generates a uniform random integer in the range 1 to 7, write a function rand10 which generates a uniform random integer in the range 1 to 10.

Do NOT use system's Math.random().


#### Example 1:
```
Input: 1
Output: [7]
```

#### Example 2:
```
Input: 2
Output: [8,4]
```

#### Example 3:
```
Input: 3
Output: [8,1,10]
```

#### Note:

* rand7 is predefined.
* Each testcase has one argument: n, the number of times that rand10 is called.

# Python3 Solution
## 解题思路1：
使用拒绝采样的思想，先利用rand7生成rand49，rand49 = (rand7-1)*7 + rand7(), (rand7-1)*7等概率生成0,7,14,21,28,35,42,再加上rand7等概率生成rand49，在rand49里面利用拒绝采样的思想找到rand40，即大于40拒绝重新采样，小于等于40接受，从rand40生成rand10，rand10 = rand40 % 10 + 1
```
# The rand7() API is already defined for you.
# def rand7():
# @return a random integer in the range 1 to 7

class Solution:
    def rand10(self):
        """
        :rtype: int
        """
        while 1:
            num = (rand7() - 1) * 7 + rand7()
            if num <= 40:
                return num % 10 + 1
```

# C++ Solution
## 解题思路：
同上

```
// The rand7() API is already defined for you.
// int rand7();
// @return a random integer in the range 1 to 7

//使用拒绝采样的思想，先利用rand7生成rand49，rand49 = (rand7-1)*7 + rand7(), (rand7-1)*7等概率生成0,7,14,21,28,35,42,再加上rand7等概率生成rand49，在rand49里面利用拒绝采样的思想找到rand40，即大于40拒绝重新采样，小于等于40接受，从rand40生成rand10，rand10 = rand40 % 10 + 1
class Solution {
public:
    int rand10() {
        while(1)
        {
            int num = (rand7() - 1) * 7 + rand7();
            if (num <= 40)
                return num % 10 + 1;
        }
    }
};
```
