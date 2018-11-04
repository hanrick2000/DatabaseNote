# Advanced Algorithms \(5\) - Dynamic Programming I

## 1. 动态规划初步

动态规划比较复杂，可能后面会有一个专题来详细研究动态规划，这里主要简单的提一下。

#### 什么时候使用动态规划？

动态规划一般用来解决以下三种问题：

* 最优解，也就是最大最小值
* 可行性问题，比如是否存在
* 方案总数问题

一般来说，动态规划问题每一个状态之间都存在着一定的联系，这样才可以使用动态规划来求解。

* 就我个人的理解来说，动态规划是在dfs基础之上的一种记忆化搜索，dfs也需要递归条件，也一般是一种状态转移，而动态规划将重复做的事情进行记忆化，因而减少了不必要的时间和空间耗费

#### 动态规划的四要素 ：

* **a. 状态** ：灵感，创造力，存储小规模问题的结果
  * 具体来说，是求极值，可行性还是计数
* **b. 方程** : 状态之间的转移过程
  * 状态之间的联系，怎么通过小的状态，来求得大的状态
* **c. 初始化** : 基本就是确认边界是什么
  * 最极限的小状态是什么, 起点
* **d. 答案** : 最终需要的是什么
  * 最大的那个状态是什么，终点

Review 

* 记得DFS的三要素么？定义，出口，拆解。和DP非常相似

#### 动态规划的利器 ：

* **滚动数组** ：不使用 n \* m 的空间去存储数据，而是使用 2 \* n , 从而节省了空间
  * mod : f\[i % 3\] = f\[\(i - 1\) % 3\] + f\[\(i - 2\) % 3\] 
    * 取模是四则运算中最慢的
  * old, new :  
    * ```python
      old, new = 0, 1
      old, new = new, 1 - old
      ```
* **记忆化搜索 :** 通过将访问过的结果存储以加速运算，典型是的斐波那契数列

## 2. 序列、矩阵型

#### [392. House Robber](https://www.lintcode.com/problem/house-robber/description)

基本所有的DP问题，只要想清楚那四要素就可以解决了，这个问题的本质其实是要不偷相邻的，突破点是如何转化为子问题：

* 状态 ： f\[i\] 表示前面i个房子能偷到的最大值
* 状态转移： f\[i\] = max\( f\[i-1\], f\[i-2\] + A\[i-1\]\)
  * 不偷最后一个，就是f\[i - 1\]
  * 偷最后一个获得A\[i -1\]，以及f\[i -2\]的最大值
* 初始化 ：f\[0\] = 0, f\[1\] = A\[0\]
  * 对于序列型，因为需要前面的和，n个状态需要n + 1的数组来存储
* 答案 : f\[n\]

```python
class Solution:
    """
    @param A: An array of non-negative integers
    @return: The maximum amount of money you can rob tonight
    """
    def houseRobber(self, A):
        n = len(A)
        # corner
        if not A and n == 0 :
            return 0
        
        f = []
        for i in range(n) :
            # init 
            if i == 0 or i == 1:
                f.append(A[i])
                continue 
            # state transfer
            f[i % 2] = max(f[(i - 1) % 2], f[(i - 2) % 2] + A[i]) 
            
        return f[(n - 1) % 2]
```

#### [534. House Robber II](https://www.lintcode.com/problem/house-robber-ii/description)



