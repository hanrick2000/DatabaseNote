---
description: 这个是Hackrank的题解
---

# Hackrank SQL database \(1\)

又要面试SQL了，就又刷了一遍，这一次会更加注意整体的分析以及对于新兴技术的运用。因为hackrank以及有了非常好的分类，所以我就按照它的分类来依次解答。

数据库我还是使用的MySQL，因为它是开源的，我觉得hackrank比较好的是，很侧重一步一步地练习，所以table一般都是一样的。

这里我侧重的不仅仅是能不能写出了SQL，而是如果让别人更好地理解我的思路。

## Basic Select

#### Revising the Select Query I

这个题是一个基本的题，但是需要小心的是data type，如果莽撞地直接去写sql query可能不是很好，需要先检查一下表格的数据类型，以做到bug free。

```sql
select *
from city 
where population > 100000 and countrycode = 'USA';
```

#### Revising the Select Query II

这个题是前面一个题的延续，在我们已经了解了数据结构的情况下，我们选择特定的column。

```sql
select name
from city 
where population > 120000 and countrycode = 'USA';
```

#### Select All

这个题主要考查的是会不会用\*来选择所有行，需要注意的是这里不能使用1而必须使用\*，这个是MySQL版本的一个特性。

```sql
select *
from city;
```

#### Select by ID

这个题主要侧重data type，如果面试的时候要和其他candidate比较有优势，我觉得应该用一些小的细节impress面试官，这样可以反映出你认真在思考，并且很有条理地解决问题。

```sql
select *
from city
where id = 1661;
```

#### Japanese Cities' Attributes

我一开始觉得这种题都是小意思，但是其实细细想想，这里希望训练的是对data type的感觉。

```sql
select *
from city
where countrycode = 'JPN';
```

#### Japanese Cities' Names

这个就不说啦，这些简单题基本的重点已经了解了。

```sql
select name
from city
where countrycode = 'JPN';
```

#### Weather Observation Station 1

这里想到了另一点，就是如果只给了data type，但是没有给sample data怎么办，应该和面试官交流一下，看一下如何解决这个问题，不管能不能拿到sample data，这都是一个加分项。

```sql
select city, state
from station；
```

#### Weather Observation Station 3

这里首先我们需要的数据在station里面，那么我们先在脑海中建立一个table，然后我们用偶数这个条件去filter所有的data，剩下的数据中可能有duplicates，那么我们再只去unique的值就可以了。

```sql
select distinct city
from station 
where id % 2 =0 ;
```

#### Weather Observation Station 4

首先，我们需要得到总的cities，这样需要写第一个query，然后我们需要得到distinct的cities，这样我们有了数据之后，我们就去想的是下一步如何join这两个table，因为这个是两个数字可以直接相减，就直接可以解出来。

```sql
select count(*) - (select count(distinct city)
                   from station)
from station ;
```

得到这个结论之后，我们可以进行优化一下，因为我们观察到都是从一个表里面extract data，所以可以合并在一起。

```sql
select count(*) - count(distinct city)
from station ;
```

#### Weather Observation Station 5

首先，我们需要得到总的cities，这样需要写第一个query，然后我们需要得到distinct的cities，这样我们有了数据之后，我们就去想的是下一步如何join这两个table，因为这个是两个数字可以直接相减，就直接可以解出来。

```sql
select count(*) - (select count(distinct city)
                   from station)
from station ;
```

得到这个结论之后，我们可以进行优化一下，因为我们观察到都是从一个表里面extract data，所以可以合并在一起。

```sql
select count(*) - count(distinct city)
from station ;
```

#### Weather Observation Station 6

这里我能想到的一种比较清晰的解决方法就是分别求出最大和最小，如果求最大值的话，只需要降序排序求第一个就可以了，如果求最小值，只需要升序排序求第一个就可以，然后我们将两个union起来就行。

```sql
(select city,length(city) as len
 from station
 order by len, city
 limit 1)

 union 

(select city,length(city) as len
 from station
 order by len desc, city
 limit 1)
```

{% hint style="info" %}
这里有一个非常非常重要的问题，就是使用limit 1会丢失并列最大值的情况，因为这个是一个比较常用的方法，但是缺点是 **会丢失duplicates**，那么如果需要保留duplicates的话应该怎么办？

就是使用subquery。
{% endhint %}

Follow up 一下这个问题的解答：\(这个题中最小的有三个并列\)

```sql
select city,length(city) as len 
from station 
where length(city) in (select min(length(city)) from station)

union 

select city,length(city) as len 
from station 
where length(city) in (select max(length(city)) from station)
```

#### Weather Observation Station 6 {#weather-observation-station-5}

主要是考察了MySQL的正则表达式，这里语法有一些不一样，我感觉面试不会考到，但是还是应该复习一下。具体内容见下面的博文，我觉得总结的非常好。这里需要注意的是：

* Like : 是完全匹配
* Rlike : 是部分匹配
* Mysql不区分大小写

