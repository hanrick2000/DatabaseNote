# DP Introduction \(1\)

## 1. 简介

![](../../.gitbook/assets/screen-shot-2018-11-04-at-3.08.27-pm.png)

从个人角度来讲，动态规划和递归是非常非常相似的，但是又存在着微小的区别 ，一般来说动态规划有以下特点: 

* 计数
  * 有多少种方法走到右下角
  * 有多少种方法选出k个数，使得和是Sum
* 求最大最小值
  * 从左上角走到右下角的最大数字和
  * 最长上升子序列长度
* 求存在性
  * 取石子游戏，判断先手是否必胜
  * 能不能选出k个数,使得和为Sum

## 2. 组成部分

这里通过练习coin change，来穿插讲解了如何破解动态规划问题 ：

#### [669. Coin Change](https://www.lintcode.com/problem/coin-change/description)

这个题属于求最大最小值的动态规划问题，要求用最少的硬币数付钱，这里首先要想的是如何确认状态。

**确认状态**

* 状态在动态规划中的作用属于定海神针
* 简单地说，解动态规划的时候需要开一个数组，数组的每一个元素f\[i\]或者f\[i\]\[j\] 代表着什么
  * 类似于解数学题中，X、Y、Z代表着什么
* 确认状态需要两个意识
  * 最后一步是怎么样的
  * 如何拆解为子问题

在这个题之中，最优策略一定是 a1, a2, ... , ak 面值加起来是一定的，因此最后一定有一枚硬币 ak 等待着被加， 如何这个成立，那么前面的硬币和一定是 27 -  ak

![](../../.gitbook/assets/screen-shot-2018-11-04-at-3.27.56-pm.png)

#### 最后一步

* 这里忽略了前面k - 1枚硬币是怎么拼出27 - ak的，但是确定前面拼出了 27 - ak
* 因为要最优策略，所以拼出27 - ak的硬币一定要小，不然就不是最优了

#### 子问题和转移方程

* 这里逐渐变成了，求最少用多少硬币可以拼出 27 - ak
* 这里设 状态 f\(x\) = 最少用多少硬币拼出x 
* 最后这里只有三种可能，2，5，7
  * f\(x\) =min\( f\(x - 2\) , f\(x - 5\) , f\(x - 7\)\) + 1

```python
# 递归解法
# 主要问题是重复计算
def f(x) :
    if x == 0 : 
        return 0
    if x >= 2 :
        res = max(f(x - 2) + 1, res)
    if x >= 5 :
        res = max(f(x - 5) + 1, res)
    if x >= 7 :
        res = max(f(x - 7) + 1 , res)
    return res
```

因此对应的优化方法，就是将计算过的结果保留下来

#### 初试条件和边界情况

* 根据转移方向，如果x - 2, x - 5, x - 7 小于0怎么办
* 如果拼不出来，就设f\[x\] 为正无穷
* 初始条件 f\[0\] = 0

#### 计算顺序

* 应该从小到大，因为计算f\[x\]时，应该已经有f\[x - 2\], f\[x - 5\], f\[x - 7\]
* 每一步尝试三种硬币，一共27步
  * 同递归相比，没有任何重复

#### 小结动态规划的四要素 ：

* 确定状态
  * 最后一步
  * 化成子问题
* 转移方程
  * f\[x\] = min\(f\[x - 2\], f\[x - 5\], f\[x -7\]\) + 1
* 初始条件和边界情况
  * f\[0\] = 0, f\[y\] = 正无穷 if y不能拼出
* 计算顺序
  * f\[0\], f\[1\], f\[2\]

```python
class Solution:
    """
    @param coins: a list of integer
    @param amount: a total amount of money amount
    @return: the fewest number of coins that you need to make up
    """
    def coinChange(self, coins, amount):
        
        n = len(coins)
        # test corner
        if not coins or n == 0 :
            return -1
        # init
        dp = [0] + [sys.maxsize] * amount
        # traverse
        for i in range(1, amount + 1) :
            stack = [dp[i - c] if i >= c else sys.maxsize for c in coins]
            dp[i] = min(stack) + 1
                
        return dp[-1] if dp[-1] < sys.maxsize else -1
```

