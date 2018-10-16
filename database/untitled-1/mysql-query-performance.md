# MySQL Query Performance

有的时候面试也会问到SQL query的性能的问题，主要搬运自这里

{% embed url="https://community.modeanalytics.com/sql/tutorial/sql-performance-tuning/" %}

**主要因素：**

* **Table size:** 主要是table中的行数的问题，因为数据量本身巨大而引起的。
* **Joins:** 主要是join的话有可能扩大表的size，其次是join的运算都是乘法基本的，运算时间很高。
* **Aggregations:** 优先使用聚合函数对数据进行缩减，然后再执行join之类的会快很多。

**其他因素：**

* **Other users running queries:** 这个讲的是并发数量和数据库本身性能
* **Database software and optimization:** 这个是表本身结构没有优化没有做到第三范式最优。
* **CTE函数：**核心思想就是避免写很多重复的join，这样其实很耗时间，所以推荐的写法是讲很多重复的地方用一个tempary的cte表做过渡，这样整体运行速度会快很多。

