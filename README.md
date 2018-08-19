---
description: 最近重新刷了Leetcode的database部分，把题解记录下来以便于以后的分析和复习。
---

# Leetcode Database Solutions

## 导言

主要数据库版本：MySQL - \(类似postgre\)

我主要自己使用的是MySQL数据库，因为这个用的比较多。从MySQL 8.0之后，CET以及Window Function都可以使用了，但是在Leetcode的系统中应该还是旧版本的MySQL 5.X系列。

本来是按照题号一个一个进行了，后来觉得这样不利于自己后面的复习，就将它进行切块，变成了一部分一部分的。

## Easy Query

#### 182. Duplicate Emails

难度：Easy

```sql
select email as Email
from person
group by email
having count(email)>1
```

#### 570. Managers with at Least 5 Direct Reports

难度：Easy

思路：这个题很简单，用having 控制好就可以。

```sql
select e1.Name
from employee as e , employee as e1
where e.managerid = e1.id 
group by e1.name
having count(*)>4
```

**577. Employee Bonus**

难度：Easy

```sql
select e.name,b.bonus
from employee as e
left join bonus as b
on e.empid = b.empid
where b.bonus <1000 or b.bonus is null
```

#### 584. Find Customer Referee

难度：Easy

```sql
select  c.name
from customer as c
where c.referee_id != 2 or c.referee_id is null
```

#### 586. Customer Placing the Largest Number of Orders

难度：Easy

求最大值，掌握order by x limit就行

```sql
select customer_number
from orders
group by customer_number
order by count(*) desc
limit 1
```

#### 595. Big Countries

难度：Easy

```sql
select name, population, area
from World
where  area > 3000000 or population > 25000000
order by population;
```

#### 596. Classes More Than 5 Students

难度：Easy

```sql
select distinct class
from courses
group by class
having count(distinct student)>=5
```

#### 610. Triangle Judgement

难度：Easy

```sql
select x,y,z ,
        case when (x+y)>z and (x+z)>y and (y+z)>x then 'Yes'
             else 'No'
        end as triangle
from triangle
```

#### 619. Biggest Single Number

难度：Easy

```sql
select ifnull((select num
    from  number
group by num
having count(*)=1
order by num desc
limit 1),null) as num
```

#### 620. Not Boring Movies

难度：Easy

```sql
select *
from cinema
where id %2 <> 0 and description <>'boring'
order by rating desc
```

#### 626. Exchange Seats

难度：Easy

```sql
select 
    case when id %2 =0 then id -1
         when id %2 = 1 and (id+1) in (select id from seat) then id +1
         else id
    end as id, student
from seat
order by id 
```

#### 627. Swap Salary

难度：Easy

```sql
UPDATE salary
SET sex = IF(sex='f','m','f');
```

## Join

### Left Join

#### 175. Combine Two Tables

难度：Easy

这个题主要考察对表的Join，因为右边要求Null也可以，所以使用Left Join就行。

```sql
select FirstName, LastName,City, State
from person
left join address on person.personid = address.personid
```

#### 181. Employees Earning More Than Their Managers

难度：Easy

这个题主要考察对表的Join，因为没有涉及Null，其实Left和Inner都可以。

```sql
select e.name as Employee 
from employee as e 
left join employee as e1 
on e.managerid = e1.id 
where e.salary > e1.salary
```

#### 183. Customers Who Never Order

难度：Easy

这个题主要考察对表的Join，因为没有涉及Null，其实Left和Inner都可以

```sql
select name as customers
from customers
left join orders on customers.id = orders.CustomerId
where orders.id is null
```

#### 580. Count Student Number in Departments

难度：Medium

```sql
select d.dept_name,ifnull(temp.num,0) as student_number
from (select dept_id, count(*) as num
     from student
     group by dept_id) as temp
right join department as d
on d.dept_id = temp.dept_id
order by student_number desc,d.dept_name

```

#### 607. Sales Person

难度：Easy

这个题主要提示我们的是，需要想清楚left join还是inner join，主要在于数据会丢失。一般推荐都用left join + null来filter即可。

```sql
select s.name
from salesperson as s
where s.sales_id not in (select o.sales_id
                         from orders as o
                         left join company as c 
                         on o.com_id = c.com_id 
                         where c.name = 'RED')
```

#### 608. Tree Node

难度：Medium

