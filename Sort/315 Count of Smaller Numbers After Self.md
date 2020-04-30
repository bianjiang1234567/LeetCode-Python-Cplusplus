# 题目

You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].

#### Example 1:
```
Input: [5,2,6,1]
Output: [2,1,1,0]
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```

# Python3 Solution
## 解题思路：
这是求取逆序的题目，采用归并排序的方法，归并排序是一种稳定排序，相等的元素不交换位置，记录归并排序中右边的元素到左边的个数，就是右边的元素小于左边的元素的数量
```
class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        self.smaller = [0] * len(nums)#记录小的个数
        self.sort(list(enumerate(nums)))#利用枚举0 到 n-1对应smaller的下标
        return self.smaller

    def sort(self, num):
        half = len(num) // 2#一个元素不可以运行，两个元素可以运行
        if not half:#递归出口，返回1个元组
            return num
        left , right = self.sort(num[:half]), self.sort(num[half:])
        m , n = len(left), len(right)
        i = j = 0 #不可变对象标签会新建不可变对象
        while i < m or j < n:#0 到 n-1操作
            #实际分三种情况，都没有到头，其中一方到头 可以按照移动i或者移动j两种情况分
            if j == n or i < m and j < n and left[i][1]<=right[j][1]:#right中存在着逆序数字  剩下的情况都是j+1的情况且不用记录small 如果j超出 或者左边小于等于右边 放入左边
                self.smaller[left[i][0]] += j
                num[i+j] = left[i] #直接修改原list 将枚举元组放入
                i += 1
            else:
                num[i+j] = right[j]
                j += 1
        return num
```
