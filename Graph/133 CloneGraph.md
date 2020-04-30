# 题目
Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a val (int) and a list (List[Node]) of its neighbors.

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

# C++ Solution

## 解题思路1：
深度遍历图，陆续对邻居节点进行递归，用哈希表unordered_map记录已经遍历过的克隆过的节点，
将节点和副本进行对应，直接作为邻居返回以加速复制过程。

```
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;

    Node() {}

    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};

class Solution {
public:
    Node* cloneGraph(Node* node) {
        unordered_map<Node*, Node* > m;
        return(helper(node,m));
    }
    Node* helper(Node* node, unordered_map<Node*, Node*>& m) {
        if(node == nullptr) return nullptr;
        if(m.find(node)!=m.end()) return m[node];
        Node* clone = new Node(node->val);
        m[node] = clone;
        for(int i = 0; i < node->neighbors.size();i++)
        {
            clone->neighbors.push_back(helper(node->neighbors[i], m));
        }
        return clone;
    }
};
```

## 解题思路2：
广度优先遍历，利用队列记录每一层遍历的节点，并陆续从队列中提取进行复制。
利用unordered_map记录复制过的顶点，避免重复复制。
```
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;

    Node() {}

    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if(node == nullptr)
            return nullptr;
        qu.push(node);//原始节点
        Node* cloneNode = new Node(node->val, {});
        map[node] = cloneNode;//初始化
        while(!qu.empty())
        {//每一个循环做的事情是复制子节点并将子节点建立连接
            Node* node = qu.front();
            qu.pop();
            for(int i = 0; i < node->neighbors.size() ; i++)
            {
                if(map.find(node->neighbors[i]) == map.end())//判断子节点是否复制过
                {
                    Node* cloneNodeTmp = new Node(node->neighbors[i]->val, {});
                    map[node->neighbors[i]] = cloneNodeTmp;//告诉map子节点已经复制过了
                    qu.push(node->neighbors[i]);//放原始顶点子节点
                }
                map[node]->neighbors.push_back(map[node->neighbors[i]]);
            }
        }
        return cloneNode;
    }
private:
    queue<Node*> qu;
    unordered_map<Node*, Node*> map;//记录复制过的顶点
};
```

# Python3 Solution
## 解题思路1：
深度优先遍历
```
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = []):
        self.val = val
        self.neighbors = neighbors
"""

class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        dic = {}
        def dfs(node):
            if not node:
                return
            if node in dic.keys():
                return dic[node]
            clonenode = Node(node.val, [])
            dic[node] = clonenode
            for neighbor in node.neighbors:
                clonenode.neighbors.append(dfs(neighbor))
            return clonenode
        return dfs(node)
```

## 解题思路2：
广度优先遍历
```
class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        if not node:
            return
        nodeCopy = Node(node.val, [])
        queue = collections.deque([node])
        dic = dict({node: nodeCopy})#初始化
        while queue:
            curNode = queue.popleft()
            for neighbor in curNode.neighbors:
                if neighbor not in dic:
                    neighborCopy = Node(neighbor.val, [])
                    dic[neighbor] = neighborCopy
                    queue.append(neighbor)
                dic[curNode].neighbors.append(dic[neighbor])
        return nodeCopy
```
## 解题思路3：
迭代求解
```
def cloneGraph2(self, node):
    if not node:
        return
    nodeCopy = UndirectedGraphNode(node.label)
    dic = {node: nodeCopy}
    stack = [node]
    while stack:
        node = stack.pop()
        for neighbor in node.neighbors:
            if neighbor not in dic:
                neighborCopy = UndirectedGraphNode(neighbor.label)
                dic[neighbor] = neighborCopy
                dic[node].neighbors.append(neighborCopy)
                stack.append(neighbor)
            else:
                dic[node].neighbors.append(dic[neighbor])
    return nodeCopy
```