这个题考察的join，主要要理清思路这个最为重要，如果节点母节点为null，则其为root，如果其子节点为null，则为leaf

* 第0层 ： node0.p\_id 如果这个是null，则为root
* 第1层 ：node0.id
* 第2层 ：node1.id 如果这个是null，则为leaf

```sql
select distinct node0.id as Id ,case when node0.p_id  is null then "Root"
            when node1.id is null then 'Leaf'
            else 'Inner'
            End as Type
from tree as node0
left join tree as node1
on node0.id = node1.p_id 

```

### Union

#### 602. Friend Requests II: Who Has the Most Friends

难度：Medium

主要是一个value pair，union之后任取一边计算就可以。

* Union不含重复的，union all 含有重复的

```sql
select accepter_id as id,count(*) as num
from (select t1.requester_id,t1.accepter_id
     from request_accepted as t1
     union all
     select t2.accepter_id as requester_id, t2.requester_id as accepter_id
     from request_accepted as t2) as temp
group by accepter_id
order by count(*) desc
limit 1

```

### Subquery - Self Join

#### 196. Delete Duplicate Emails

难度：Easy

思路：主要考察自连接，很简单。

```sql
delete p1
from Person as p1, Person as p2
where p1.email = p2.email and p1.id >p2.id
```

#### 197. Rising Temperature

难度：Easy

思路：同之前的思路一样，先虚构两个表，一个左一个右，控制左边的日期比右边的日期大一，然后用温度做filter，然后去id就行。

```sql
select leftw.Id
from weather as leftw, weather as rightw
where leftw.temperature > rightw.temperature and leftw.RecordDate = DATE_ADD(rightw.RecordDate, INTERVAL 1 DAY)
```

#### 612. Shortest Distance in a Plane

难度：Medium

```sql
select distinct round(sqrt((p1.x-p2.x)*(p1.x-p2.x) + (p1.y-p2.y) * (p1.y-p2.y)),2) as shortest
from point_2d as p1, point_2d as p2
order by shortest 
limit 1 offset 1
```

#### 613. Shortest Distance in a Line

难度：Easy

```sql
select sqrt(abs(p1.x*p1.x - p2.x*p2.x)) as shortest 
from point as p1, point as p2
where (p1.x*p1.x - p2.x*p2.x) != 0
order by shortest 
limit 1;
```

**614. Second Degree Follower**

难度：Easy

```sql
select f1.follower, count(distinct f2.follower) as num
from follow f1
join follow f2 on f1.follower = f2.followee
group by f1.follower
order by f1.follower;
```

### Subquery - Self Join - Consecutive

#### 180. Consecutive Numbers

难度：Medium

思路：这个题其实很简单，脑海中有三个表就可以，分别是左-中-右。先将它们自连接起来，然后依次控制左-中-右递增，就可以了，很简单。

```sql
select distinct mid.num as ConsecutiveNums
from logs as le,logs as mid, logs as ri
where le.num = mid.num and mid.num = ri.num and 
       le.id = mid.id +1 and mid.id = ri.id +1
```

#### 601. Human Traffic of Stadium

难度：Hard

思路：其实这种连续的非常简单，只要把每种情况考虑好就行。这里如果有三个数A，B，C，考虑三种情况，所有情况都以左为主，只考虑M

* 最左的时候 ：M=B-1 and B =C-1 \(M为A\)
* 中间的时候 ：A+1 =M and M = C-1 \(M为B\)
* 最右的时候 ：A = B-1 and B = M-1 \(M为C\)

这种方法必然存在重复，只要去掉重复就可以了

```sql
select distinct s1.id,s1.date,s1.people
from stadium as s1, stadium as s2, stadium as s3
where s1.people >=100 and s2.people >= 100 and s3.people >=100
     and ((s1.id = s2.id -1 and s2.id = s3.id -1) or (s3.id = s2.id -1 and s2.id = s1.id -1) or (s1.id = s3.id -1 and s1.id = s2.id +1))
order by s1.id                                                     
```

#### 603. Consecutive Available Seats

难度：Easy

思路：基本逻辑不变，主要在于分析清楚哪两种情况就可以

```sql
select distinct c1.seat_id
from cinema as c1, cinema as c2
where (c1.seat_id = c2.seat_id - 1 or c1.seat_id = c2.seat_id +1) 
      and c1.free = c2.free and c1.free =1
order by c1.seat_id      
```

