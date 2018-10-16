# Advanced Algorithms \(1\) - Two Pointers Follow up

#### 406. Minimum Size Subarray Sum

双指针遍历，不回头。

```python
class Solution:
    """
    @param nums: an array of integers
    @param s: An integer
    @return: an integer representing the minimum size of subarray
    """
    def minimumSize(self, nums, s):
        n = len(nums)
        
        if s is None  or n == 0 :
            return -1
            
        left, right = 0, 0 
        sum = 0
        mini_length = n + 1
        
        for left in range(n) :
            
            while right < n and sum < s :
                sum += nums[right]
                right += 1
                
            if sum >= s :
                mini_length = min(mini_length, right - left) 
                
            sum -= nums[left]
            
        if mini_length == n + 1:
            return -1
            
        return mini_length
                
            
            
        
```

