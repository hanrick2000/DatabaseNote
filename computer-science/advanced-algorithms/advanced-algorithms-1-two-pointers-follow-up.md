# Advanced Algorithms \(1\) - Two Pointers Follow up

## 1. 同向双指针

基本套路就是下图，稍微修改了下形成自己的风格。

![](../../.gitbook/assets/screen-shot-2018-10-16-at-8.50.31-pm.png)

```python
left, right = 0, 0
for left in range(n):
    while right < n and subarray 不满足：
        right += 1 # 拓宽序列 注意right不回头
    
    if subarray 满足条件：
        看下是否满足条件
    
    将nums[left]移除 # remove left
```

#### 406. Minimum Size Subarray Sum

双指针遍历，分为左右两个left和right，控制两个条件

* sum需要大于threshold
* mini\_len要最小

控制这两个带入模板就可以了

```python
class Solution:
    """
    @param nums: an array of integers
    @param s: An integer
    @return: an integer representing the minimum size of subarray
    """
    def minimumSize(self, nums, s):
        # get maximum length
        n = len(nums)
        # test case
        if n == 0 or s is None :
            return -1
        # same direction two pointers
        left, right = 0, 0
        sum = 0
        mini_len = n + 1
        for left in range(n) :
            # get right value
            while right < n and sum < s:
                sum += nums[right]
                right += 1
            # find
            if sum >= s :
                mini_len = min(mini_len, right - left)
            # remove left 
            sum -= nums[left]
        # not find 
        if mini_len == n + 1:
            return  -1
        
        return mini_len
```

#### 384. Longest Substring Without Repeating Characters

主要通过hashset来进行判断，先找限制条件

* 最长的 需要 longest记录
* 不重复需要hashset判断

```python
class Solution:
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        n = len(s)
        left, right = 0, 0
        max_val, hashmap = 0, {}
        
        for left in range(n) :
            while right < n and s[right] not in hashmap :
                hashmap[s[right]] = True
                right += 1
            max_val = max(max_val, right - left)
            hashmap.pop(s[left])
        return max_val
```

#### 32. Minimum Window Substring

主要是要用length和hashmap来控制字符串的数字，详细见下。

* 需要hashset形成target的char作为限制条件
* 需要保证length最小
* 需要注意hashset在get和pop的时候需要确认顺序

```python
class Solution:
    """
    @param source : A string
    @param target: A string
    @return: A string denote the minimum window, return "" if there is no such a string
    """
    def minWindow(self, source, target):
        matchStr = ''
        # corner case
        if source is None :
            return matchStr
        # init
        targetHash, newHash = self.newhash(target), {}
        targetLen, matchLen = len(targetHash), 0
        # denote l, r - left, right
        left, right = 0, 0
        n, miniLen = len(source), len(source) + 1
        
        for left in range(n) :
            # right move to find the string
            while right < n and matchLen < targetLen :
                char = source[right]
                if char in targetHash:
                    newHash[char] = newHash.get(char, 0) + 1
                    if targetHash[char] == newHash[char] :
                        matchLen += 1
                right += 1
            # if find, confirm min
            if right - left < miniLen and matchLen == targetLen :
                miniLen = right - left
                matchStr = source[left:right]
            # move left pointer and del related info
            char = source[left]
            if char in targetHash :
                if targetHash[char] == newHash[char] :
                    matchLen -= 1
                newHash[char] -= 1
        return matchStr
    
    def newhash(self, target) :
        hashset = {}
        for char in target :
            hashset[char] = hashset.get(char, 0) + 1
        return hashset
```

#### 386. Longest Substring with At Most K Distinct Characters

和上一个题差不多，基本同一个套路

* 主要需要注意控制条件，小于k或者在hash里面

```python
class Solution:
    """
    @param s: A string
    @param k: An integer
    @return: An integer
    """
    def lengthOfLongestSubstringKDistinct(self, s, k):
        if s is None or k == 0 :
            return 0
            
        left, right = 0, 0
        n, longest = len(s), 0
        hashset = {}
        
        for left in range(n) :
            while right < n and (len(hashset) < k or s[right] in hashset) :
                hashset[s[right]] = hashset.get(s[right], 0) + 1
                right += 1
        
            longest = max(longest, right - left)
            
            if s[left] in hashset :
                hashset[s[left]] -= 1
                if hashset[s[left]] == 0 :
                    hashset.pop(s[left])
            
        return longest
```

## 2. 查找类问题

查找类问题大多可以二分，这里本质还是二分加双指针，核心算法是quick select，这个非常重要。这里比较典型的是top k问题。

### 2.1 一维查找

#### 461. Kth Smallest Numbers in Unsorted Array

这个主要是在原有的merge sort基础上进行优化，原有的merge sort是O\(nlogn\)，这里通过二分优化到了O\(n\)

