# 题目
There are a total of numCourses courses you have to take, labeled from 0 to numCourses-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

# Python3 Solution
## 解题思路：
深度优先遍历，找到环即可接力返回false
```
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        visited = [0] * (numCourses)#可改变的  -1访问过了找到了环   0    1此节点正常不用继续遍历 加速
        graph = [[] for _ in range(numCourses)]#临接矩形
        for i, j in prerequisites:#i,j变量依然有效  二维数组遍历
            graph[i].append(j)
        for i in range(numCourses):#找环
            if not self.dfs(graph, visited, i):#有环 不断访问并标记正常节点为1
                return False
        return True

    def dfs(self, graph, visited, i):
        if visited[i] == -1:#遍历过了
            return False
        if visited[i] == 1:#该节点好
            return True
        visited[i] = -1#此节点遍历了
        for j in graph[i]:#将该节点所有的子节点遍历完
            if not self.dfs(graph, visited, j):
                return False
        visited[i] = 1#此节点深度展开递归结束后  返回正常值1
        return True#默认到0无环
```

# C++ Solution
## 解题思路：

```
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int> > graph(numCourses, vector<int>(0));
        vector<int> visited(numCourses);
        //for(vector<int> a : prerequisites)
        for(int i = 0; i<prerequisites.size(); i++ )
        {
            vector<int> temp = prerequisites[i];
            graph[temp[1]].push_back(temp[0]);
        }
        for(int i = 0; i< numCourses; i++)
        {
            bool flag = canFinish(i,visited,graph);
            if(flag == false)
                return false;
        }
        return true;
    }

    bool canFinish(int i, vector<int>& visited, vector<vector<int>>& graph)
    {
        if(visited[i] == -1) return false;
        if(visited[i] == 1) return true;//起到加速作用 广度这一层都没事 返回后修正为一了
        visited[i] = -1;
        for(int a : graph[i])
        {
            bool flag = canFinish(a,visited,graph);
            if(flag == false) return false;//接力返回
        }
        visited[i] = 1;
        //visited[i] = 0;
        return true;
    }
};
```
