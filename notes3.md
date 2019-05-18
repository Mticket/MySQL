# 项目七: 各部门工资最高的员工（难度：中等）

第一步：创建employee表和department表

```mysql
CREATE TABLE Employee(
Id INT NOT NULL PRIMARY KEY,
Name VARCHAR(255) NOT NULL,
Salary INT NOT NULL,
DepartmentId INT NOT NULL
);

CREATE TABLE Department(
Id INT NOT NULL PRIMARY KEY,
Name VARCHAR(255) NOT NULL
);
```

导入数据

```mysql
INSERT INTO Employee VALUES('1','Joe','70000','1');
INSERT INTO Employee VALUES('2','Henry','80000','2');
INSERT INTO Employee VALUES('3','Sam','60000','2');
INSERT INTO Employee VALUES('4','Max','90000','1');

INSERT INTO Department VALUES('1','IT');
INSERT INTO Department VALUES('2','Sales');
```

显示数据：

```mysql
SELECT Id,Name,Salary,DepartmentId FROM Employee;
SELECT Id,Name FROM Department;
```

得到：

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
|  1 | Joe   |  70000 |            1 |
|  2 | Henry |  80000 |            2 |
|  3 | Sam   |  60000 |            2 |
|  4 | Max   |  90000 |            1 |
+----+-------+--------+--------------+

+----+-------+
| Id | Name  |
+----+-------+
|  1 | IT    |
|  2 | Sales |
+----+-------+
```

第二步：查询每个部门工资最高的员工

```mysql
SELECT D.Name AS Department, E.Name As Employee, E.Salary
FROM Department D, Employee E
WHERE E.DepartmentId=D.Id and E.Salary = (SELECT MAX(Salary) FROM Employee WHERE DepartmentID=D.Id);
```

得到：

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| Sales      | Henry    |  80000 |
| IT         | Max      |  90000 |
+------------+----------+--------+
```

# 项目八: 换座位（难度：中等）

第一步：建表

```mysql
CREATE TABLE seat(
id INT NOT NULL PRIMARY KEY,
student VARCHAR(255) NOT NULL
);
```

输入数据：

```mysql
INSERT INTO seat VALUES('1','Abbot');
INSERT INTO seat VALUES('2','Doris');
INSERT INTO seat VALUES('3','Emerson');
INSERT INTO seat VALUES('4','Green');
INSERT INTO seat VALUES('5','Jeames');
```

显示：

```mysql
SELECT id,student FROM seat;
```

得到：

```
+----+---------+
| id | student |
+----+---------+
|  1 | Abbot   |
|  2 | Doris   |
|  3 | Emerson |
|  4 | Green   |
|  5 | Jeames  |
+----+---------+
```

第二步：改变同学们的座位：

```mysql
SELECT (
CASE
WHEN MOD(id,2) = 1 AND id = (SELECT COUNT(*) FROM seat) THEN id
WHEN MOD(id,2) = 1 
THEN id+1
ELSE id-1
END)
as id,student
FROM seat
ORDER BY id;
```

得到

```
+----+---------+
| id | student |
+----+---------+
|  1 | Doris   |
|  2 | Abbot   |
|  3 | Green   |
|  4 | Emerson |
|  5 | Jeames  |
+----+---------+
```

# 项目九: 分数排名（难度：中等）

第一步：创建表

```mysql
CREATE TABLE score(
id INT NOT NULL PRIMARY KEY,
score DECIMAL(5,2)
);
```

插入数据：

```mysql
INSERT INTO score VALUES('1','3.50');
INSERT INTO score VALUES('2','3.65');
INSERT INTO score VALUES('3','4.00');
INSERT INTO score VALUES('4','3.85');
INSERT INTO score VALUES('5','4.00');
INSERT INTO score VALUES('6','3.65');
```

显示：

```mysql
SELECT id,score FROM score;
```

得到：

```
+----+-------+
| id | score |
+----+-------+
|  1 |  3.50 |
|  2 |  3.65 |
|  3 |  4.00 |
|  4 |  3.85 |
|  5 |  4.00 |
|  6 |  3.65 |
+----+-------+
```

第二步：排名

```mysql
SELECT score,(SELECT COUNT(DISTINCT score)
FROM score
WHERE score>=s.score) AS r
FROM score AS  s
ORDER BY score DESC;
```

得到：

```
+-------+------+
| score | r    |
+-------+------+
|  4.00 |    1 |
|  4.00 |    1 |
|  3.85 |    2 |
|  3.65 |    3 |
|  3.65 |    3 |
|  3.50 |    4 |
+-------+------+
```
