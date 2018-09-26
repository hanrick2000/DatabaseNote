# Data Structure & Algorithms \(6\) - DFS Combination

在非二叉树上的深度优先搜索（Depth-first Search）中，90%的问题，不是求组合（Combination）就是求排列（Permutation）。特别是组合类的深度优先搜索的问题特别的多。

#### 组合搜索问题 Combination

* 问题模型: 求出所有满足条件的“组合”。 
* 判断条件: 组合中的元素是顺序无关的。
* 时间复杂度：与 2^n 相关 （多层for循环）。

## 1. 全子集问题



1. 如何用最简单的递归方式来实现？
2. 如何用可以推广到排列类搜索问题的递归方式来实现？
3. 如果集合中有重复元素如何处理？
4. 
#### 简单递归

使用DFS组合的思路，对于每一位，考虑是否需要加入subset，是的话append再DFS，否的话pop再DFS

* 需要注意的是python的reference，这里不断的new 新的list，所以不存在之前的问题

```python
class Solution:
    """
    @param nums: A set of numbers
    @return: A list of lists
    """
    def subsets(self, nums):
        # write your code here
        self.result = []
        self.dfs(sorted(nums), [], 0)
        return self.result
    
    # 1. 递归的定义    
    def dfs(self, nums, subset, level) :
        # 3.出口
        if len(nums) == level :
            self.result.append(subset)
            return
        
        # 2.分拆
        # 加进去
        self.dfs(nums, subset + [nums[level]], level + 1)
        # 不加
        self.dfs(nums, subset, level + 1)
```

#### 回溯

使用DFS排列的思路，对于每一位，从这一位往后进行DFS，DFS结束后要pop（回溯）

```python
class Solution:
    """
    @param nums: A set of numbers
    @return: A list of lists
    """
    def subsets(self, nums):
        # write your code here
        self.result = []
        self.dfs(sorted(nums), [], 0)
        return self.result
    
    # 1. 递归的定义    
    def dfs(self, nums, subset, level) :
        # 3.出口
        self.result.append(subset[:])
        # 2.分拆
        for i in range(level, len(nums)) :
            # 向下走
            subset.append(nums[i])
            self.dfs(nums, subset, i + 1)
            # 回溯
            subset.pop(-1)
```