## Advanced Subquery 

### Subquery 

#### 184. Department Highest Salary

难度: Medium

这个题值得一说的地方在于陷阱，一开始想的非常简单，先将表Join起来，然后按照Department分组就行，这里要提一下这个具体的运作机制。

* 这里的问题是，因为只是按Deparment分的，所以如果选取了Name，就会自动选取第一个，因此并不对，所以比较推荐的使用Subeuery或者Inner Join。

```sql
select d.name as Department, e.name as Employee, e.Salaryfrom employee as e
join department as d
on e.departmentid = d.id
where (e.DepartmentId,Salary) in  (select DepartmentId,max(salary)                                
                                    from employee                                
                                    group by DepartmentId)                                                                        
order by e.Salary
```

#### 185. Department Top Three Salaries

难度: Hard

这个题是非常有意思的一道题，和前一道题非常像，这里的局限在于MySQL不能再子查询中使用Limit，所以无法直接取Top 3，但是我们之前已经习惯如果取Rank，所以变相使用了Rank的方法来得到Top 3。

```sql
select d.name as Department, e.name as Employee, e.Salary 
from department as d
join employee as e
on d.id = e.DepartmentId 
where (select count(*) 
        from (select distinct salary,DepartmentId from employee) as e1 
        where e.salary < e1.salary and e.departmentid = e1.departmentid) <3
order by e.departmentid , e.Salary desc
```

#### 574. Winning Candidate

难度: Medium

这个题本身不是很难，但是我卡了一段时间，主要是有几个细节没有注意到，这道题能够提供两个非常重要的知识点。

* 虽然Subquery不能使用Limit，但是可以将它写在from的表中，这时系统并不认为这是subquery，我猜测可能这是系统读取的第一个表，自然不是subquery，如果用in之类的，则已经存在了query
* 对于取N个最大数使用order by x desc limit N即可，这个之前提到过

陷阱：之所以卡在这里是要注意题意和corner case，不能先将两个表join起来，如果vote最大的id不在candidate中，那么返回的将是另一个id，这个是非常值得注意的。要找到vote中最大的id，然后再join。

```sql
select c.Name
from (select v.candidateid, count(v.candidateid) as num
      from vote as v
      group by v.candidateid
      order by num desc
      limit 1 
) as temp
join candidate as c 
on c.id = temp.candidateid
```

**585. Investments in 2016**

难度: Medium

这个题不是很难，主要考察多表之间的子查询，先选出所有城市是唯一的数据，再控制TIV\_2015不是唯一的就行，符合这两个条件的必然是满足要求的结果。

```sql
select sum(i.TIV_2016) as TIV_2016
from (select lat,lon from insurance group by lat,lon having count(*)=1) as latlon
join insurance as i
on concat(i.lat,i.lon) = concat(latlon.lat,latlon.lon)
where i.TIV_2015 in (select TIV_2015 from insurance group by TIV_2015 having count(*)!=1)
```

#### 615. Average Salary: Departments VS Company

难度：Hard

这个题并不难就是繁琐，把好几个表都并在了一起，如果用window function就简单多了。

思路：先按时间和部门分组计算平均值，再固定时间计算平均值，最后在比较两个值就可以。

```sql
select temp.pay_month,temp.department_id,
        case when temp.salary>temp.depSa then 'higher'
             when temp.salary=temp.depSa then 'same'
        else 'lower' 
        end as comparison
from ( select left(s.pay_date,7) as pay_month,e.department_id, 
      avg(s.amount) as salary, (select avg(s1.amount)
                                from salary as s1, employee as e1
                                where s1.employee_id = e1.employee_id and left(s.pay_date,7)  = left(s1.pay_date,7) ) as depSa
      from salary as s, employee as e
      where s.employee_id = e.employee_id
      group by pay_month, e.department_id ) as temp
order by department_id, pay_month 
```

### Subquery - Ranks

#### 178. Rank Scores

这里涉及到一个比较重要的概念，是在刷题中遇到的问题，就是一种嵌套的循环。这套technique的技巧在于，固定一个值，再用于其他的子查询中。在后面的running total里面也会遇到同样的问题。

类似于Python的循环，我们在给定i的时候，去看后面的循环或query

```python
for i in iList:
    for j in jList:
        Count++
```