{% embed data="{\"url\":\"https://blog.csdn.net/qq\_34359327/article/details/78601426\",\"type\":\"link\",\"title\":\"【MySQL笔记】like、rlike、REGEXP关键词的使用 - CSDN博客\",\"description\":\"MySQL workbench中like、rlike和regexp常用的方法\",\"icon\":{\"type\":\"icon\",\"url\":\"https://csdnimg.cn/public/favicon.ico\",\"aspectRatio\":0}}" %}

```sql
select distinct city
from station
where city RLIKE '^[aeiou]';
```

#### Weather Observation Station 7

这个题还是正则表达式，多练习即可。

```sql
select distinct city
from station 
where city Rlike '[aeiou]$';
```

#### Weather Observation Station 8

这个题考的更加深入一些，包括贪婪匹配之类的，需要熟练掌握，.\*以及.\*?之类的用法，见下面的blog:

{% embed data="{\"url\":\"http://database.51cto.com/art/200811/98155\_all.htm\",\"type\":\"link\",\"title\":\"MySQL中的字符串模式匹配 - 51CTO.COM\",\"description\":\"本文介绍了有关字符串模式匹配的有关知识。标准的SQL模式匹配是SQL语言的标准，可以被其它关系数据库系统接受。扩展正规表达式模式匹配是根据Unix系统的标准开发了，一般只可使用在MySQL上，但是其功能要比标准的SQL模式匹配更强。\"}" %}

```sql
select distinct city
from station 
where city Rlike '[aeiou]$';
```

#### Weather Observation Station 9

这个题考的主要是非运算

```sql
select distinct city
from station 
where city rlike '^[^aeiou]';
```

#### Weather Observation Station 10

同上一题一样。

```sql
select distinct city
from station
where city rlike '[^aeiou]$';
```

#### Weather Observation Station 11

这里主要考察或运算。

```sql
select distinct city
from station 
where city rlike "^[^aieou]|.*[^aeiou]$";
```

#### Weather Observation Station 12

这个就不说啦，写了好几个正则表达式了，都差不多。

```sql
select distinct city
from station 
where city rlike "^[^aieou].*[^aeiou]$";
```

#### Higher Than 75 Marks

这里主要是要了解一下left和right对string的处理就可以，整体来讲，没有什么特别难的地方。

```sql
select name
from students
where marks > 75
order by right(name,3), id;
```

#### Employee Names

主要是还是熟悉一下order by。

```sql
select name
from Employee
order by name;
```

####  Employee Salaries {#employee-names}

```sql
select name
from Employee
where salary > 2000 and months < 10
order by employee_id ;
```

#### 小结 {#employee-names}

Basic Select这一部分基本就是过了一下基本的所有常规语法和用法。需要注意的地方如下：

* Data Type和Data Integrity永远是最先检查的
* 写SQL的基本逻辑非常重要，要可以将复杂问题简化为很多个小问题
* 正则表达式的理解和MySQL的语法很重要 

## Advanced Select

#### Type of Triangle

这个题是用case when就可以解决的一道简单题，我们需要的使用一些条件来进行排序。这个题的难点是，一般来说都会选择互斥的条件进行，但是这里不是，所以我们需要巧妙的用条件来一步步筛选。

case when的执行语句类似于Python的if语句，都是先执行前一个if，再执行后一个if语句。

这里需要先排除不是三角形的，然后排除等边三角形，然后是等腰三角形。

```sql
select case when A + B <= C OR A + C <= B OR B + C <= A then 'Not A Triangle'
            when A = B AND B = C then 'Equilateral'
            when A = B OR A = C OR B = C then 'Isosceles'
            else 'Scalene'
        end as TriangleType
from triangles
```

#### The PADS

这个题主要考察concat函数，主要需要小心的是，“和'在这之中的使用，这个非常的重要，如果需要连续concat，必须使用' '。

```sql
select concat(name,'(',left(occupation,1),')')
from occupations
order by name;

select concat('There are a total of ', count(occupation),' ',lower(occupation),'s.')
from occupations
group by occupation
order by count(occupation),occupation;
```

#### Occupations

这个题其实还是很难的，我个人对assignment不是非常的熟悉，这里的大致思路是先设四个变量分别记录四个职业的行数，这里如果dRow是1，代表是第一个出现的，然后如果row的数量是一样的那么就可以放在一行，有这样的一种思想在。

我们先统计出所有的行数的标记，然后建立四列，按照row分组就可以了。

