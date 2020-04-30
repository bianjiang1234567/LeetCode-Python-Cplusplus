# 题目
In this problem, a tree is an undirected graph that is connected and has no cycles.

The given input is a graph that started as a tree with N nodes (with distinct values 1, 2, ..., N), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair [u, v] with u < v, that represents an undirected edge connecting nodes u and v.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge [u, v] should be in the same format, with u < v.

#### Example1
```
Input: [[1,2], [1,3], [2,3]]
Output: [2,3]
Explanation: The given undirected graph will be like this:
  1
 / \
2 - 3
```

#### Example2
```
Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
Output: [1,4]
Explanation: The given undirected graph will be like this:
5 - 1 - 2
    |   |
    4 - 3
```

#### Note

* The size of the input 2D-array will be between 3 and 1000.
* Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.

#### Update (2017-09-26):

We have overhauled the problem description + test cases and specified clearly the graph is an undirected graph. For the directed graph follow up please see Redundant Connection II). We apologize for any inconvenience caused.

# Python3 Solution
## 解题思路：

采用并查集的思想，可以找到冗余的边
找自己的父节点作为代表，
若发现父节点相同，则两个子集连通，
一旦连通则应该去除连通的边，数字由小到大为连通顺序
```
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        parent = [0] * len(edges)#闭包中初始化所有的父节点为0  下标代表子节点

        def find(x): #寻找x的代表性父节点
            if parent[x] == 0: #若该子集元素代表性父节点为初始化的零则用自己作为代表
                return x
            parent[x] = find(parent[x])#寻找父节点的代表性父节点
            return parent[x]

        def union(x,y):
            rootx = find(x)
            rooty = find(y)
            if rootx == rooty:
                return True#这是连通的
            parent[rootx] = rooty #建立连通关系 其中rootx为小 rooty为大 形成有向图
            return False #两个区域不连通

        for x,y in edges:
            if union(x-1,y-1):
                return [x,y]
```

# C++ Solution
## 解题思路：
思路同上
```
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        parent = vector<int>(edges.size(), 0);
        for( auto edge : edges)
        {
            if(unionset(edge[0] - 1, edge[1] - 1))
                return vector<int>({edge[0], edge[1]});
        }
        return vector<int>(0,0);
    }

    int find(int x)
    {
        if(parent[x] == 0)
        {
            return x;
        }
        parent[x] = find(parent[x]);
        return parent[x];
    }

    bool unionset(int x, int y)
    {
        int rootx = find(x);
        int rooty = find(y);
        if(rootx == rooty)
            return true;
        parent[rootx] = rooty;
        return false;
    }

private:
     vector<int> parent;
};
```
