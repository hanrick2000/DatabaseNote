# MSSQL 解答

MS SQL和MySQL最新版一样都可以调用Analytics Functions，这里主要就是练习一下。

#### 176. Second Highest Salary

不能使用window function，这里主要是无法应对返回值是null的情况，这里应该是MSSQL bug了。

```sql
SELECT DISTINCT e.Salary as SecondHighestSalary
FROM ( SELECT Salary, dense_rank() over(ORDER BY Salary desc) as rank
       FROM Employee) as e
where rank = 2) as temp
```

```sql
WITH maxVal AS (
    SELECT MAX(Salary) AS val
    FROM Employee)

SELECT MAX(Salary) AS SecondHighestSalary
FROM Employee AS e, maxVal AS m
WHERE e.Salary < m.val
```

#### 177. Nth Highest Salary

这里就是window function的运用了，

```sql
SELECT DISTINCT Salary 
FROM (SELECT Salary, dense_rank() over(ORDER BY Salary DESC) AS rank
      FROM employee) AS temp
WHERE rank = @N
```

#### 185. Department Top Three Salaries



```sql
WITH temp AS (
SELECT Salary, Name, DepartmentId, dense_rank() over(PARTITION by DepartmentId ORDER BY Salary desc) as Rank
FROM Employee)

SELECT Department.Name AS Department, temp.Name AS Employee, temp.Salary 
FROM temp, Department
WHERE temp.DepartmentId = Department.Id and temp.Rank < 4
ORDER BY Department.Name, temp.Salary desc, Employee
```

