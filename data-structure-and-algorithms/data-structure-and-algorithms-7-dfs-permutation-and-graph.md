# Data Structure & Algorithms \(7\) - DFS Permutation & Graph



全排列问题是“排列式”深度优先搜索问题的鼻祖。很多搜索的问题都可以用类似全排列的代码来完成。包括我们前面学过的全子集问题的一种做法。

#### 全排列搜索问题

* 问题模型: 求出所有满足条件的“排列”
* 判断条件: 组合中的元素是顺序“相关”的
* 时间复杂度: 与 n! 相关。

这一小节中我们需要掌握：

## 1.普通的全排列问题

全排列问题和组合问题的代码模板基本一样，差异在于全排列需要用每次遍历全数组，因此需要用visited或者哈希表来控制不要访问这个元素。而组合天然可以使用index来控制不回头即可。

```python
class Solution:
    """
    @param: nums: A list of integers.
    @return: A list of permutations.
    """
    def permute(self, nums):
        # 初始化
        self.results = []
        self.dfs([], nums)
        return self.results
        
    def dfs(self, path, nums) :
        # 达到长度
        if len(path) == len(nums) : 
            self.results.append(path[:])
            return
        
        for i in range(len(nums)) :
            # 没有visited
            if nums[i] not in path :
                # 回溯
                path.append(nums[i])
                self.dfs(path, nums)
                path.pop()
```



## 2. 有重复的全排列问题

全排列问题是一样的，基本几个要点:

* 先排序，不影响时间复杂度
* 使用visited来记录是否走过
* 使用和前一个元素的比较方法来判断是否重复的数字走过

```python
class Solution:
    """
    @param: :  A list of integers
    @return: A list of unique permutations
    """

    def permuteUnique(self, nums):
        self.results = []
        # 引入Visited来记录
        self.visited = {i: False for i in range(len(nums))}
        self.dfs([], sorted(nums))
        return self.results
        
    def dfs(self, path, nums) :
        if len(path) == len(nums) :
            self.results.append(path[:])
            return
        
        for i in range(len(nums)) :
            if self.visited[i] :
                continue
            # 选代表，只选符合的
            if i != 0 and nums[i] == nums[i - 1] and self.visited[i - 1]:
                continue
            # 回溯
            self.visited[i] = True
            path.append(nums[i])
            self.dfs(path, nums)
            path.pop()
            self.visited[i] = False    
```

## 3 非递归的全排列算法

