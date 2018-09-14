# Data Structure & Algorithms \(4\) - BFS & Topological Sorting

## 1. BFS在二叉树上的应用

#### 分层遍历宽度优先搜索模板 :

主要是层级遍历，具体问题我写在注释里面。

* Level是时刻会变的，因为每一层级不一样，所以需要放入循环
* 这里默认了树的节点和值的setitem方法

```python
from collections import deque
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
class Solution:
    """
    @param root: A Tree
    @return: Level order a list of lists of integer
    """
    def levelOrder(self, root):
        if root is None:                  # 基本检查，这里是二叉树
            return []
            
        queue = deque([root])             # 变成双向的queue
        result = []                       # 存储最后的结果
        while queue:                      # 当队列不为空 
            level = []                    # 存储这一层级
            for _ in range(len(queue)):
                node = queue.popleft()   
                level.append(node.val)    # 先将这一层的值
                if node.left:             # 如果左儿子存在，压入队列
                    queue.append(node.left)
                if node.right:            # 如果右儿子存在，压入队列  
                    queue.append(node.right)
            result.append(level)          # 合并
        return result
```

#### 几个Follow up的问题

#### 这里可不可以用栈？就是不再使用deque而是用list ?

* 从实现上面来说是可行的，但是需要两个stack
* python的list，如果pop\(0\)，计算复杂度是O\(n\)，所以一般都是pop\(-1\)默认的

#### 这里可不可以不用分层遍历？

* 是可以的，其实就是去掉level那里的一个循环，直接将值写入result

#### 常犯的错误

* 在写if的时候如果写在了for的外面，就会少掉一层，特别需要注意

#### 重要应用-序列化

## 2. BFS在图上的应用

使用宽度优先搜索 BFS 的版本。

第一步：找到所有的点  
第二步：复制所有的点，将映射关系存起来  
第三步：找到所有的边，复制每一条边

```python
"""
Definition for a undirected graph node
class UndirectedGraphNode:
    def __init__(self, x):
        self.label = x
        self.neighbors = []
"""
class Solution:
    """
    @param: node: A undirected graph node
    @return: A undirected graph node
    """
    def cloneGraph(self, node):
        # 1. check corner case
        if node is None :
            return node
        root = node
        # 2. get nodes    
        nodes = self.getNodes(node)
        
        # 3. change node to graph type
        mapping = {}
        for node in nodes:
            mapping[node] = UndirectedGraphNode(node.label)
        
        # 4. build relationship
        for node in nodes:
            newNode = mapping[node]
            for neighbor in node.neighbors :
                newNeighbor = mapping[neighbor]
                newNode.neighbors.append(newNeighbor)

        return mapping[root]
    
    def getNodes(self, node):
        queue = collections.deque([node])
        nodes = set([node])
        # traverse without levels
        while queue :
            headNode = queue.popleft()
            for neighbor in headNode.neighbors :
                if neighbor not in nodes :
                    nodes.add(neighbor)
                    queue.append(neighbor)
        
        return nodes
```

#### 隐式图 \(Implicit Graph\) 最短路径 - Word Ladder

分层遍历记录路径长度，每次遍历之后查看替换之后的下一个元素是不是在dict里面，不是就再换一次。

```python
class Solution:
    """
    @param: start: a string
    @param: end: a string
    @param: dict: a set of string
    @return: An integer
    """
    def ladderLength(self, start, end, dict):
        # 1. prepare
        dict.add(end)
        visited = set([start])
        queue = collections.deque([start])
        distance = 0 
        
        while queue :
            distance += 1
            for _ in range(len(queue)) :
                word = queue.popleft()
                if word == end :
                    return distance
                for next_word in self.get_next_word(word) :
                    if next_word not in dict or next_word in visited :
                        continue
                    queue.append(next_word)
                    visited.add(next_word)
                    
        return 0
                    
    def get_next_word(self, word) :
        words = []
        
        for i in range(len(word)) :
            left, right = word[:i], word[i + 1:]
            for char in 'abcdefghijklmnopqrstuvwxyz' :
                if char == word[i] :
                    continue
                words.append(left + char + right)
        
        return words
```

## 3. BFS在矩阵中的应用

