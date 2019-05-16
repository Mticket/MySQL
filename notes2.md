# 项目三：

超过5名学生的课（难度：简单）

创建如下所示的courses 表 ，有: student (学生) 和 class (课程)。

例如,表:

+---------+------------+

| student | class      | +---------+------------+ | A       | Math       | | B       | English    | | C       | Math       | | D       | Biology    | | E       | Math       | | F       | Computer   | | G       | Math       | | H       | Math       | | I       | Math       | | A      | Math       | +---------+------------+

编写一个 SQL 查询，列出所有超过或等于5名学生的课。

第一步：创建表

```mysql
CREATE TABLE email (
student VARCHAR(255),
class VARCHAR(255)
);
```

第二步：插入数据：

```mysql
INSERT INTO email VALUES('A','Math');
INSERT INTO email VALUES('B','English');
INSERT INTO email VALUES('C','Math');
INSERT INTO email VALUES('D','Biology');
INSERT INTO email VALUES('E','Math');
INSERT INTO email VALUES('F','Computer');
INSERT INTO email VALUES('G','Math');
INSERT INTO email VALUES('H','Math');
INSERT INTO email VALUES('I','Math');
INSERT INTO email VALUES('A','Math');
```

对上面的数据进行显示：

```mysql
SELECT student,class FROM courses;
```

得到：

```
+---------+----------+
| student | class    |
+---------+----------+
| A       | Math     |
| B       | English  |
| C       | Math     |
| D       | Biology  |
| E       | Math     |
| F       | Computer |
| G       | Math     |
| H       | Math     |
| I       | Math     |
| A       | Math     |
+---------+----------+
```

第三步：查询

```mysql
SELECT class FROM courses GROUP BY class HAVING COUNT(DISTINCT student)>=5;
```

得到：

```
+-------+
| class |
+-------+
| Math  |
+-------+
```

# 项目四：交换工资（难度：简单）

第一步：创建salary表

```mysql
CREATE TABLE salary (
id INT NOT NULL PRIMARY KEY,
name VARCHAR(255),
sex VARCHAR(255),
salary INT NOT NULL
);
```

第二步：插入数据

```mysql
INSERT INTO salary VALUES(1,'A','m',2500);
INSERT INTO salary VALUES(2,'B','f',1500);
INSERT INTO salary VALUES(3,'C','m',5500);
INSERT INTO salary VALUES(4,'D','f',500);
```

显示：

```mysql
SELECT id,name,sex,salary FROM salary;
```

得到：

```
+----+------+------+--------+
| id | name | sex  | salary |
+----+------+------+--------+
|  1 | A    | m    |   2500 |
|  2 | B    | f    |   1500 |
|  3 | C    | m    |   5500 |
|  4 | D    | f    |    500 |
+----+------+------+--------+
```

第三步：交换f/m值：

```mysql
UPDATE salary SET sex = CASE sex
WHEN 'f' THEN 'm'
WHNE 'm' THEN 'f'
END;
```

查询：

```mysql
SELECT id,name,sex,salary FROM salary;
```

得到：

```
+----+------+------+--------+
| id | name | sex  | salary |
+----+------+------+--------+
|  1 | A    | f    |   2500 |
|  2 | B    | m    |   1500 |
|  3 | C    | f    |   5500 |
|  4 | D    | m    |    500 |
+----+------+------+--------+
```

# 项目五：组合两张表 （难度：简单）

第一步：创建表一，并插入数据：

```mysql
CREATE TABLE Person(
PersonId INT NOT NULL PRIMARY KEY,
FirstName VARCHAR(255),
LastName VARCHAR(255)
);
INSERT INTO Person VALUES(1,'Kobe','Bryant');
INSERT INTO Person VALUES(2,'Magic','Johnson');
INSERT INTO Person VALUES(3,'Isaiah','Thomas');
```

显示表一：

```mysql
SELECT PersonId,FirstName,LastName FROM Person;
```

得到：

```
+----------+-----------+----------+
| personid | firstname | lastname |
+----------+-----------+----------+
|        1 | Kobe      | Bryant   |
|        2 | Magic     | Johnson  |
|        3 | Isaiah    | Thomas   |
+----------+-----------+----------+
```

创建表二，并插入数据：

```mysql
CREATE TABLE Address(
AddressId INT NOT NULL PRIMARY KEY,
PersonId INT NOT NULL,
City VARCHAR(255),
State VARCHAR(255)
);
INSERT INTO Address VALUES(1,2,'Los Angles','California');
INSERT INTO Address VALUES(2,3,'Houston','Texas');
INSERT INTO Address VALUES(3,4,'Miami','Florida');
```

显示表二：

```mysql
SELECT AddressId,PersonId,State FROM Address;
```

得到：

```
+-----------+----------+------------+------------+
| addressid | personid | city       | state      |
+-----------+----------+------------+------------+
|         1 |        2 | Los Angles | California |
|         2 |        3 | Houston    | Texas      |
|         3 |        4 | Miami      | Florida    |
+-----------+----------+------------+------------+
```

第二步：查找

```mysql
SELECT Person.FirstName, Person.LastName, Address.City, Address.State
FROM Person LEFT JOIN Address
ON Person.PersonId = Address.PersonId;
```

得到结果：

```
+-----------+----------+------------+------------+
| FirstName | LastName | City       | State      |
+-----------+----------+------------+------------+
| Magic     | Johnson  | Los Angles | California |
| Isaiah    | Thomas   | Houston    | Texas      |
| Kobe      | Bryant   | NULL       | NULL       |
+-----------+----------+------------+------------+
```

# 项目六：删除重复的邮箱（难度：简单）

第一步：创建表并插入数据

```mysql
CREATE TABLE email (
ID INT NOT NULL PRIMARY KEY,
Email VARCHAR(255)
);
INSERT INTO email VALUES('1','a@b.com');
INSERT INTO email VALUES('2','c@d.com');
INSERT INTO email VALUES('3','a@b.com');
```

对上面的信息进行显示：

```mysql
SELECT ID,Email FROM email;
```

得到：

```
+----+---------+
| id | email   |
+----+---------+
|  1 | a@b.com |
|  2 | c@d.com |
|  3 | a@b.com |
+----+---------+
```

第二步：查询

```mysql
SELECT MIN(id),email FROM email GROUP BY email HAVING COUNT(DISTINCT id)>=1；
```

得到：

```
+---------+---------+
| min(id) | email   |
+---------+---------+
|       1 | a@b.com |
|       2 | c@d.com |
+---------+---------+
```

