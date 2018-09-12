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

## 2. BFS在图上的应用

使用宽度优先搜索 BFS 的版本。

第一步：找到所有的点  
第二步：复制所有的点，将映射关系存起来  
第三步：找到所有的边，复制每一条边

```python
class Solution:
    def cloneGraph(self, node):
        root = node
        if node is None:
            return node
            
        # use bfs algorithm to traverse the graph and get all nodes.
        nodes = self.getNodes(node)
        
        # copy nodes, store the old->new mapping information in a hash map
        mapping = {}
        for node in nodes:
            mapping[node] = UndirectedGraphNode(node.label) 
        
        # copy neighbors(edges)
        for node in nodes:
            new_node = mapping[node]
            for neighbor in node.neighbors:
                new_neighbor = mapping[neighbor]
                new_node.neighbors.append(new_neighbor)
        
        return mapping[root]
        
    def getNodes(self, node):
        q = collections.deque([node])            # 建立deque  
        result = set([node])                     # 保证唯一性
        while q:
            head = q.popleft()
            for neighbor in head.neighbors:      # 复制邻居
                if neighbor not in result:
                    result.add(neighbor)
                    q.append(neighbor)
        return result
```



