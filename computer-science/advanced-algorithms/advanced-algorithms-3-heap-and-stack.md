# Advanced Algorithms \(3\) - Heap & Stack

## 1. Heap

#### [363. Trapping Rain Water](https://www.lintcode.com/problem/trapping-rain-water/description)

非常非常巧妙的一个，先从左边开始遍历，然后再从右边开始遍历，有一点像mono stack，就是找到左右最高的点，然后看里面和和最高中最小的查即可。

* 这里尝试过将两个循环写在一起来减少代码量，但是不行
* 整体来说，时间复杂度并没有改变，所以代码量不太重要

```python
class Solution:
    """
    @param heights: a list of integers
    @return: a integer
    """
    def trapRainWater(self, heights):
        left, right = [], []
        left_max, right_max = -sys.maxsize, -sys.maxsize
        
        for height in heights :
            left_max = max(left_max, height)
            left.append(left_max)
            
        for height in reversed(heights) :
            right_max = max(right_max, height)
            right.append(right_max)
            
        n = len(heights)
        water = 0
        
        for i in range(n) :
            water += min(left[i], right[n - i - 1]) - heights[i]
            
        return water
```

#### [364. Trapping Rain Water II](https://www.lintcode.com/problem/trapping-rain-water-ii/description)



