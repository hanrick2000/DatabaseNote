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

这个题非常非常的有趣，但是容易写错的点也比较多，需要多些几遍，脑子比较清楚。

* 参考了第一个题，先将边界都加入heap，来找到短板
* 然后从短板出发，依次向里面推，并不断找短板，用这种思路来解决

这种类似棋盘类的问题，值得多练一下，比如word search， number of islands。

* 怎么样通过trapping rain water 1 拓展到这题的思路?
* 怎么样想到利用堆?
* 怎么想到由外向内遍历

```python
import heapq as h

class Solution:
    """
    @param heights: a matrix of integers
    @return: an integer
    """
    def trapRainWater(self, heights):
        # test corner case
        if not heights :
            return 0
        # init
        self.init(heights)
        
        water = 0
        while self.boarder:
            height, x, y = h.heappop(self.boarder)
            for (x1, y1) in self.adjacent(x, y) :
                water += max(0, height - heights[x1][y1])
                self.add(x1, y1, max(height, heights[x1][y1]))
        return water
            
    def init(self, heights) :
        # init heap and add outer to heap
        # x, y -> n, m
        self.n = len(heights)
        self.m = len(heights[0])
        self.boarder = []
        self.visited = set()
        # row
        for i in range(self.n) :
            self.add(i, 0, heights[i][0])
            self.add(i, self.m - 1, heights[i][self.m - 1])
        # col
        for j in range(self.m) :
            self.add(0, j, heights[0][j])
            self.add(self.n - 1, j, heights[self.n - 1][j])
    
    def add(self, x, y, height) :
        h.heappush(self.boarder, (height, x, y))
        self.visited.add((x, y))
        
    def adjacent(self, x, y) :
        adj = []
        for dx, dy in [(1, 0), (-1, 0), (0, 1), (0, -1)] :
            x1, y1 = x + dx, y + dy
            if 0 <= x1 < self.n and 0 <= y1 < self.m and (x1, y1) not in self.visited :
                adj.append((x1, y1))
        return adj
        
```

#### [81. Find Median from Data Stream](https://www.lintcode.com/problem/find-median-from-data-stream/description)

这里根据leetcode进行了修正，以求得真正的meidan，这里我用小大来区分左右两个heap，小就是左边，大就是右边。大是minheap， 小是maxheap。

* 当左右相等时，往large里面加元素，这个元素是左边的最大值
* 其他情况都是向small里面加，加large里面最小的元素

```python
from heapq import *

class Solution:
    """
    @param nums: A list of integers
    @return: the median of numbers
    """
    def medianII(self, nums):
        if not nums :
            return []
            
        self.large = []
        self.small = [] 
            
        medians = []
        for num in nums[0:] :
            self.addNum(num)
            medians.append(self.findMedian())
    
        return medians
        
    def addNum(self, num):
        if len(self.small) == len(self.large):
            heappush(self.large, -heappushpop(self.small, -num))
        else:
            heappush(self.small, -heappushpop(self.large, num))

    def findMedian(self):
        if len(self.small) == len(self.large):
            return int(-self.small[0]) 
        else:
            return int(self.large[0])
                
    def findMedian_2(self):
        if len(self.small) == len(self.large):
            return float(self.large[0] - self.small[0]) / 2.0
        else:
            return float(self.large[0])
```

#### [360. Sliding Window Median](https://www.lintcode.com/problem/sliding-window-median/description) 

这个题比较复杂，用到hashheap，这个是具体实现很复杂，先post到这里边看边学。

* 中位数怎么想到堆
* 窗口操作怎么分解
* How to get 带删除操作Heap
  * priority\_queue \(Java\) / Heapq \(Python\)
  * HashHeap
  * 可以用代替TreeSet\(JAVA\) vs Set\(C++\)

#### 补充：

Python的queue里面也有了priorityqueue，而目前使用的都是heapq，这里主要提一下两者的区别:

* heapq线程不安全
* priorityqueue在heapq的基础上建立，线程安全

## 2. Stack

#### [12. Min Stack](https://www.lintcode.com/problem/min-stack/description)

使用另一个min\_stack来同步记录每一时刻的最小值

```python
class MinStack:
    
    def __init__(self):
        self.stack = []
        self.min_stack = []

    """
    @param: number: An integer
    @return: nothing
    """
    def push(self, number):
        self.stack.append(number)
        if not self.min_stack or number <= self.min_stack[-1]:
            self.min_stack.append(number)

    """
    @return: An integer
    """
    def pop(self):
        number = self.stack.pop()
        if number == self.min_stack[-1]:
            self.min_stack.pop()
        return number
        
    """
    @return: An integer
    """
    def min(self):
        return self.min_stack[-1]
```

#### [575. Decode String](https://www.lintcode.com/problem/decode-string/description)

```python
class Solution:
    """
    @param s: an expression includes numbers, letters and brackets
    @return: a string
    """
    def expressionExpand(self, s):
        stack = []
        for c in s:
            if c == ']':
                strs = []
                while stack and stack[-1] != '[':
                    strs.append(stack.pop())
                
                # skip '['
                stack.pop()
                
                repeats = 0
                base = 1
                while stack and stack[-1].isdigit():
                    repeats += (ord(stack.pop()) - ord('0')) * base
                    base *= 10
                stack.append(''.join(reversed(strs)) * repeats)
            else:
                stack.append(c)
        
        return ''.join(stack)
```

