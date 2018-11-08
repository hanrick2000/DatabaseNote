# Window Functions

## 1. Over\(\)

#### 什么是window function ?

window function对表中的数据进行运算，而当前的运算操作和其他行是相关的，见下动图

![animated\_explanation](https://academy.vertabelo.com/static/window-functions-part2-ex4.gif)

这个东西被叫做window function是因为行在一个滑动窗口之中，这个窗口就是**window** or a **window frame。**

```sql
<window_function> OVER (...)
```

`<window_function>` 可以是 **aggregate function**  \(`COUNT()`, `SUM()`, `AVG()`etc.\), 或者 **ranking** or **analytical function** 

*  window frame 在 `OVER(...)`  中定义

#### Over 概念

简单的over概念，`aggregate function + OVER()`

```sql
SELECT title, editor_rating, AVG(editor_rating) OVER()
FROM movie;
```

over的本质就是group by，当什么都不放入的时候就是全部的集合，当需要partition的时候，就加入partitiion by即可。

* partition by = group by 这里省去了子查找，后面会细细说

```sql
SELECT
  first_name,
  last_name,
  country,
  COUNT(id) OVER(PARTITION BY country)
FROM customer;
```

#### 小结

* Use `<window_function> OVER()` to compute an aggregate for all rows in the query result.
* The window functions is applied **after** the rows are filtered by `WHERE`.
* The window functions are used to compute aggregates but keep details of individual rows at the same time.
* You **can't use** window functions in `WHERE` clauses.

## 2. Partition By

这一部分主要讲，可以在over\(\)进行分组，主要的syntax是 `PARTITION BY`

```sql
<window_function> OVER (PARTITION BY column1, column2
... column_n)
```

`PARTITION BY` 同`GROUP BY` 基本一样 ：就是按照列进行分组。

*  不同于 `GROUP BY`, `PARTITION BY`  并不好实际上合并行，更像是扫描

![Part3\_diagram](https://academy.vertabelo.com/static/window-functions-part3-ex5.png)

#### 小结

* `OVER(PARTITION BY x)`works in a similar way to `GROUP BY`, defining the window as all the rows in the query result that have the same value in **x**.
* **x** can be a single column or multiple columns separated by commas.

## 3. Ranking

排序的基本语法如下：

```sql
<ranking function> OVER (ORDER BY <order by columns>)
```

因为需要排序，所以在over中需要使用order by来进行排序

#### 主要函数的区别（从前面的搬运过来）：

* **Row\_number\(\):** 类似一个统计函数，就是计数函数，比如从1到n，如果不用partition by 就是行的index。
* **RANK\(\) ：**主要是给出具体的排序，但是rank不一样在于，它允许一种并列的情况，而且会自动允许存在gap
*  **DENSE\_RANK\(\):** 和rank很像，只是它会取消之间的gap
  * 有并列 :  Rank & Dense\_Rank 
    * 有gap :  Rank
    * 无gap ：Dense\_Rank 
  * 无并列 :  Row\_number 

![https://blog.csdn.net/Wikey\_Zhang/article/details/72765464](../../.gitbook/assets/screen-shot-2018-08-23-at-7.30.12-pm.png)

#### Ntile

这个函数我一直用的比较少，大致是将数据分成n个部分。

```sql
SELECT name, genre, editor_rating,
  NTILE(3) OVER (ORDER BY editor_rating DESC)
FROM game;
```

* 在分组的过程之中是可能存在不能均分的情况的

![\[Part4\_graphic\]](https://academy.vertabelo.com/static/window-functions-part4-ex15.png)

#### CTE函数的应用

因为使用了Window Function，所以不能进行filter，必须用cte作为缓存表来存储所有的数据，这里cte可以理解为subquery。

```sql
WITH ranking AS
  (SELECT title,
    RANK() OVER(ORDER BY editor_rating DESC) AS rank
  FROM movie)

SELECT title
FROM ranking
WHERE rank = 2;
```

## 4. Window Frame

**Window frames** 

主要定义了哪些行应当纳入到计算范围之内

* 可以从一开始算到现在的行
* 或者 现在行的前一行和后一行

![](https://academy.vertabelo.com/static/window-functions-part5-ex5.png)

#### 窗口的大小主要的语法有Rows和Range两种

```sql
<window function>
OVER (...
  ORDER BY <order_column>
  [ROWS|RANGE] <window frame extent>)
```

* `UNBOUNDED PRECEDING` – 当前行之前所有可行的行
* `PRECEDING` – 当前行之前的n行
* `CURRENT ROW` – 当前行
* `FOLLOWING` – 当前行之后的n行
* `UNBOUNDED FOLLOWING` – 当前行之后所有可行的行

#### 一些可以优化的小tips :

主要是有的时候不用给出between的完整定义，也就是缩写，因为其实本身在内容之中已经包括了：

* `ROWS UNBOUNDED PRECEDING` 等同于`BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`
* `ROWS n PRECEDING` 等同于 `BETWEEN n PRECEDING AND CURRENT ROW`
* `ROWS CURRENT ROW` 等同于 `BETWEEN CURRENT ROW AND CURRENT ROW`

#### Range v.s. Rows

之前看了官方文档和其他技术博客，感觉从物理和逻辑层面去理解太过于底层了，就和要去理解B+ tree一样，感觉下面的例子更加简单。

其实Rows和Range的本质区别在于：

* Range会将order by的**同一行赋予一样的值**

```sql
# Rows
SELECT id, placed, total_price,
  SUM(total_price) OVER (ORDER BY placed ROWS UNBOUNDED PRECEDING)
FROM single_order;
```

![Diagram A](https://academy.vertabelo.com/static/window-functions-part5-ex13-graphic1.png)

上面对于同一天的逐渐累加的，而如果使用range，每一天其实应该是已经加好的，因为这一天对我们来说是一个整体，注意下图同一时间的结果。

```sql
# Range
SELECT id, placed, total_price,
  SUM(total_price) OVER(ORDER BY placed RANGE UNBOUNDED PRECEDING)
FROM single_order;
```

![Diagram B](https://academy.vertabelo.com/static/window-functions-part5-ex13-graphic2.png)

在ranking的时候，rows和range的区别类似于row\_number\(\)和rank\(\)的区别，详细见下图，第一个是row\_number + rows, 第二个是rank + range

![Diagram A](https://academy.vertabelo.com/static/window-functions-part5-ex14-graphic1.png)

![Diagram B](https://academy.vertabelo.com/static/window-functions-part5-ex14-graphic2.png)

#### 小结：

* You can define a window frame within `OVER(...)`. The syntax is: `[ROWS|RANGE] <window frame definition>`.
* `ROWS` always treats rows individually \(like the `ROW_NUMBER()` function\), `RANGE` also adds rows which share the same value in the column we order by \(like the `RANK()`function\).
* `<window frame definition>` is defined with `BETWEEN <lower bound> AND <upper bound>`, where the bounds may be defined with:
  * `UNBOUNDED PRECEDING`,
  * `n PRECEDING` \(`ROWS` only\),
  * `CURRENT ROW`,
  * `n FOLLOWING` \(`ROWS` only\),
  * `UNBOUNDED FOLLOWING`

## 11/07 current place

## 5. Analytics Function

这些函数都需要在window frame里面才可以发挥作用，基本可以分为两类。

* 对列移动的：
  * lead - 列往上移n格
  * lag - 列往下移n格
* 对列求值的:
  * first\_value : 列的第一个值
  * last\_value : 列的最后一个值

#### Over中含有聚合函数

如果在over中也含有聚合函数，是需要group by的，因为这里本质上需要group by 两次。

