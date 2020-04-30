# 题目
There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

# Python3 Solution
## 解题思路：
深度优先遍历，找到环就不行返回空，
若没找到环，则将orders在递归中填满
若遍历一次成功后就可以返回order了
```
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        #找到环则不行 存成邻接表
        graph = [[] for _ in range(numCourses)]
        for i,j in prerequisites:#j指向i
            graph[j].append(i)
        self.visited = [0] * numCourses # -1单次遍历过了  0   1表示好的节点无需继续遍历以加速
        self.orders = []
        for i in range(numCourses):
            if not self.visited[i] and self.dfsCircle(graph, i):#这里也可以加速，为1的好节点不用继续遍历了
                return []
        return self.orders[::-1]#倒序输出

    def dfsCircle(self, graph, i):
        if self.visited[i] == -1:#有环
            return True
        if self.visited[i] == 1:#无环 不用继续遍历了 加速
            return False
        self.visited[i] = -1
        for j in graph[i]:
            if self.dfsCircle(graph,j):
                return True
        self.orders.append(i)#这里是递归返回后添加的
        self.visited[i] = 1
        return False#经历过循环以后无环
```
