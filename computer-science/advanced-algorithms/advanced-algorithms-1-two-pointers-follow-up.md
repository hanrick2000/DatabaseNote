# Advanced Algorithms \(1\) - Two Pointers Follow up

## 1. 同向双指针

基本模板

![](../../.gitbook/assets/screen-shot-2018-10-16-at-8.50.31-pm.png)

#### 406. Minimum Size Subarray Sum

双指针遍历，分为左右两个left和right

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

主要通过hashset来进行判断

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