思路：先找到每一个score，然后新建一个query 去看比这个score大的元素，这就形成了rank

* 要注意是否允许两个元素并列第一的问题。具体来说，如果三个人，两个第一，一个第二，那么第二是算作第三还是第二。这个问题主要通过distinct来控制。
* 要小心在子查询中比较的符号，因为这影响了升序和降序。

```sql
select s1.score as Score,   # 第一次s1.score
        (select count(*) 
        from (select distinct score from scores) s2 
        where s1.score<s2.score)+ 1 as Rank # 第二次s1.score 
from scores s1
order by score desc
```

#### 176. Second Highest Salary

难度：Easy

这个题不是特别难，但是corner case比较多，我也是一直在试错，这里可以看出自己的功底还是不扎实，想问题想的不是很全面。

* 就是当只有一行的时候，应该返回Null
* 如果出现并列的情况，应当注意

思路：首先仅取出表中不重复的Salary，然后排序按Salary升序排序即可，在取数据的时候，从第二行开始取仅取一个\(**Limit 1 Offset 1\)**

```sql
select ifnull(
                (select distinct Salary
                from employee
                order by salary desc
                limit 1 offset 1)
                , null) as SecondHighestSalary
```

受后面题的影响，这里还有另一种解法：

主要给出Salary的Rank，然后根据Rank选择排名第二的就可以，这种可以试用于N个最大的。

```sql
SELECT e1.Salary  as  SecondHighestSalary   
FROM (SELECT DISTINCT Salary FROM Employee) as e1      
WHERE (SELECT COUNT(*) 
        FROM (SELECT DISTINCT Salary FROM Employee) as e2 
        WHERE e2.Salary > e1.Salary) = 1            
LIMIT 1
```

#### 177. Nth Highest Salary

难度: Medium

这里强制写成函数的目的是强迫使用N，其实思路前一个题已经说了，主要是要求可以Rank，对于Rank我们已经比较熟悉了，就可以直接写了。

```sql
SELECT e1.Salary
FROM (SELECT DISTINCT Salary FROM Employee) as e1
      WHERE (SELECT COUNT(*) 
            FROM (SELECT DISTINCT Salary FROM Employee) as e2 
            WHERE e2.Salary > e1.Salary) = N - 1      
LIMIT 1
```

### Subquery - Median

#### 569. Median Employee Salary

难度: Hard

这个题是非常经典的median的题目，对于求median 的题目要牢记这个技巧，因为median其实本质是和Rank有关的，所以还是从Rank出发。

**奇数情况** 

在奇数的时候，比最大的数大的是0，第二大是1，第三大是2，在正反排序下面，两者的差为0，由此很容易找出中位数，这里的差值指的是在这边数字左边和数字右边数的差距。

| 正向排序 | 0 | 1 | 2 |
| :--- | :--- | :--- | :--- |
| 反向排序 | 2 | 1 | 0 |
| 差 | -2 | 0 | 2 |

**偶数情况** 

在奇数的时候，同上，两者的差为-1或1，由此很容易找出中位数。

| 正向排序 | 0 | 1 | 2 | 3 |
| :--- | :--- | :--- | :--- | :--- |
| 反向排序 | 3 | 2 | 1 | 0 |
| 差 | -3 | -1 | 1 | 3 |

使用这种方法求median的问题在于，会出现重复的，回想一下之前写的双循环结构，解决办法是，采用group by 字段的方法，利用之前提到的自动选取第一个id，来做一个独立。



```sql
select distinct e.Id ,e.Company, e.Salary
from Employee as e
where abs((select count(*) from employee as e1 where e.salary > e1.salary and e.company = e1.company) 
- (select count(*) from employee as e2 where e.salary < e2.salary and e.company = e2.company)) < 2 
group by  e.company,e.salary                                                                                            
order by e.company,e.salary
```

**571. Find Median Given Frequency of Numbers**

难度: Hard

思路：这个题非常非常非常好，融合和running total和rank。首先，一看到是求median，自然入手就是rank，那么这里不太一样的地方给出的是具体的频数，要把它变成rank。这里就用到了running total。接下来就是之前提到的技巧，用正反排序进行计算。

| 数 | 0 | 1 | 2 | 3 |
| :---: | :---: | :---: | :---: | :---: |
| 频数 | 7 | 1 | 3 | 1 |
| 正向 | 7 | 8 | 11 | 12 |
| 反向 | 12 | 5 | 4 | 1 |
| 差值 （左右的差值） | -5 | 3 | 7 | 11 |