```python
class Solution:
    """
    @param: k: An integer
    @param: nums: An integer array
    @return: kth smallest element
    """
    def kthSmallest(self, k, nums):
        return self.quick_select(0, len(nums) - 1, nums, k - 1)
        
    def quick_select(self, start, end, nums, k) :
        # corner case
        if start == end :
            return nums[start]
            
        left, right = start, end
        pivot = nums[(start + end) // 2]
        # quick sort
        while left <= right :
            while left <= right and nums[left] < pivot :
                left += 1
            while left <= right and nums[right] > pivot :
                right -= 1
                
            if left <= right :
                nums[left], nums[right] = nums[right], nums[left]
                # start, right, left, end
                left += 1
                right -= 1
        # half-half        
        if start <= right and right >= k :
            self.quick_select(start, right, nums, k)
        if left <= end and left <= k :
            self.quick_select(left, end, nums, k)
            
        return nums[k]
```

### 2.2 二维查找

这里主要的一种核心的思想是K merged sorted array，即K路归并算法，这里主要就是follow up这一种思路。

假设给了N个平均长度为M的数组，这里要分类讨论一下：

* 所有的数组都是无序的
  * 全部放到一个array之中进行quick select : O\(MN\) **最优**
  * 对每个数组进行排序再利用最大堆：O\(nmlogm + klogn\)
    * 每个排序mlogm，一共n个，堆的大小是n，取k次
* 所有的数组是有序的
  * 直接使用最大堆，类似多路归并算法：O\(klogn + N\)
    * 堆化N，pop k次 klogn
  * 更快的解法，二分答案：O\(NlogMlog range \) **最优**
    * 每次都要看n个数组的元素，每次二分原有数组，每次再看range的范围

#### 543. Kth Largest in N Arrays

这里使用了quick select的算法。

```python
class Solution:
    """
    @param arrays: a list of array
    @param k: An integer
    @return: an integer, K-th largest element in N arrays
    """
    def KthInArrays(self, arrays, k):
        # write your code here
        merge_array = []
        for array in arrays :
            merge_array.extend(array)
        n = len(merge_array) - 1
        
        return self.quick_select(0, n, merge_array, k -1)
            
    def quick_select(self, start, end, nums, k):
        if start == end :
            return nums[start]
            
        left, right = start, end
        pivot = nums[(start + end) // 2]
        
        while left <= right :
            while left <= right and nums[left] > pivot :
                left += 1
            while left <= right and nums[right] < pivot :
                right -= 1
            
            if left <= right :
                nums[left], nums[right] = nums[right], nums[left]
                # start, right, left, end
                left += 1
                right -= 1
            
        if start <= right and right >= k :
            self.quick_select(start, right, nums, k)
        if left <= end and left <= k :
            self.quick_select(left, end, nums, k)
            
        return nums[k]
```

使用heap的做法

```python
import heapq
class Solution:
    """
    @param arrays: a list of array
    @param k: An integer
    @return: an integer, K-th largest element in N arrays
    """
    def KthInArrays(self, arrays, k):
        # sort array
        sorted_array = []
        for array in arrays :
            if array :
                sorted_array.append(sorted(array, reverse = True))
        # heapify
        heap = []
        for index, array in enumerate(sorted_array):
            heap.append((-array[0], index, 0))
        heapq.heapify(heap)
        # pop k times
        for _ in range(k) :
            val, x, y = heapq.heappop(heap)
            if y + 1 < len(sorted_array[x]) :
                heapq.heappush(heap, (-sorted_array[x][y + 1], x, y + 1))
        
        return -val
```

#### 1272. Kth Smallest Element in a Sorted Matrix

用前面任意一个解法都可以秒

```python
import heapq
class Solution:
    """
    @param matrix: List[List[int]]
    @param k: a integer
    @return: return a integer
    """
    def kthSmallest(self, matrix, k):
        # heapify
        heap = []
        for index, array in enumerate(matrix) :
            heapq.heappush(heap, (array[0], index, 0))
        heapq.heapify(heap)
        # pop k times
        for _ in range(k) :
            val, x, y = heapq.heappop(heap)
            if y + 1 < len(matrix[x]) :
                heapq.heappush(heap, (matrix[x][y + 1], x, y + 1))
                
        return val
```

#### 465. Kth Smallest Sum In Two Sorted Arrays

使用记忆化搜索将所有之前访问过的坐标放入visited，避免下次再次访问从而影响了最小的值。

```python
import heapq
class Solution:
    """
    @param A: an integer arrays sorted in ascending order
    @param B: an integer arrays sorted in ascending order
    @param k: An integer
    @return: An integer
    """
    def kthSmallestSum(self, A, B, k):
        # heapify
        heap = [(A[0] + B[0], 0, 0)]
        heapq.heapify(heap)
        visited = {(0, 0) : True}
        # pop k times
        for _ in range(k):
            val, a, b = heapq.heappop(heap)
            if a + 1 < len(A) and (a + 1, b) not in visited :
                heapq.heappush(heap, (A[a + 1] + B[b], a + 1, b))
                visited[(a + 1, b)] = True
            if b + 1 < len(B) and (a, b + 1) not in visited  :
                heapq.heappush(heap, (A[a] + B[b + 1], a, b + 1))
                visited[(a, b + 1)] = True
                
        return val
```

