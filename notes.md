# 【任务一】

## 1.1 MySQL软件安装及数据库基础

1. 软件安装及服务器设置

   提示：如果官网下载速度很慢的话，可以使用国内镜像。

2. 使用图形界面软件Navicat for SQL

3. 数据库基础知识

   ==数据库==：保存有组织的数据的容器（通常是一个文件或一组文件）。

   ==关系型数据库==：采用了关系模型来组织数据的数据库。

   ==关系模型==指的就是二维表格模型，一个关系型数据库就是由二维表及其之间的联系组成的一个数据组织。

   ==关系==：一张二维表，每个关系都具有一个关系名，也就是表名。

   ==表==（table）：某种特定类型数据的结构化清单。

   ==列==（column）：表中的一个字段。所有表都是由一个或多个列组成的。（也叫属性、字段）

   ==行==（row）：表中的一个记录。（也叫元组、记录）

   ==主键==（primary key）：一列（或一组列），其值能够唯一标识表中每一行。（也叫关键字）

   ==关系模式==：指对关系的描述。其格式为：关系名(属性1，属性2， ... ... ，属性N)，在数据库中成为表结构。

   ==外键==：用于与另一张表的关联。是能确定另一张表记录的字段，用于保持数据的一致性。比如，A表中的一个字段，是B表的主键，那他就可以是A表的外键。

4. MySQL数据库管理系统

   数据库管理系统（DBMS）：负责实施对数据库的统一管理和控制，比如数据组织、数据操纵、数据维护、数据控制及保护和数据服务等。

## 1.2 MySQL基础（一）-查询语句



# #作业#

## 项目一：

查找重复的电子邮箱（难度：简单） 创建 email表，并插入如下三行数据 +----+---------+ | Id | Email   | +----+---------+ | 1  | a@b.com | | 2  | c@d.com | | 3  | a@b.com | +----+---------+

编写一个 SQL 查询，查找 Email 表中所有重复的电子邮箱。 根据以上输入，你的查询应返回以下结果： +---------+ | Email   | +---------+ | a@b.com | +---------+ 说明：所有电子邮箱都是小写字母。

第一步：创建表。

```mysql
CREATE TABLE email (
ID INT NOT NULL PRIMARY KEY,
Email VARCHAR(255)
);
```

第二步：插入数据。

```mysql
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

第三步：查找重复的电子邮箱：

```mysql
SELECT Email
FROM email 
GROUP BY email having count (Email)>1;
```

得到:

```
+---------+
| email   |
+---------+
| a@b.com |
+---------+
```

## 项目二：

查找大国（难度：简单） 创建如下 World 表 +-----------------+------------+------------+--------------+---------------+ | name            | continent  | area       | population   | gdp           | +-----------------+------------+------------+--------------+---------------+ | Afghanistan     | Asia       | 652230     | 25500100     | 20343000      | | Albania         | Europe     | 28748      | 2831741      | 12960000      | | Algeria         | Africa     | 2381741    | 37100000     | 188681000     | | Andorra         | Europe     | 468        | 78115        | 3712000       | | Angola          | Africa     | 1246700    | 20609294     | 100990000     | +-----------------+------------+------------+--------------+---------------+ 如果一个国家的面积超过300万平方公里，或者(人口超过2500万并且gdp超过2000万)，那么这个国家就是大国家。 编写一个SQL查询，输出表中所有大国家的名称、人口和面积。 例如，根据上表，我们应该输出: +--------------+-------------+--------------+ | name         | population  | area         | +--------------+-------------+--------------+ | Afghanistan  | 25500100    | 652230       | | Algeria      | 37100000    | 2381741      | +--------------+-------------+--------------+

第一步：创建表

```mysql
CREATE TABLE World (
name VARCHAR(50) NOT NULL,
continent VARCHAR(50) NOT NULL,
area INT NOT NULL,
population INT NOT NULL,
gdp INT NOT NULL
);
```

第二步：插入数据

```mysql
INSERT INTO World
  VALUES('Afghanistan','Asia',652230,25500100,20343000);
INSERT INTO World 
  VALUES('Albania','Europe',28748,2831741,12960000);
INSERT INTO World 
  VALUES('Algeria','Africa',2381741,37100000,188681000);
INSERT INTO World
  VALUES('Andorra','Europe',468,78115,3712000);
INSERT INTO World
  VALUES('Angola','Africa',1246700,20609294,100990000);
```

对上面的信息进行显示：

```mysql
SELECT name,continent,area,population,gdp FROM World;
```

显示：

```
+-------------+-----------+---------+------------+-----------+
| name        | continent | area    | population | gdp       |
+-------------+-----------+---------+------------+-----------+
| Afghanistan | Asia      |  652230 |   25500100 |  20343000 |
| Albania     | Europe    |   28748 |    2831741 |  12960000 |
| Algeria     | Africa    | 2381741 |   37100000 | 188681000 |
| Andorra     | Europe    |     468 |      78115 |   3712000 |
| Angola      | Africa    | 1246700 |   20609294 | 100990000 |
+-------------+-----------+---------+------------+-----------+
```

第三步：输出表中所有大国家的名称、人口和面积。

```mysql
SELECT name,populaiton,area  
FROM World
where(population>25000000 and gdp>20000000)
or area>3000000;
```

显示：

```
+-------------+------------+---------+
| name        | population | area    |
+-------------+------------+---------+
| Afghanistan |   25500100 |  652230 |
| Algeria     |   37100000 | 2381741 |
+-------------+------------+---------+
```



# 出现的错误及解决办法

==错误1：==ERROR 1820 (HY000): You must reset your password using ALTER USER statement befo
re executing this statement.

解决办法：修改用户密码

```mysql
mysql> alter user 'root'@'localhost' identified by 'yourpassword';
```

或者

```mysql
mysql> set password=password("youpassword");
```

==错误2：==ERROR 1630 (42000): FUNCTION yiibaidb.count does not exist. Check the 'Function Name Parsing and Resolution' section in.

解决办法：如sum() count() avg这些函数里面是这样子写的sum () sum和()分开了，不是挨着写的，所以报这个错。