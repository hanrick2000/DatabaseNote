# Data Structure & Algorithms \(5\) - DFS & Binary Tree

## 1. 第一类考察形态 ：求值、路径

这里的基本解决思路是递归，既然是递归，那么重要的在于找到递归条件，并给出corner case，然后再进行解决，这个整体的解决思路。

#### 596. Minimum Subtree

这里写的时候需要记录历史最小值、历史最小值的root，以及当前的值，并且固定leaf的值，这样一步一步解决这个问题。

```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the root of binary tree
    @return: the root of the minimum subtree
    """
    def findSubtree(self, root):
        min, subtree, sum = self.find(root)
        return subtree
        
    def find(self, root) :
        # 如果遇到叶节点时，必然左右不相加，而返回原节点的值
        if root is None :
            return sys.maxsize, None, 0 
            
        # 这里返回的左边历史最小值，现在的树root，以及现在的左边sum，并不断递归找到叶节点    
        left_min, left_subtree, left_sum = self.find(root.left)
        right_min, right_subtree, right_sum = self.find(root.right)
        
        # 含当前节点的sum值
        sum = left_sum + right_sum + root.val
       
        # 如果历史最小，不断返回最小点的root和值，并记录当前的sum
        if left_min == min(left_min, right_min, sum) :
            return left_min, left_subtree, sum
        if right_min == min(left_min, right_min, sum) :
            return right_min, right_subtree, sum
        
        # 如果当前最小，返回新的值        
        return sum, root, sum
        
```

#### 480. Binary Tree Paths

递归需要关注return的到底是什么，这个非常的重要，递归是比较难以解决的。

```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the root of the binary tree
    @return: all root-to-leaf paths
    """
    def binaryTreePaths(self, root):
        # no root
        if root is None :
            return []
        # no childs    
        if root.right is None and root.left is None :
            return [str(root.val)]
        
        paths = []    
        # left 
        for path in self.binaryTreePaths(root.left) : # type list
            paths.append(str(root.val) + '->' + path)
        # right
        for path in self.binaryTreePaths(root.right) :
            paths.append(str(root.val) + '->' + path)
            
        return paths
```

#### 88. Lowest Common Ancestor of a Binary Tree

这个题也很好玩

```python
def lowestCommonAncestor(self, root, A, B):
        if root is None:
            return None
            
        # 是 A， A下面有B
        if root is A or root is B:
            return root
        # 先无脑的丢给左右子树  
        left = self.lowestCommonAncestor(root.left, A, B)
        right = self.lowestCommonAncestor(root.right, A, B)
        
        # 当左右子树的结果都非空时，意味着A 和 B 一边一个
        if left is not None and right is not None:
            return root
            
       # 左子树有一个点，或左子树有LCA
        if left is not None:
            return left
        if right is not None:
            return right
            #左右子树啥都没有
        return None
```

## 2. 第二类考察形态 ：二叉树结构变化

453.

```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: a TreeNode, the root of the binary tree
    @return: nothing
    """
    last_node = None

    def flatten(self, root):
        if root is None:
            return
        
        if self.last_node is not None:
            self.last_node.left = None
            self.last_node.right = root
            
        self.last_node = root
        right = root.right
        self.flatten(root.left)
        self.flatten(right)
```

## 3. 第二类考察形态 ：二叉树搜索树

86. Binary Search Tree Iterator

```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None

Example of iterate a tree:
iterator = BSTIterator(root)
while iterator.hasNext():
    node = iterator.next()
    do something for node 
"""


class BSTIterator:
    """
    @param: root: The root of binary tree.
    """
    def __init__(self, root):
        self.stack = []
        while root != None:
            self.stack.append(root)
            root = root.left

    """
    @return: True if there has next node, or false
    """
    def hasNext(self):
        return len(self.stack) > 0

    """
    @return: return next node
    """
    def next(self):
        node = self.stack[-1]
        if node.right is not None:
            n = node.right
            while n != None:
                self.stack.append(n)
                n = n.left
        else:
            n = self.stack.pop()
            while self.stack and self.stack[-1].right == n:
                n = self.stack.pop()
        
        return node
```

