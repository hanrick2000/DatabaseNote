# Standard SQL Function



## 1. Introduction

数据库版本 ： PostgreSQL

这里的题因为比较细致，所以有的题非常的简单，还是戒骄戒躁，认认真真来做一遍吧。

## 2. Text

#### Concatenate

* Progresql : 用 \|\| 来连接
* SQL server : 用 + 来连接
* MySQL : 用concat\(\)函数

#### Functions

* length : length\(\)
  * SQL server : len\(\)
* lower : lower\(\)
* upper : upper\(\)
* 首字母大写 ： initcap（）
  * 只有Progresql和oracle有

#### String modify

* substring\(start, how many chars\)： 主要用来取子串
  * 在oracle中时substr\(\)
* replace\(text, match, replace\) : 主要用来替换
  * 在text中找到match的，替换为replace

#### 小结

* `||` is the concatenation operator; it merges multiple texts into one \(in **SQL Server** use `+`, in **MySQL** use `concat()`\),
* `length(x)` returns the **length** of text **x** \(in **SQL Server** use `len()`\),
* `lower(x)`, `upper(x)`, `initcap(x)` will all write **x** in the appropriate case,
* `substring(x, y, z)` will return the part of **x** starting from position **y** and with **z** characters in length \(in **Oracle** use `substr()`\),
* `replace(x,y,z)` will search **x**for **y**, and if it finds any **y**, it will replace if with **z**.

## 3. Conversion

#### 字符类型转换

主要语法 ： cast\(col as decimal\) 

需要注意的主要数字类别

* **integer** data types. There are types with names like `INTEGER`, `INT`, `SMALLINT`, `BIGINT`, etc.
* **exact numeric** data types \(=decimal types\). These are types with names like `NUMERIC` and `DECIMAL`.
* **inexact numeric** data types, with names like `FLOAT`, `REAL`, 

#### mod

* mod\(x, y\) = x / y 余下的 
* SQL server使用的是 % ：9%7 = 2
  * 除了oracle所有的数据库都支持%

#### round

* round\(x, precision\) 
* `ceil(x)` \(in **SQL Server** - `ceiling()`\) 上界
* `floor(x)` 下界
* `trunc(x)` \(in **MySQL** - `truncate()`\) 总是朝着0估计
  * 大于0的时候 **13.7 -&gt;** **13**
  * 小于0的时候 **-13.7** -&gt; **-13**

#### 其他：

* sqrt 和 abs 

#### 小结：

* Computations in floating point numbers are not always exact. Use **decimal numbers for all money columns** and when precision matters.
* Dividing two integers is a integer division. Use `CAST(column AS TYPE)` to avoid it.
* Avoid division by zero. 
* `mod(x,y)` or `x%y` - it returns the reminder of division of `x` by `y`
* `round (x)` or `round(x,p)` - rounds up the number to the integer or to specified precision \(`p`\)
* `ceil(x)` - rounds up to a nearest integer value
* `floor(x)` - rounds down to a nearest integer value
* `trunc(x)` or `trunc(x,p)` - always round towards zero to interger value or to specified precision \(`p`\)
* `abs(x)` - returns a absolute value of `x`
* `sqrt(x)` - returns a square root of `x`

## 4. Date, Time, Timestamp

#### 主要区别

* **Date** : yyyy-mm-dd  - 2018-06-13
* **Time**: hh-mm-ss - 07:55:00
* **Timestamp** :  yyyy-mm-dd hh-mm-ss utc 2014-06-10 07:55:00+02
  * 主要是含有时区，utc

#### Extract

主要语法 ： EXTRACT\(field FROM column\). 

* Filed 可以是 DAY, MONTH, YEAR, HOUR, MINUTE, SECOND.
* **SQL server** 使用的是datepart

#### 时区转换

主要语法 ： departure AT TIME ZONE 'Europe/Warsaw'

* at time zone 后面要跟着 大洲/城市
* MySQL 使用的是 conver\_tz

#### Interval

主要语法： date + interval ' ' + 

* MySQL不支持 date to date类符合操作
* year.month : INTERVAL 'x-y' YEAR TO MONTH 
  * 后面的year to month可以省略，只有这个可以省略
* day.second : INTERVAL 'd hh:mm:ss' DAY TO SECOND

如果单纯只加一种类型的时间type，直接使用inteval也是可以的：

* `INTERVAL '2' HOUR`
* `INTERVAL '3' DAY`
* `INTERVAL '5' MONTH`
* `INTERVAL '1' YEAR`

