# Data Structure & Algorithms \(3\) - Two Pointers

双指针算法主要是在数组一端存在两个指针，同时向一边移动，用这样的方式来解决例如Two Sum的问题。

## 同向双指针

#### 521. 数组去重问题 Remove duplicates in an array

第一种做法使用hash表进行记录，就是遍历一次如果不在hash table就扔掉

* 时间复杂度 O\(n\)   
* 空间复杂度 O\(n\)

```python
class Solution:
    """
    @param nums: an array of integers
    @return: the number of unique integers
    """
    def deduplication(self, nums):
        # Write your code here
        hashList, result = {}, 0
        
        for val in nums :
            if val not in hashList :
                nums[result] = val
                hashList[val] = 1
                result += 1
                
        return result
```

第二种做法，双指针同向，这里有两个指针一个slow\_pointer，另一个quick\_pointer，slow\_pointer一直从0开始，而quick\_pointer遍历整个数组，如果遇到不一样的，就写入slow\_pointer，然后slow\_pointer右移一格。

```python
class Solution:
    """
    @param nums: an array of integers
    @return: the number of unique integers
    """
    def deduplication(self, nums):
        # Write your code here
        n = len(nums) 
        
        if n == 0 :
            return 0
            
        nums.sort()
        slow_pointer = 1

        for quick_pointer in range(1, n) :
            
            if nums[quick_pointer - 1] != nums[quick_pointer] :
                nums[slow_pointer] = nums[quick_pointer]
                slow_pointer += 1
                
        return slow_pointer
```

#### 603. 滑动窗口问题 Window Sum

左右指针分别保持一定的距离，每次一进一出进行加减就可以了，不要使用\[:\]进行切片，如果滑动窗口非常大，计算复杂度会非常非常高

```python
class Solution:
    """
    @param nums: a list of integers.
    @param k: length of window.
    @return: the sum of the element inside the window at each moving.
    """
    def winSum(self, nums, k):
        # write your code here
        n = len(nums)
        
        if n == 0 or n < k :
            return []
            
        k_sum = sum(nums[0:k])
        result = [k_sum]
        
        for right in range(k, n) :
            left = right - k
            k_sum += nums[right] - nums[left]
            result.append(k_sum)
            
        return result
```

#### 610. 两数之差问题 Two Difference

双指针遍历即可

```python
class Solution:
    """
    @param nums: an array of Integer
    @param target: an integer
    @return: [index1 + 1, index2 + 1] (index1 < index2)
    """
    def twoSum7(self, nums, target):
        # write your code here
        if len(nums) == 0 :
            return []
            
        for left in range(0, len(nums)) :
            
            for right in range(left + 1, len(nums)) :
                
                if nums[left] - nums[right] == target :
                    return [left + 1, right + 1]
                
                if nums[right] - nums[left] == target :
                    return [left + 1, right + 1]
                    
        return []
```

#### 228. 链表中点问题 Middle of Linked List

快慢指针，主要是通过控制快指针走两步，而慢指针只走一步的方法，以此实现了取中点的方法。

```python
"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    # @param head: the head of linked list.
    # @return: a middle node of the linked list
    def middleNode(self, head):
        # Write your code here
        if head == None :
            return None
        
        fast, slow = head, head
        
        while fast and fast.next and fast.next.next :
            slow = slow.next
            fast = fast.next.next
        
        return slow
```

#### 102. 带环链表问题 Linked List Cycle

主要是用快慢指针判断是否存在环，如果要判断入口，只需要将快指针重置到起点，并确定步长为1即可。

```python
"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""
class Solution:
    """
    @param head: The first node of linked list.
    @return: True if it has a cycle, or false
    """
    def hasCycle(self, head):
        
        if not head or not head.next:
            return False

        slow = head
        fast = head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow is fast:
                return True
    
        return False
```

