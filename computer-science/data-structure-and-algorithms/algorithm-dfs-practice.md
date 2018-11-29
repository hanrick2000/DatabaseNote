# Algorithm - DFS Practice

在刷dfs的过程中，感觉自己对dfs的掌握不是特别的好，这严重影响了我对于dp的理解，因为dp本质还是一种dfs，所以这里先刷一刷简单题，强制自己写出来一些思路，慢慢锻炼思维

[155. Minimum Depth of Binary Tree](https://www.lintcode.com/problem/minimum-depth-of-binary-tree/discuss) / [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

递归三要素：

* 定义：使用的树的形式，具体来讲需要取左右节点
* 出口：出口在于触及到叶节点，叶节点的特征是null，所以如果是null，那么返回就行
* 递归的拆解：当我们知道root的左边最短深度，和右边最短深度的时候，只需要选择两个中最小的，然后加1即可

```python

```