#### 系统内现在的的时间

* current\_date ：现在的date
* current\_time  : 现在的time
* current\_timestamp 现在的date，time，还有timezone

#### 小节

* SQL 特征 **dates** \(`'2010-01-01'`\), **time** \(`'13:00:00'`\) and **timestamps** \(`'2010-01-01 13:00:00'`\).
* 可以使用 `BETWEEN` 和 `ORDER BY`来比较
*  `EXTRACT(DAY FROM column)` 主要用来提取时间的特定部分
*  `AT TIME ZONE` 时区转换
* `INTERVAL '7' DAY` 可以支持加减操作
* `INTERVAL '2-1' YEAR TO MONTH` 混合间隔，2年1个月
* `CURRENT_DATE`, `CURRENT_TIME` or `CURRENT_TIMESTAMP`

## 5. Null

null是非常特殊的一种数据类型，它在数据库中有自己的定义，所以在比较的时候会出现问题。

* Null 的比较需要使用is null 或 is not null，这是由于它本身数据定义的特质。
  * 这里有个比较好的用法， in \('a', 'b', null\)。
* null 在一般的算术运算和文本计算中是失效的

#### Coalesce

* COALESCE\(x,y\) 会返回x 与 y中不是Null的值
* 函数支持嵌套

#### Nullif

* 如果检测到0，一般给出null

#### 小结

* Rule number one: never trust a `NULL`. Keep your eyes open.
* Use the operator `IS NULL` to check if the column is `NULL`.
* Use the operator `IS NOT NULL`to check if the column is `not NULL`.
* The equality and non-equality conditions \(`price = 7`, `price = NULL`, `price > 15`\) are NEVER true when the argument is `NULL`.
* Arithmetical operations \(e.g. `a + b`\) and most functions \(e.g. `a || b`, `length(a)`\) will return a `NULL` if any of the values is a `NULL`.
* `COALESCE(x,y,…)` returns the first **non-NULL** argument.
* `NULLIF(x,y)` returns `x` when `x != y` or `NULL` when `x = y`.
* In some databases, you can use `NULLS FIRST` or `NULLS LAST`to specify how `NULL`s are treated in the `ORDER BY` clause.

## 6. Aggregation

#### count\(\) 

* 要注意的是可能会考count\(distinct \)
* count\(\*\)和count\(col\)的区别，后者有0

#### avg\(\)

* avg在统计的时候并不统计null，这也是聚合函数的特性
* avg\(coalesce\(col, 0\)\)来进行补充

#### sum, max, min

#### 小结

* `COUNT(*)` counts the number of all rows.
* `COUNT(column1)` counts the number of rows where `column1`is not `NULL`.
* `COUNT` can be used to count non-`NULL` expressions.
* Avoid `COUNT(*)` with `LEFT JOIN`s.
* `DISTINCT` only takes into account distinctive values.
* `AVG`, `SUM`, `MAX` and `MIN` ignore `NULL`s, but `COUNT` takes them into account when counting the number of rows.
* You can use `COALESCE(x,y)` to replace **x** with **y** when **x** is `NULL`. For example, you can replace `NULL`s with **0**.
* You can use `AVG`, `SUM`, `MIN` or `MAX` with `DISTINCT`.
* You can use `ROUND` to round the score obtained with `AVG`.

## 7. Case When

case when还是有很多花式的操作的，之前一直没有注意到，现在想想这些操作在生产reporting的时候还是非常有用的。

* 最简单的例子是同一个col的不同值

  ```text
  CASE column_name
    WHEN value1 THEN text1
    WHEN value2 THEN text2
    …
    ELSE text_else
  END
  ```

* 但是一旦需要条件的时候就需要从case 后面写入when

  ```text
  CASE
    WHEN condition1 THEN text1
    WHEN condition2 THEN text2
    …
    ELSE text_else
  END
  ```

* The `ELSE` part is optional.
* Remember about the `END` clause at the end.
* You can use `CASE WHEN` with `SUM` to count multiple values in a single query:

  ```text
  SUM(CASE WHEN x THEN 1 ELSE 0 END)
  ```

* Similarly, you can use `CASE WHEN` with `COUNT` to count multiple values in a single query

  ```text
  COUNT(CASE WHEN x THEN column END)
  ```

* 还可以在group by 里面写case when
* 小细节是要注意case的判断是一条一条来的，而且null的归属非常的重要。

