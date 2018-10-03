# Data Structure & Algorithms \(8\) - Data Structure

## 1. 数据结构设计类问题

比如你需要设计一个 Set 的数据结构，提供 lowerBound 和 add 两个方法。

* lowerBound 的意思是，找到比某个数小的最大值 

具体比较

* 算法1:  使用数组存储，每次打擂台进行比较，插入就直接插入到数组最后面
  * O\(n\) lowerBound 
  * O\(1\) add
* 算法2: 使用红黑树\(Red-black Tree\)存储，Java 里的 TreeSet，C++ 里的 map 
  * O\(logn\) lowerBound 
  * O\(logn\) add

上面两个算法谁好谁坏呢?

**不一定谁好谁坏 !   要看这两个方法被调用的频率如何。**

* 如果 lowerBound 很少调用, add 非常频繁, 则算法1好。
* 如果 lowerBound 和 add 调用的频率都差不多，或者 lowerBound 被调用得更多，则算法2好

不过通常来说，在面试中的问题，我们会很容易找到一个像算法1这样的实现方法，其中一个操作时间 复杂度很大，另外一个操作时间复杂度很低。通常的解决办法都是想办法增大较快操作的时间复杂度来加速较慢操作的时间复杂度。

#### 数据结构的主要考法

数据结构可以认为是一个数据存储集合以及定义在这个集合上的若干操作\(功能），它有如下的三种考法: 

* 考法1：问某种数据结构的基本原理，并要求实现
  * 例题:说一下 Hash 的原理并实现一个 Hash 表 
* 考法2：使用某种数据结构完成事情
  * 例题:归并 K 个有序数组 
* 考法3：实现一种数据结构，提供一些特定的功能，通常需要一个或者多个数据结构配合在一起使用
  * 例题:最高频 K 项问题

#### 数据结构时间复杂度的衡量方法

数据结构通常会提供“多个”对外接口，只用一个时间复杂度是很难对其进行正确评价的，所以通常要对每个接口的时间复杂度进行描述

## 2. 队列 Queue

队列（queue）是一种采用**先进先出**（FIFO，first in first out）策略的抽象数据结构。比如生活中排队，总是按照先来的先服务，后来的后服务。队列在数据结构中举足轻重，其在算法中应用广泛，**最常用的就是在宽度优先搜索\(BFS）中，记录待扩展的节点**。

队列内部存储元素的方式，一般有两种，**数组**（array）和**链表**（linked list）。两者的最主要区别是：

* 数组对**随机访问**有较好性能。
* 链表对**插入**和**删除**元素有较好性能。

Python中，使用`collections.deque`，双端队列。

队列的主要操作有：

* `add()`队尾追加元素 - O\(1\)
* `poll()`弹出队首元素 - O\(1\)
* `size()`返回队列长度 - O\(1\)
* `empty()`判断队列为空 - O\(1\)

#### 队列的实现

```python
class MyQueue:

    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    def adjust(self):
        if len(self.stack2) == 0:
            while len(self.stack1) != 0:
                self.stack2.append(self.stack1.pop())
                
    def push(self, element):
        self.stack1.append(element)

    def top(self):
        self.adjust()
        return self.stack2[len(self.stack2) - 1]

    def pop(self):
        self.adjust()
        return self.stack2.pop()
```

#### 642. Moving Average from Data Stream

一开始是每次都将queue里面原始加起来然后再去计算，主要的启示是:

* 如果使用了特定的数据结构，尽量使用它的特性，而不引入其他的操作

```python
class MovingAverage:
    """
    @param: size: An integer
    """
    def __init__(self, size):
        # do intialization if necessary
        self.queue = collections.deque()
        self.size = size
        self.sum = 0
    """
    @param: val: An integer
    @return:  
    """
    def next(self, val):
    
        if len(self.queue) == self.size :
            head = self.queue.popleft()
            self.sum -= head
            
        self.sum += val
        self.queue.append(val)    
        
        return self.sum / len(self.queue)
```

## 3. 栈 Stack 

栈（stack）是一种采用**后进先出**（LIFO，last in first out）策略的抽象数据结构。比如物流装车，后装的货物先卸，先转的货物后卸。栈在数据结构中的地位很重要，在算法中的应用也很多，比如用于非递归的遍历二叉树，计算逆波兰表达式，最常用的是**非递归实现DFS的主要数据结构**。

栈一般用一个存储结构（常用数组，偶见链表），存储元素。并用一个指针记录**栈顶位置**。**栈底位置**则是指栈中元素数量为0时的栈顶位置，也即栈开始的位置。栈的主要操作：

* `push()`，将新的元素压入栈顶，同时栈顶上升。
* `pop()`，将新的元素弹出栈顶，同时栈顶下降。
* `empty()`，栈是否为空。
* `peek()`，返回栈顶元素。

Python，直接使用`list`，查看栈顶用`[-1]`这样的切片操作，弹出栈顶时用`list.pop()`，压栈时用`list.append()`

#### 栈的实现

```python
class Stack:
    # initialize your data structure here.
    def __init__(self):
        self.queue = []

    # @param x, an integer, push a new item into the stack
    # @return nothing
    def push(self, x):
        # Write your code here
        self.queue.append(x)

    # @return nothing, pop the top of the stack
    def pop(self):
        # Write your code here
        for x in range(len(self.queue) - 1):
            self.queue.append(self.queue.pop(0))
        self.queue.pop(0)

    # @return an integer, return the top of the stack
    def top(self):
        # Write your code here
        top = None
        for x in range(len(self.queue)):
            top = self.queue.pop(0)
            self.queue.append(top)
        return top

    # @return an boolean, check the stack is empty or not.
    def isEmpty(self):
        # Write your code here
        return self.queue == []
```

#### 栈在计算机内存当中的应用

我们在程序运行时，常说的内存中的堆栈，其实就是栈空间。这一段空间存放着程序运行时，产生的各种临时变量、函数调用，一旦这些内容失去其作用域，就会被自动销毁。

函数调用其实是栈的很好的例子，后调用的函数先结束，所以为了调用函数，所需要的内存结构，栈是再合适不过了。在内存当中，**栈从高地址不断向低地址扩展**，随着程序运行的层层深入，栈顶指针不断指向内存中更低的地址。

{% embed data="{\"url\":\"https://www.cnblogs.com/kevinGaoblog/archive/2012/03/23/2413102.html\",\"type\":\"link\",\"title\":\"栈空间和堆空间 - kevinGao - 博客园\",\"description\":\"一个由C/C++编译的程序占用的内存分为以下几个部分：1、栈区（stack）：又编译器自动分配释放，存放函数的参数值，局部变量的值等，其操作方式类似于数据结构的栈。2、堆区（heap）：一般是由程序员\",\"icon\":{\"type\":\"icon\",\"url\":\"https://www.cnblogs.com/favicon.ico\",\"aspectRatio\":0}}" %}

## 4. 哈希表 Hash

哈希表（Hash Table，也叫散列表），是根据关键码值 \(Key-Value\) 而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。哈希表的实现主要需要解决两个问题，哈希函数和冲突解决。

#### 哈希函数

哈希函数也叫散列函数，它对不同的输出值得到一个固定长度的消息摘要。理想的哈希函数对于不同的输入应该产生不同的结构，同时散列结果应当具有同一性（输出值尽量均匀）和雪崩效应（微小的输入值变化使得输出值发生巨大的变化）。

* O\(1\) Insert 
* O\(1\) Find 
* O\(1\) Delete

#### 冲突的解决方式

冲突（Collision），是说两个不同的 key 经过哈希函数的计算后，得到了两个相同的值。解决冲突的方法，主要有两种：

* 开散列法（Open Hashing）。是指哈希表所基于的数组中，每个位置是一个 Linked List 的头结点。这样冲突的 &lt;key, value&gt; 二元组，就都放在同一个链表中。
* 闭散列法（Closed Hashing）。是指在发生冲突的时候，后来的元素，往下一个位置去找空位。
* 重哈希（Refresh Hashing）。哈希表容量的大小在一开始是不确定的。如果哈希表存储的元素太多（如超过容量的十分之一），我们应该将哈希表容量扩大一倍，并将所有的哈希值重新安排。

{% embed data="{\"url\":\"https://www.jianshu.com/p/bdf6109ecb18\",\"type\":\"link\",\"title\":\"129. 重哈希\",\"description\":\"描述 哈希表容量的大小在一开始是不确定的。如果哈希表存储的元素太多（如超过容量的十分之一），我们应该将哈希表容量扩大一倍，并将所有的哈希值重新安排。假设你有如下一哈希表：size=3, capa...\",\"icon\":{\"type\":\"icon\",\"url\":\"https://cdn2.jianshu.io/assets/apple-touch-icons/152-bf209460fc1c17bfd3e2b84c8e758bc11ca3e570fd411c3bbd84149b97453b99.png\",\"width\":152,\"height\":152,\"aspectRatio\":1}}" %}

## 5. Heap

堆（heap）也被称为优先队列（priority queue）。队列中允许的操作是先进先出（FIFO），在队尾插入元素，在队头取出元素。而堆也是一样，在堆底插入元素，在堆顶取出元素，但是堆中元素的排列不是按照到来的先后顺序，而是按照一定的优先顺序排列的。这个优先顺序可以是元素的大小或者其他规则。如图一所示就是一个堆，堆优先顺序就是大的元素排在前面，小的元素排在后面，这样得到的堆称为**最大堆**。

{% embed data="{\"url\":\"https://blog.csdn.net/juanqinyang/article/details/51418629\",\"type\":\"link\",\"title\":\"数据结构-堆（heap） - 蜗牛君的奋斗史 - CSDN博客\",\"description\":\"堆（heap）也被称为优先队列（priority queue）。队列中允许的操作是先进先出（FIFO），在队尾插入元素，在队头取出元素。而堆也是一样，在堆底插入元素，在堆顶取出元素，但是堆中元素的排列不是按照到来的先后顺序，而是按照一定的优先顺序排列的。这个优先顺序可以是元素的大小或者其他规则。\",\"icon\":{\"type\":\"icon\",\"url\":\"https://csdnimg.cn/public/favicon.ico\",\"aspectRatio\":0}}" %}

#### 操作的时间复杂度

* O\(log N\) Add 
* O\(log N\) Remove 
* O\(1\) Min or Max 

既然堆有这种最大先出的性质，那么利用堆，就可以构建优先队列。大家知道普通的队列是“先进先出”的原则，而优先队列则是“最大先出”的原则，也就是说，队列中的每一个元素都有一个权重，权重大的最先出队。这种思路最常见的应用是医院排队，一种原则是让病情更严重的患者最先就诊，那如果按照这个原则设计一个就医系统的话，优先队列就是最恰当的选择。

{% embed data="{\"url\":\"https://blog.csdn.net/guoziqing506/article/details/52372469\",\"type\":\"link\",\"title\":\"堆的实现与优先队列 - guoziqing506的博客 - CSDN博客\",\"description\":\"堆（Heap）这种数据结构对我们来说是极为有用的。因为它可以非常方便地帮我们完成高效的动态的排序。接下来，我将给出堆的原理以及实现堆的具体操作。   先看一下堆的结构，在此，只需要牢记两点： 1. 堆是一棵完全二叉树 2. 堆中每个节点都大于等于其任何子节点   所谓完全二叉树，一定要满足这个特征：这棵树除最后一层外，每一层都是满的，且最后一层如果不满，所有节点都位于最左边。看下图\",\"icon\":{\"type\":\"icon\",\"url\":\"https://csdnimg.cn/public/favicon.ico\",\"aspectRatio\":0}}" %}

#### Heap的常见操作

值得注意的是，python的heapq是最小堆

* `heapq.heappush`\(_heap_, _item_\) 
  * Push the value _item_ onto the _heap_, maintaining the heap invariant.
* `heapq.heappop`\(_heap_\)
  * Pop and return the smallest item from the _heap_, maintaining the heap invariant. If the heap is empty, [`IndexError`](https://docs.python.org/3.6/library/exceptions.html#IndexError) is raised. To access the smallest item without popping it, use `heap[0]`.
* `heapq.heappushpop`\(_heap_, _item_\) 
  * Push _item_ on the heap, then pop and return the smallest item from the _heap_. The combined action runs more efficiently than [`heappush()`](https://docs.python.org/3.6/library/heapq.html#heapq.heappush) followed by a separate call to [`heappop()`](https://docs.python.org/3.6/library/heapq.html#heapq.heappop).
* `heapq.heapify`\(_x_\)
  * Transform list _x_ into a heap, in-place, in linear time.

#### 4. Ugly Number II

这个题很简单，不细说了

```python
import heapq

class Solution:
    """
    @param n: An integer
    @return: the nth prime number as description.
    """
    def nthUglyNumber(self, n):
        # 初始化
        heap = [1]
        visited = set([1])
        
        val = None
        # 找到第n个
        for i in range(n) :
            # pop 最小的出来
            val = heapq.heappop(heap)
            # 序列化一个堆
            for mul in [2, 3, 5] :
                if val * mul not in visited :
                    visited.add(val * mul)
                    heapq.heappush(heap, val * mul)
                    
        return val
```

## 数组

## 链表

