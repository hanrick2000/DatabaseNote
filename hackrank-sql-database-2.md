---
description: 接上面的题，如果都放在一页编辑的时候有些卡。
---

# Hackrank SQL database \(2\)

## Basic Join

#### Asian Population

基本的inner join要非常熟悉，这是一个简单题。

```sql
select sum(c1.population)
from city as c1, country as c2
where c1.countrycode = c2.code and c2.continent = 'Asia';
```

#### African Cities

```sql
select c1.name
from city as c1, country as c2
where c1.countrycode = c2.code and c2.continent = 'Africa';
```

#### Average Population of Each Continent

注意一下floor函数和之前的ceil函数。

```sql
select c2.continent, floor(avg(c1.population))
from city as c1, country as c2
where c1.countrycode = c2.code
group by c2.continent;
```

#### The Report

这里需要熟悉cross join，具体的内容不再赘述。

```sql
select if(g.grade>7,s.name,'NULL') as name, g.grade,s.marks
from grades as g , students as s
where s.marks between g.min_mark and g.max_mark
order by g.grade desc,s.name,s.marks ;
```

#### Top Competitors

这是一个比较复杂的多表查询，整体来说不是很复杂，需要理清逻辑。

```sql
select h.hacker_id, h.name
from hackers as h, difficulty as d, challenges as c, submissions as s
where h.hacker_id = s.hacker_id and s.challenge_id = c.challenge_id and c.difficulty_level = d.difficulty_level and s.score = d.score
group by h.hacker_id, h.name
having COUNT(s.hacker_id) > 1
order by count(h.hacker_id) desc ,h.hacker_id；
```

#### Ollivander's Inventory

这个题主要是需要先出来需要的columns，但是因为group by必须按照select中的条件，所以需要再写一个subquery。

```sql
select wands.id, min_prices.age, wands.coins_needed, wands.power
from wands 
inner join (select wands.code, wands.power, 
                   min(wands_property.age) as age, 
                   min(wands.coins_needed) as min_price
            from wands
            inner join wands_property
            on wands.code = wands_property.code
            where wands_property.is_evil = 0
            group by wands.code, wands.power) min_prices
on wands.code = min_prices.code
   and wands.power = min_prices.power
   and wands.coins_needed = min_prices.min_price
order by wands.power desc, min_prices.age desc
```

#### Challenge

这个题不难，但是太麻烦了，从challenge入手，首先小于最大的话我们只要频数为1的，而如果是最大的，我们又不能舍弃，因此我们有了两个分支，接下来就很简单了，深感MySQL不能用CTE函数非常不方便。

```sql
select H.hacker_id, H.name, count(*) as total
from Hackers as H, Challenges as C
where H.hacker_id = C.hacker_id
group by H.hacker_id, H.name
having total = 
    (select count(*) 
     from challenges
     group by hacker_id 
     order by count(*) desc limit 1
     )
or total in
    (select total
     from (
        select count(*) as total
        from Hackers as H, Challenges as C
        where H.hacker_id = C.hacker_id
        group by H.hacker_id, H.name
      ) as counts
     group by total
     having count(*) = 1)
order by total desc, H.hacker_id asc
;
```

#### Contest Leaderboard

这里的主要问题是这个是submission的table，所以里面有很多重复的内容，只能取最高的内容，因此需要做一些子查询。

```sql
select h.hacker_id, h.name, sum(score) as total 
from hackers as h 
inner join  (select s.hacker_id, s.challenge_id, max(s.score) as score 
             from submissions as s
             group by s.hacker_id, s.challenge_id 
             having scr > 0) as t1 
on h.hacker_id = t1.hacker_id
group by h.hacker_id ,h.name 
order by total desc, h.hacker_id;

```

**小结**

Basic Join 这一部分真的不难，需要注意的是：

* Left join 和inner join的写法区别和应用
* 如果逻辑比较复杂就分成小块去写就可以了，这样也比较容易。

## Advanced Join

#### Projects

这里的主要思路是先找出所有的start date和end date，然后做一个cross join，找出所有符合条件的时间，关键是这样计算有个问题就是所有的开始和结束进行了随机匹配，所以需要用min来控制找到了最小的那个。

```sql
SELECT Start_Date, MIN(End_Date)
FROM 
    (SELECT Start_Date FROM Projects WHERE Start_Date NOT IN (SELECT End_Date FROM Projects)) a,
    (SELECT End_Date FROM Projects WHERE End_Date NOT IN (SELECT Start_Date FROM Projects)) b 
WHERE Start_Date < End_Date
GROUP BY Start_Date
ORDER BY DATEDIFF(MIN(End_Date), Start_Date) ASC, Start_Date ASC;
```

#### Placement

需要想清楚这三个表join的基本逻辑，只要搞清楚这个基本逻辑就可以了。

```sql
select s.name
from students as s, friends as f, packages as p, packages as p1
where s.id = f.id and s.id = p.id and f.friend_id = p1.id 
      and p.salary < p1.salary
order by p1.salary;
```

#### Symmetric Pairs

这个题还是一个挺有意思的题，第一次写没有写出来，后来看了discussion才找到了解决方法。主要是分类讨论的问题，一种是x!=y，另一种是x=y。

