# 题目
In this problem, a rooted tree is a directed graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with N nodes (with distinct values 1, 2, ..., N), with one additional directed edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair [u, v] that represents a directed edge connecting nodes u and v, where u is a parent of child v.

Return an edge that can be removed so that the resulting graph is a rooted tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array.

#### Example1
```
Input: [[1,2], [1,3], [2,3]]
Output: [2,3]
Explanation: The given directed graph will be like this:
  1
 / \
v   v
2-->3
```

#### Example2
```
Input: [[1,2], [2,3], [3,4], [4,1], [1,5]]
Output: [4,1]
Explanation: The given directed graph will be like this:
5 <- 1 -> 2
     ^    |
     |    v
     4 <- 3
```

#### Note

* The size of the input 2D-array will be between 3 and 1000.
* Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.

# Python3 Solution
## 解题思路：
基于并查集思想，即RedundantConnection1的算法，
考虑一棵树，多出一条边，
将情况分为三种，
第一种有条边从任意非根节点指向根节点，这个时候所有节点入度为1，去掉最后出现的边，
第二种有条边从任意非根节点指向其父或祖父节点，有节点入度为2，去掉此边恢复树结构，
第三种有条边从任意非根节点指向非父或祖父节点，有节点入度为2，去掉这两条边中最后出现的。
第三种情况不能形成一个环，以区分第二三种情况。检测到环，为第二种情况，删除来自环里面的边。
若检测不到环，都要删除最后加入的边
```
class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        def find(a):#找到头父节点
            if p[a] == 0:
                return a#自己就是走到头的父节点
            p[a] = find(p[a])#继续找将所有节点接到最高节点下了
            return p[a]

        def detectCycle(edge):#尽量用不可变对象传参
            u,v=edge#不可变变量赋值 局部变量
            #u1, v1 = copy.deepcopy(u), copy.deepcopy(v)
            while u != v and u in parents:#u没有父节点或者uv相等的时候
                u = parents[u]#从u开始走
            return u == v

        candidates=[]#存储拥有两个节点的边
        parents={}#用字典记录父节点
        for u, v in edges:
            if v not in parents:
                parents[v] = u#没有父节点的时候加入父节点
            else:#存在两个父节点
                candidates.append([parents[v], v])
                candidates.append([u, v])
        #return [u, v]
        if candidates:#candidates非空则为第二三种情况，通过检测是否有圆来确定
            return [*candidates[0]] if detectCycle(candidates[0]) else candidates[1] #检测先出现的边，没有检测到圆去掉令一条边即可，检测到环删除环里面的边
        else:#常规方法解决并查集,若发现是连通集合则去掉此条边
            p = [0] * (len(edges)+1)#利用1到n来作为下标
            for u, v in edges:
                if find(v) == find(u):
                    return [u,v]#如果形成连通环即返回  没有形成连通环则证明是孤立的
                p[v] = u#连起来孤立的点

```