之前已经很明确的是，如果一个数是中位数，那么比它小的和比它大的应该一样多。我们寻找中位数的思路就在这里。

首先以0为例子，它的左边比右边少5个，为了使得平均分它，必须从0中分出5个拨给左边，如果这样划拨之后，0还有剩余的数字，这里是2（频数是7），那么中位数就在0之中。

| 左 | 0 | median \(0\) | 右 |
| :---: | :---: | :---: | :---: |
| 0 | 5 | 2 | 5 |

然后以1为例子，它的左边比右边多3个，为了使得平均分它，必须从1中分出3个拨给右边，然而1只有1个，所以不够拨给右边，自然1不可能是中位数。

| 左 | median \(0\) | 1 | 右 |
| :---: | :---: | :---: | :---: |
| 3 | 2 | 1 | 0 |

利用这样的思路，我们就可以计算出最终的中位数了，需要注意的是如果中位数在0，1之间，那么应该是0.5，所以最好需要取平均。

```sql
select  n.Number median
from Numbers n
where n.Frequency >= abs((select sum(Frequency) from Numbers where Number<=n.Number) -
                         (select sum(Frequency) from Numbers where Number>=n.Number))
```

### Subquery - Ratio

#### 262. Trips and Users

难度：Hard

思路：SQL其实基本没有hard题，所谓的hard也就是将多步并成一部，想解决这类问题就是找到突破口各个击破。

分析：这类要算的是**Cancellation Rate**，这就是突破口，这个比例是由两个指标相除得到的，由此可以用前面提到的理念做个子查询算。比较简单的方法就是直接用case when统计。基本这里就可以直接算出来了。

```sql
select t.Request_at as Day, round(sum(case when t.status in ('cancelled_by_driver','cancelled_by_client') then 1
                            else 0
                            end)/count(*),2 )as 'Cancellation Rate'
from trips as t
where t.Client_Id in (select Users_Id from users where Banned = 'No') and t.Request_at BETWEEN '2013-10-01' AND '2013-10-03'
group by t.Request_at
```

#### 578. Get Highest Answer Rate Question

难度：Medium

思路：Ratio类的突破口就在ratio，很简单。

* 如果条件是单一的话，可以用if替代case when

```sql
select question_id as survey_log
from (select question_id, sum(if(s.action = 'answer',1,0))/ sum(if(s.action = 'show',1,0)) as rate
      from survey_log as s
      group by question_id
      order by rate desc
      limit 1) as temp
                        
```

#### 597. Friend Requests I: Overall Acceptance Rate

难度：Easy

思路：老话，Ratio类的突破口就在ratio，这两个表示独立的，算一下就行。

```sql
select
round(
    ifnull(
    (select count(*) from (select distinct requester_id, accepter_id from request_accepted) as A)
    /
    (select count(*) from (select distinct sender_id, send_to_id from friend_request) as B),
    0)
, 2) as accept_rate;
```

### Subquery - Running Total

#### 579. Find Cumulative Salary of an Employee

难度: Hard

这个题是需要用到的是Running Total的相关知识，首先求所有的running total，之后用时间来限制就可以了，其实本质还是window function的一种，可惜这里不能使用window function。

```sql
select e.Id as id,e.month as month,(select sum(e1.salary) 
                                 from employee as e1 
                                 where e.id = e1.id and e.month < e1.month+3 and e.month>e1.month-1 ) as Salary
from employee as e
having Salary is not null and (e.Id,e.month) not in (select Id, max(month) as month from employee group by id  )
order by e.Id,e.Month desc
```

#### 618. Students Report By Geography 

难度：Very Hard 

这个题还不太会，先放在这，有时间复习一下assignment

```sql
select max(america) as america, max(asia) as asia, max(europe) as europe
from (
    select 
        if(@pre = @pre := continent, @row := @row + 1, @row := 0) as row_count, 
        if(continent = 'America', name, null) as america,
        if(continent = 'Asia', name, null) as asia,
        if(continent = 'Europe', name, null) as europe
    from student, (select @pre := '0', @row := 0) v
    order by continent,name
) t
group by row_count
order by row_count
;
```

{% hint style="info" %}
二刷完真的累，下一次总结一下window function。
{% endhint %}