```sql
SET @dRow = 0, @pRow = 0, @sRow = 0, @aRow = 0;

SELECT MIN(Doctor), MIN(Professor), MIN(Singer), MIN(Actor)
FROM (
    SELECT  CASE Occupation    
                WHEN 'Doctor'       THEN @dRow := @dRow + 1
                WHEN 'Professor'    THEN @pRow := @pRow + 1
                WHEN 'Singer'       THEN @sRow := @sRow + 1
                WHEN 'Actor'        THEN @aRow := @aRow + 1
            END AS row,
            IF (Occupation = 'Doctor', Name, NULL) AS Doctor,
            IF (Occupation = 'Professor', Name, NULL) AS Professor,
            IF (Occupation = 'Singer', Name, NULL) AS Singer,
            IF (Occupation = 'Actor', Name, NULL) AS Actor
    FROM    OCCUPATIONS
    ORDER BY Name
) a
GROUP BY row;
```

#### Binary Tree Nodes

之前做过类似的题，这里重新优化一下，首先如果节点的母节点是null，则必然是root，我们首先排除了root的情况，那么剩下在不是null的里面，如果它是其他节点的母节点，那么它必然是inner，所以再排除这种情况，剩下的就是leaf了。

```sql
select t1.n,
    case 
        when t1.p is null then 'Root'
        when t1.n in (select distinct p from bst) then 'Inner'
        else 'Leaf'
    end as nodeType    
from BST as t1
order by t1.n;
```

#### New Companies

这个题有一个bug就是可以直接用第一个和最后一个表进行join，这样直接就可以得到最好的结果了。

```sql
select C.company_code , C.founder, 
    count(distinct E.lead_manager_code),
    count(distinct E.senior_manager_code), 
    count(distinct E.manager_code),
    count(distinct E.employee_code)
from Company as C, Employee as E
where C.company_code= E.company_code
group by C.company_code,C.founder ；
```

**小结**

Advanced Select这一部分主要熟悉了一下一些函数case when 之类的，需要注意的是：

* case when写的具体顺序非常的重要
* 能够在脑中构建自己的table关系

## Aggregation

#### Revising Aggregations - The Count Function

只要熟悉count function就可以了。

```sql
select count(*)
from city
where population > 100000;
```

#### Revising Aggregations - The Sum Function

只要熟悉sum function就可以了。

```sql
select sum(population)
from city 
where district = 'California';
```

#### Average Population

熟悉round就可以了。

```sql
select round(avg(population),0)
from city ;
```

#### Japan Population

逐步增加了一点难度。

```sql
select sum(population)
from city 
where countrycode = 'JPN';
```

#### Population Density Difference

只要熟悉max，min function就可以了。

```sql
select max(population) - min(population)
from city ;
```

#### The Blunder

这里主要注意replace函数也可以对number有效，还有就是不用round而使用ceil是因为需要向上取整。

```sql
select ceil(avg(salary)-avg(replace(salary,'0','')))
from employees;
```

#### Top Earners

首先，我们需要得到总的earning，来进行排序，然后我们根据最大数的个数进行选择和计数就可以了。这个题需要想一想。

```sql
select salary*months as earnings, count(*) 
from employee
group by earnings
order by earnings desc 
limit 1;
```

#### Weather Observation Station 2

这里就是注意一下联合使用就可以。

```sql
select round(sum(lat_n),2),round(sum(long_w),2)
from station ;
```

#### Weather Observation Station 13

这个还是前面题的延续，我这里稍微练习了一下between和and的用法。

```sql
select round(sum(lat_n),4)
from station
where lat_n between 38.7880 and 137.2345;
```

#### Weather Observation Station 14

这里主要注意计算的顺序。

```sql
select round(max(LAT_N),4)
from station
where lat_n <137.2345;
```

#### Weather Observation Station 15

这里主要注意replace函数也可以对number有效，还有就是不用round而使用ceil是因为需要向上取整。

```sql
select round(long_w,4)
from station
where lat_n <137.2345 
order by lat_n desc
limit 1;
```

#### Weather Observation Station 16

```sql
select round(min(lat_n),4)
from station
where lat_n > 38.7780;
```

#### Weather Observation Station 17

这里主要注意replace函数也可以对number有效，还有就是不用round而使用ceil是因为需要向上取整。

```sql
select round(long_w,4)
from station
where lat_n > 38.7780
order by lat_n
limit 1;
```

#### Weather Observation Station 18

主要是熟悉一下round的用法。

```sql
select round((abs(max(lat_n) -min(lat_n))+
            abs(max(LONG_W) -min(LONG_W))),4)
from station
```

#### Weather Observation Station 19

主要熟悉曼哈顿距离和欧式距离就可以。

```sql
select round(sqrt(
        (max(lat_n) -min(lat_n))*(max(lat_n) -min(lat_n))    
        +  (max(LONG_W) -min(LONG_W))*(max(LONG_W) -min(LONG_W))
        )  ,4)        
from station
```

#### Weather Observation Station 20

这个是之前提到的median解法

```sql
select round(s.lat_n,4)
from station as s
where abs(  (select count(*) from station as s1 where s.lat_n>=s1.lat_n)
          - (select count(*) from station as s2 where s.lat_n<=s2.lat_n)) <=0
```

**小结**

Aggregation这一部分主要熟悉了一下一些常见的聚合函数就可以了。

