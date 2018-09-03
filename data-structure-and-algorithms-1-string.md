---
description: Lintcode
---

# Data Structure & Algorithms \(1\) - String



#### 428. Pow\(x, n\)

计算复杂度O\(logn\)

需要注意的是这个题的corner case很多，需要把问题想清楚。

```python
class Solution:
    """
    @param x: the base number
    @param n: the power number
    @return: the result
    """
    def myPow(self, x, n):
        # write your code here
        ans, multiple, m = 1, x, abs(n)
        
        # corner case
        if x == 0 :
            return 0
        # fast mode    
        if n == 0 :
            return 1
        if n == 1 :
            return x
        
        while m > 0 :
            
            if m % 2 == 1:
                ans *= multiple 
            
            multiple = multiple * multiple
            m = m // 2
            
        # check negative value
        if n > 0 :
            return ans 
        else :
            return 1 / ans 428. Pow(x, n)
计算复杂度O(logn)
需要注意的是这个题的corner case很多，需要把问题想清楚。
class Solution:
    """
    @param x: the base number
    @param n: the power number
    @return: the result
    """
    def myPow(self, x, n):
        # write your code here
        ans, multiple, m = 1, x, abs(n)
        
        # corner case
        if x == 0 :
            return 0
        # fast mode    
        if n == 0 :
            return 1
        if n == 1 :
            return x
        
        while m > 0 :
            
            if m % 2 == 1:
                ans *= multiple 
            
            multiple = multiple * multiple
            m = m // 2
            
        # check negative value
        if n > 0 :
            return ans 
        else :
            return 1 / ans 428. Pow(x, n)
计算复杂度O(logn)
需要注意的是这个题的corner case很多，需要把问题想清楚。
class Solution:
    """
    @param x: the base number
    @param n: the power number
    @return: the result
    """
    def myPow(self, x, n):
        # write your code here
        ans, multiple, m = 1, x, abs(n)
        
        # corner case
        if x == 0 :
            return 0
        # fast mode    
        if n == 0 :
            return 1
        if n == 1 :
            return x
        
        while m > 0 :
            
            if m % 2 == 1:
                ans *= multiple 
            
            multiple = multiple * multiple
            m = m // 2
            
        # check negative value
        if n > 0 :
            return ans 
        else :
            return 1 / ans 428. Pow(x, n)
计算复杂度O(logn)
需要注意的是这个题的corner case很多，需要把问题想清楚。
class Solution:
    """
    @param x: the base number
    @param n: the power number
    @return: the result
    """
    def myPow(self, x, n):
        # write your code here
        ans, multiple, m = 1, x, abs(n)
        
        # corner case
        if x == 0 :
            return 0
        # fast mode    
        if n == 0 :
            return 1
        if n == 1 :
            return x
        
        while m > 0 :
            
            if m % 2 == 1:
                ans *= multiple 
            
            multiple = multiple * multiple
            m = m // 2
            
        # check negative value
        if n > 0 :
            return ans 
        else :
            return 1 / ans 428. Pow(x, n)
计算复杂度O(logn)
需要注意的是这个题的corner case很多，需要把问题想清楚。
class Solution:
    """
    @param x: the base number
    @param n: the power number
    @return: the result
    """
    def myPow(self, x, n):
        # write your code here
        ans, multiple, m = 1, x, abs(n)
        
        # corner case
        if x == 0 :
            return 0
        # fast mode    
        if n == 0 :
            return 1
        if n == 1 :
            return x
        
        while m > 0 :
            
            if m % 2 == 1:
                ans *= multiple 
            
            multiple = multiple * multiple
            m = m // 2
            
        # check negative value
        if n > 0 :
            return ans 
        else :
            return 1 / ans 
```

