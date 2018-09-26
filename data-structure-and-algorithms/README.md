# Data Structure & Algorithms

## 0. 核心思想

这里我的主要目的是强化算法应对面试，因而所有的题解和思路都是基于面试和套路的，主要是快速熟悉算法和解决问题，非常面向工程，而非系统地学习算法，具有非常强的功利性，主要是因为时间紧迫。

**方法论**：一套模板为基础来解决所有相似的问题，并通过不断地重复以实现不断对自己的模板进行深入的理解，从而形成自己的风格并不断提高。

#### 独孤**九剑**

* **1.总决式**：最容易出卖你的，就是你的Coding Style工程师的代码长什么比脸长什么样重要
* **2.破剑式**：比O\(n\)更优的时间复杂度几乎只能是O\(logn\)的二分法
* **3.破掌式**：对于求两个变量如何组合的问题，可以循环其中一个变量，然后研究另一个变量如何变化
* **4.破枪式**：能够用BFS解决的问题，一定不要用DFS去做，因为recursion可能stack-overflow
* **5.破枪式**：碰到二叉树的问题，就想想整棵树在该问题上的结果和左右儿子在该问题上的结果直接的联系
* **6.破鞭式**：碰到让你找所有方案的题，基本可以确定是DFS，除了二叉树以外90%DFS的题，那么是排列要么是组合
* **7. 破箭式**： 哈希表的任何时间复杂度从严格意义上都是O\(Keysize\)而不是O\(1\)

## 1. 栈

### 什么是栈（Stack）？

栈（stack）是一种采用**后进先出**（LIFO，last in first out）策略的抽象数据结构。比如物流装车，后装的货物先卸，先转的货物后卸。栈在数据结构中的地位很重要，在算法中的应用也很多，比如用于非递归的遍历二叉树，计算逆波兰表达式，等等。

栈一般用一个存储结构（常用数组，偶见链表），存储元素。并用一个指针记录**栈顶位置**。**栈底位置**则是指栈中元素数量为0时的栈顶位置，也即栈开始的位置。  
栈的主要操作：

* `push()`，将新的元素压入栈顶，同时栈顶上升。
* `pop()`，将新的元素弹出栈顶，同时栈顶下降。
* `empty()`，栈是否为空。
* `peek()`，返回栈顶元素。

Python，直接使用`list`，查看栈顶用`[-1]`这样的切片操作，弹出栈顶时用`list.pop()`，压栈时用`list.append()`

### 栈的实现

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

### 栈在计算机内存当中的应用

我们在程序运行时，常说的内存中的堆栈，其实就是栈空间。这一段空间存放着程序运行时，产生的各种临时变量、函数调用，一旦这些内容失去其作用域，就会被自动销毁。

函数调用其实是栈的很好的例子，后调用的函数先结束，所以为了调用函数，所需要的内存结构，栈是再合适不过了。在内存当中，**栈从高地址不断向低地址扩展**，随着程序运行的层层深入，栈顶指针不断指向内存中更低的地址。

## 2. 队列

### 什么是队列（Queue）？

队列（queue）是一种采用**先进先出**（FIFO，first in first out）策略的抽象数据结构。比如生活中排队，总是按照先来的先服务，后来的后服务。队列在数据结构中举足轻重，其在算法中应用广泛，**最常用的就是在宽度优先搜索\(BFS）中，记录待扩展的节点**。

队列内部存储元素的方式，一般有两种，**数组**（array）和**链表**（linked list）。两者的最主要区别是：

* 数组对**随机访问**有较好性能。
* 链表对**插入**和**删除**元素有较好性能。

Python中，使用`collections.deque`，双端队列。

队列的主要操作有：

* `add()`队尾追加元素
* `poll()`弹出队首元素
* `size()`返回队列长度
* `empty()`判断队列为空

### 队列的实现

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

## 3. 哈希表

哈希表（Hash Table，也叫散列表），是根据关键码值 \(Key-Value\) 而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。哈希表的实现主要需要解决两个问题，哈希函数和冲突解决。

### 哈希函数 {#哈希函数}

哈希函数也叫散列函数，它对不同的输出值得到一个固定长度的消息摘要。理想的哈希函数对于不同的输入应该产生不同的结构，同时散列结果应当具有同一性（输出值尽量均匀）和雪崩效应（微小的输入值变化使得输出值发生巨大的变化）。  


## Heap

## 数组

## 链表

## 