* x != y 时，就是正常的写法
* x = y 时， 算出大于一的就可以

我之前的写法如下，最大的问题在于可能会丢一些点，因为是按照如下写的话，只出现一次的点比如（1，1）也会被计算进去，就会存在一些问题。

```sql
select distinct f1.x, f1.y
from functions as f1, functions as f2
where f1.x = f2.y and f1.y = f2.x and f1.x <= f2.
order by f1.x;
```

修改之后的答案如下：

```sql
select x, y 
from functions f1 
    where exists(select * from functions f2 where f2.y=f1.x 
    and f2.x=f1.y and f2.x>f1.x) and (x!=y) 

union 

select x, y 
from functions f1 where x=y  
    GROUP BY x, y
    HAVING COUNT(*) > 1
order by x;
```

#### Interviews

这个题主要涉及了并表的事宜，比较繁琐，如果使用subquery的话阅读不是很清晰，这里我就直接使用了cte函数，做成temporary的表。

```sql
with aaa as(
select
a.contest_id, sum(d.total_views) as totv, sum(total_unique_views) as totuv
from contests a
join colleges b on a.contest_id=b.contest_id
join challenges c on c.college_id = b.college_id
join view_stats d on  d.challenge_id =c.challenge_id
    group by a.contest_id
)
,
bbb as
(
select
a.contest_id, sum(d.total_submissions) as tots, sum(total_accepted_submissions) as totas
from contests a
join colleges b on a.contest_id=b.contest_id
join challenges c on c.college_id = b.college_id
join Submission_Stats  d on  d.challenge_id =c.challenge_id
     group by a.contest_id
)

select a.contest_id, hacker_id, name, bbb.tots, bbb.totas, aaa.totv, aaa.totuv
from contests a
join aaa on a.contest_id=aaa.contest_id
join bbb on a.contest_id=bbb.contest_id
WHERE bbb.tots >0 OR bbb.totas> 0 OR aaa.totv >0 OR  aaa.totuv >0

```

#### 15 Days of Learning SQL

还是使用cte函数进行优化，到hard难度的题慢慢没有意思了，就是很多个复杂的表直接进行Join。

```sql
with Sub1 as (
    select s1.submission_date, s1.hacker_id,
    count (distinct s1.submission_id) as date_submissions,
    1 + datediff(day, 'March 1, 2016', s1.submission_date) as contest_day,
    count (distinct s2.submission_date) as submission_days
    from Submissions s1 join Submissions s2
        on s1.hacker_id = s2.hacker_id and s1.submission_date >= s2.submission_date
    group by s1.submission_date, s1.hacker_id
),
Sub2 as (
    select submission_date, hacker_id, date_submissions
    from Sub1 where submission_days = contest_day
),
Sub3a as (
    select submission_date, count(hacker_id) as hackers
    from Sub2 group by submission_date
),
Sub3b as (
    select submission_date, max(date_submissions) as max_submissions
    from Sub1 group by submission_date
),
Sub4 as (
    select s1.submission_date, s3.hackers,
        (select top 1 s2.hacker_id from Sub1 s2
         where s1.submission_date = s2.submission_date and s1.max_submissions = s2.date_submissions
         order by s2.hacker_id) as hacker_id
    from Sub3b s1
    join Sub3a s3 on s1.submission_date = s3.submission_date
)
select s.*, h.name from Sub4 s join Hackers h on s.hacker_id = h.hacker_id order by submission_date;
```

## Alternative Queries

#### Draw The Triangle 1

这个题挺好的，因为涉及到assignment的题不是很多，这里详细解释了如何设一个变量，然后根据这个变量输出参数。

* set @var先给定一个参数的值
* 选择一个tabel大于20行，这里就是随便选择了一个MySQL自带的系统table，理论上可以随便选，因为并不涉及到任何columns
* var小于0会自动结束

```sql
set @number = 21;
select repeat('* ', @number := @number - 1) 
from information_schema.tables;

```

#### Draw The Triangle 2

再联系一个，这个就很类似了，如果不能用负的自动结束的话，就要用一个限制来封死它的上限，不然就会无限循环。

```sql
set @row := 0;
select repeat('* ', @row := @row + 1) 
from information_schema.tables 
where @row < 20

```

#### Print Prime Number

这个不会就直接抄答案了... 有时间再看，这个感觉考不到。

```sql
SELECT GROUP_CONCAT(NUMB SEPARATOR '&')
FROM (
    SELECT @num:=@num+1 as NUMB FROM
    information_schema.tables t1,
    information_schema.tables t2,
    (SELECT @num:=1) tmp
) tempNum
WHERE NUMB<=1000 AND NOT EXISTS(
        SELECT * FROM (
            SELECT @nu:=@nu+1 as NUMA FROM
                information_schema.tables t1,
                information_schema.tables t2,
                (SELECT @nu:=1) tmp1
                LIMIT 1000
            ) tatata
        WHERE FLOOR(NUMB/NUMA)=(NUMB/NUMA) AND NUMA<NUMB AND NUMA>1
```

## Summary

Hackrank的题除了hard以外基本都很好，而且都使用的是几个常用的表，所以比较适合基本的练习，很多题用重复很多次的方式来强化特定的知识，有机会慢慢补充吧。

还是window function比较重要。

