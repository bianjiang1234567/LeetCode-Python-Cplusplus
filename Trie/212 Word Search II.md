# 题目

Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

#### Example 1:
```
Input:
board = [
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]
```

#### Note

* All inputs are consist of lowercase letters a-z.
* The values of words are distinct.

# Python3 Solution
## 解题思路1：
使用dfs深度遍历，但要注意剪枝，不然无法通过，剪枝第一利用字典中不存在的字母则标记此路不通，第二利用判断是否是字典中的前缀，不是前缀则可以直接返回，第三将字典中已经遍历出现的单词可以弹出，避免出现重复的单词。
```
class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        if not board or not board[0] or not words:
            return []
        self.dic = { x : True for x in words}#set([ ])
        self.allwords = set([x for y in words for x in y])#构建字典中字母的集合
        m, n = len(board), len(board[0])
        self.res = []
        self.visited = [[0]*n for _ in range(m)]
        self.word = ""
        for i in range(m):
            for j in range(n):
                if not board[i][j] in self.allwords:#没有集合里面的字母，可以直接设置不能通行
                    self.visited[i][j] = False
                    continue
                self.dfs(board, i, j)
        return self.res

    def dfs(self, board, i, j):
        if not self.dic:#判断当前字典状态
            return
        if i<0 or j<0 or i>= len(board) or j >= len(board[0]) or self.visited[i][j] == True:#当前格子
            return
        Flag = False
        for x in self.dic.keys():
            if x.startswith(self.word):#未加当前字母的时候判断一下是否是字典中的前缀，如果不是就返回，空字符是任意字符串前缀
                Flag = True
                break
        if not Flag:
            return
        self.visited[i][j] = True#当前格子走过了
        self.word += board[i][j]#加上当前单词
        if self.dic.get(self.word, 0):
            self.res.append(self.word)
            #self.dic[self.word] = False#去掉字典中重复的单词
            self.dic.pop(self.word)#去掉字典中重复的单词
        #移动
        self.dfs(board, i-1, j)
        self.dfs(board, i, j-1)
        self.dfs(board, i+1, j)
        self.dfs(board, i, j+1)
        #回溯
        self.visited[i][j] = False
        self.word = self.word[:-1]
        return
```
## 解题思路2：

构建单词查找树，在dfs遍历中快速查找

```
class TrieNode():
    def __init__(self):
        self.children = {}#key是字母，连接着节点，value是节点
        self.isWorld = False#不是最后的单词节点

class Trie():
    def __init__(self):
        self.root = TrieNode()#节点

    def insert(self, word):
        if not word:
            return
        current = self.root
        for x in word:
            if not x in current.children.keys():#如果没有该节点则新建
                current.children[x] =  TrieNode()
            current = current.children[x]
        current.isWorld = True#是最后的子节点，存着单词

class Solution:
    #构建单词查找树，在dfs遍历中快速查找
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        def dfs(board, i, j, word, node):#不可变对象在闭包中被赋值会新建变量，可变对象不会，用户定义的对象是可变对象
            if i<0 or j<0 or i>=m or j>=n or visited[i][j]:#当前节点
                return
            if not node.children.get(board[i][j], 0):#当前节点的下一节点
                return
            nodenext = node.children[board[i][j]]#当前节点的下一节点
            visited[i][j] = True#当前节点
            word += board[i][j]#当前格子字母
            if nodenext.isWorld and not word in res:#当前节点的下一节点是不是尾巴 且不在结果中不用重复加入
                res.append(word)
                nodenext.isWorld = False#不再反复出现该单词
            dfs(board, i-1, j, word, nodenext)
            dfs(board, i+1, j, word, nodenext)
            dfs(board, i, j-1, word, nodenext)
            dfs(board, i, j+1, word, nodenext)
            word = word[:-1]
            visited[i][j] = False
            return

        res = []
        trie = Trie()
        allwords = set()#集合中统计所有字母
        for x in words:
            trie.insert(x)
            for y in x:
                allwords.add(y)
        m, n = len(board), len(board[0])
        visited = [[0]*n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                if not board[i][j] in allwords:
                    visited[i][j] = True#不再遍历该单词
                    continue
                dfs(board, i, j, "", trie.root)
        return res
```
