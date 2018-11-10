# DP Types \(2\)

## 1. 坐标型动态规划

坐标型动态规划是一种比较简单的动态规划类型，主要是每一个坐标储存每一个值，然后转移方程主要是通过坐标来进行表示和运算的。

#### [115. Unique Paths II / ](https://www.lintcode.com/problem/unique-paths-ii/description)[63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/description/)

分析: 和之前的unique path基本一致，主要是增加了限制条件，也就是判断是否存在障碍。

#### 初始条件 ：

* 所有障碍点为0，如果障碍在第0行或者第0列，相应的后面的也为0，也就是存储前一个格子的状态
* 其他的基本条件并不改变

这个题使用了先初始化的方法，这里的小技巧是使用break，从而规避了之前小于的问题，感觉非常的巧妙，再次感觉算法的博大精深。

```python
class Solution:
    def uniquePathsWithObstacles(self, A):
        # init
        row, col = len(A), len(A[0])
        dp = [[0] * col for _ in range(row)]
        # row
        for i in range(row) :
            if A[i][0] :
                break
            else :
                dp[i][0] = 1
        # col
        for j in range(col) :
            if A[0][j] :
                break
            else :
                dp[0][j] = 1
        # other        
        for i in range(1, row) :
            for j in range(1, col) :
                if A[i][j]  :
                    dp[i][j] = 0
                else :
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1]           
        return dp[row - 1][col - 1]         
```

## 2. 序列型动态规划

#### [515. Paint House](https://www.lintcode.com/problem/paint-house/description) / [256. Paint House](https://leetcode.com/problems/paint-house/description/)

分析 : 从最后一步出发，第n个房子必然是红黄蓝三色之一，但是相邻的不同色，所以必然要记录之前染的颜色。

![](../../.gitbook/assets/screen-shot-2018-11-09-at-6.57.18-pm.png)

  
状态 ：

* f\[i - 1\]\[j\]，染成一种颜色的时候，前i - 1的花费

转移方程 ：

* f\[i\]\[0\] = min{f\[i-1\]\[1\]+ cost\[i-1\]\[0\], f\[i-1\]\[2\] + cost\[i-1\]\[0\]}

初始条件和边界：

* f\[0\]\[0\] = f\[0\]\[1\] = f\[0\]\[2\] = 0

这里还不是很会滚动数组，先不用，然后习惯了之后再换回来...

```python
class Solution:
    def minCost(self, costs):
        # init
        n = len(costs)
        dp = [[0] * 3 for _ in range(n + 1)]
        
        for i in range(1, n + 1) :
            for j in range(3) :
                # set max
                dp[i][j] = sys.maxsize
                for k in range(3) :
                    if k != j :
                        dp[i][j] = min(dp[i][j], dp[i - 1][k] + costs[i - 1][j])
        
        return min([dp[n][j] for j in range(3)])
```

#### 总结：

* 序列型动态规划是求前i个，有点像prefix sum
* 一般的求解因为条件很多，比较复杂，所以一般使用 **序列 + 状态**

## 3. 划分性动态规划

#### [512.Decode Ways](https://www.lintcode.com/problem/decode-ways/description) / [91. Decode Ways](https://leetcode.com/problems/decode-ways/description/)

#### 状态：

* 主要看最后的字符到底如何进行划分，也就是可以化为1个或者两个

#### 转移方程:

* f\[i\] = f\[i - 1\] + f\[i -2\]

#### 初始条件和边界：

* f\[0\] = 1

#### 计算顺序:

* 应该是从左到右

```python
class Solution:
    def numDecodings(self, s):
        n = len(s)
        # corner
        if n == 0 :
            return 0
        
        dp = [1] + [0] * n
        for i in range(1, n + 1) :
            # ith - first digit
            if s[i - 1] != '0' :
                dp[i] += dp[i - 1]
            # last two digits 
            if i >= 2 and ((s[i - 2] == '1') or (s[i - 2] == '2' and s[i - 1] <= '6')) :
                dp[i] += dp[i - 2]
        return dp[n]
```

