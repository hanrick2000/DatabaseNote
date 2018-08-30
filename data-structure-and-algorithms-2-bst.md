---
description: Lintcode
---

# Data Structure & Algorithms \(2\) - BST

#### 458. Last Position of Target

计算复杂度O\(n\)

没什么说的，就是遍历一遍整个数组就可以

```python
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def lastPosition(self, nums, target):
        
        # empty
        if len(nums) == 0 or target is None:
            return -1
        
        lastLocation = -1
        
        for i in range(len(nums)) :
            if target == nums[i] :
                lastLocation = i
        
        return lastLocation
```

计算复杂度O\(logn\)

就是处理左右指针相等的情况，这里比如说给\[1,2,2,4,5,5\]要找2的话，最后左右指针应该指的是\[2,2\]，这里就需要先输出右指针就可以。 

* mid = start + \(end - start\) // 2
* mid = \(start + end\) //2 
* 这两个是一样的，差别在于第一个防止stack overflow

```python
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def lastPosition(self, nums, target):
        
        if len(nums) == 0 :
            return -1
            
        start, end = 0, len(nums) - 1
        
        while start + 1 < end :
            mid = (start + end) // 2
            if nums[mid] <= target :
                start = mid
            else :
                end = mid
        
        if nums[end] == target :
            return end
        if nums[start] == target :
            return start
        return -1
```

#### 458. Last Position of Target

计算复杂度O\(n\)

没什么说的，就是遍历一遍整个数组就可以

```python
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def lastPosition(self, nums, target):
        
        # empty
        if len(nums) == 0 or target is None:
            return -1
        
        lastLocation = -1
        
        for i in range(len(nums)) :
            if target == nums[i] :
                lastLocation = i
        
        return lastLocation
```

#### 458. Last Position of Target

计算复杂度O\(n\)

没什么说的，就是遍历一遍整个数组就可以

```python
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def lastPosition(self, nums, target):
        
        # empty
        if len(nums) == 0 or target is None:
            return -1
        
        lastLocation = -1
        
        for i in range(len(nums)) :
            if target == nums[i] :
                lastLocation = i
        
        return lastLocation
```

#### 458. Last Position of Target

计算复杂度O\(n\)

没什么说的，就是遍历一遍整个数组就可以

```python
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def lastPosition(self, nums, target):
        
        # empty
        if len(nums) == 0 or target is None:
            return -1
        
        lastLocation = -1
        
        for i in range(len(nums)) :
            if target == nums[i] :
                lastLocation = i
        
        return lastLocation
```

#### 458. Last Position of Target

计算复杂度O\(n\)

没什么说的，就是遍历一遍整个数组就可以

```python
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def lastPosition(self, nums, target):
        
        # empty
        if len(nums) == 0 or target is None:
            return -1
        
        lastLocation = -1
        
        for i in range(len(nums)) :
            if target == nums[i] :
                lastLocation = i
        
        return lastLocation
```



