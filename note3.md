项目十：行程和用户（难度：困难）

第一步：创建trips表和users表

```mysql
CREATE TABLE trips (
id INT NOT NULL PRIMARY KEY,
client_id INT NOT NULL,
driver_id INT NOT NULL,
city_id INT NOT NULL,
status VARCHAR(255),
request_at VARCHAR(255)
);

CREATE TABLE users(
users_id INT NOT NULL PRIMARY KEY,
banned VARCHAR(255),
role VARCHAR(255)
);
```

输入数据：

```mysql
INSERT INTO trips VALUES(1,1,10,1,'completed','2013-10-01');
INSERT INTO trips VALUES(2,2,11,1,'cancelled_by_driver','2013-10-01');
INSERT INTO trips VALUES(3,3,12,6,'completed','2013-10-01');
INSERT INTO trips VALUES(4,4,13,6,'cancelled_by_driver','2013-10-01');
INSERT INTO trips VALUES(5,1,10,1,'completed','2013-10-02');
INSERT INTO trips VALUES(6,2,11,6,'completed','2013-10-02');
INSERT INTO trips VALUES(7,3,12,6,'completed','2013-10-02');
INSERT INTO trips VALUES(8,2,12,12,'completed','2013-10-03');
INSERT INTO trips VALUES(9,3,10,12,'completed','2013-10-03');
INSERT INTO trips VALUES(10,4,13,12,'cancelled_by_driver','2013-10-03');

INSERT INTO users VALUES(1,'No','client');
INSERT INTO users VALUES(2,'Yes','client');
INSERT INTO users VALUES(3,'No','client');
INSERT INTO users VALUES(4,'No','client');
INSERT INTO users VALUES(10,'No','drivers');
INSERT INTO users VALUES(11,'No','drivers');
INSERT INTO users VALUES(12,'No','drivers');
INSERT INTO users VALUES(13,'No','drivers');
```

查询非禁止用户的取消率：

```mysql
SELECT Request_at AS Day,
   (SELECT COUNT(Status)
   FROM Trips b1 JOIN Users b2 ON b2.Users_Id = b1.Client_Id
   AND b2.Banned = 'No'
   WHERE Request_at = t1.Request_at AND Status IN ('cancelled_by_client' , 'cancelled_by_driver')) / COUNT(t1.Status) AS Rate
FROM Trips t1, Users t2
WHERE t2.Users_Id = t1.Client_Id
   AND t2.Banned = 'No'
   AND t1.Request_at BETWEEN '2013-10-01' AND '2013-10-03'
GROUP BY t1.Request_at
ORDER BY t1.Request_at
```

得到：

```
+------------+--------+
| Day        | Rate   |
+------------+--------+
| 2013-10-01 | 0.3333 |
| 2013-10-02 | 0.0000 |
| 2013-10-03 | 0.5000 |
+------------+--------+
```

项目十一：各部门前3高工资的员工（难度：中等）

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
INSERT INTO Employee VALUES('5','Janet','69000','1');
INSERT INTO Employee VALUES('6','Randy','85000','1');

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
|  5 | Janet |  69000 |            1 |
|  6 | Randy |  85000 |            1 |
+----+-------+--------+--------------+

+----+-------+
| Id | Name  |
+----+-------+
|  1 | IT    |
|  2 | Sales |
+----+-------+
```

第二步：查询每个部门工资前3高的员工

```mysql
SELECT 
    t2.Name as Department, t1.Name as Employee, t1.Salary 
FROM
    Employee t1 join Department t2 on t1.DepartmentId = t2.Id
WHERE
    t1.Id IN (SELECT 
            Id
        FROM
            (SELECT 
                Id
            FROM
                Employee
            WHERE
                DepartmentId = t1.DepartmentId 
                ORDER BY Salary DESC
            LIMIT 3) tt)
ORDER BY DepartmentId , Salary DESC
```

得到：

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      |  90000 |
| IT         | Randy    |  85000 |
| IT         | Joe      |  70000 |
| Sales      | Henry    |  80000 |
| Sales      | Sam      |  60000 |
+------------+----------+--------+
```

项目十二 分数排名 - （难度：中等）

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
SELECT score,(SELECT COUNT(score)+1
FROM score
WHERE score>s.score) AS r
FROM score AS  s
ORDER BY score DESC;
```

得到：
+-------+---+
| score | r |
+-------+---+
| 4     | 1 |
| 4     | 1 |
| 3.85  | 3 |
| 3.65  | 4 |
| 3.65  | 4 |
| 3.5   | 6 |
+-------+---+
