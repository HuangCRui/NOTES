# SQL





1） 什么是SQL ？

**结构化查询语言**(Structured Query Language)简称SQL，是一种特殊目的的编程语言，是一种数据库查询和程序设计语言，用于存取数据以及查询、更新和管理关系数据库系统。

2） SQL 的作用

是所有关系型数据库的统一查询规范，不同的关系型数据库都支持SQL
所有的关系型数据库都可以使用SQL
不同数据库之间的SQL 有一些区别  













| 分类          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| 数据定义语 言 | 简称DDL(Data Definition Language)，用来定义数据库对象：数据库，表，列 等。 |
| 数据操作语 言 | 简称DML(Data Manipulation Language)，用来对数据库中**表的记录进行更新。** |
| 数据查询语 言 | 简称DQL(Data Query Language)，用来**查询**数据库中表的记录。 |
| 数据控制语 言 | 简称DCL(Data Control Language)，用来定义数据库的访问权限和安全级别， 及创建用户。(了解) |

重点：DML 与 DQL！  





1） SQL语句可以单行 或者 多行书写，以**分号 结尾** ; （Sqlyog中可以不用写分号）

2） 可以使用空格和缩进来增加语句的可读性。

3） MySql中使用SQL**不区分大小写**，一般关键字大写，**数据库名 表名列名 小写。**

4） 注释方式  

| 注释语法 | 说明                |
| -------- | ------------------- |
| -- 空格  | 单行注释            |
| /* */    | 多行注释            |
| #        | MySql特有的单行注释 |









## DDL操作 数据库  





**创建数据库**  

| 命令                                            | 说明                                                     |
| ----------------------------------------------- | -------------------------------------------------------- |
| create database 数据库名；                      | 创建指定名称的数据库。                                   |
| create database 数据库名 character set 字符集； | 创建指定名称的数据库，并且指定字符集（一般都 指定utf-8） |

默认是utf8mb4编码

```mysql
/*
方式1 直接指定数据库名进行创建
默认数据库字符集为：latin1
*/
CREATE DATABASE db1;
/*
方式2 指定数据库名称，指定数据库的字符集
一般都指定为 utf8,与Java中的编码保持一致
*/
CREATE DATABASE db1_1 CHARACTER SET utf8;
```



**查看/选择数据库  **

| 命令                            | 说明                       |
| ------------------------------- | -------------------------- |
| use 数据库                      | 切换数据库                 |
| select database();              | 查看当前正在使用的数据库   |
| show databases;                 | 查看Mysql中 都有哪些数据库 |
| show create database 数据库名； | 查看一个数据库的定义信息   |





**修改数据库  **

| 命令                                           | 说明                   |
| ---------------------------------------------- | ---------------------- |
| alter database 数据库名 character set 字符集； | 数据库的字符集修改操作 |



```sql
-- 将数据库db1 的字符集 修改为 utf8
ALTER DATABASE db1 CHARACTER SET utf8;
-- 查看当前数据库的基本信息，发现编码已更改
SHOW CREATE DATABASE db1;
```



**删除数据库  **

| 命令                   | 说明                          |
| ---------------------- | ----------------------------- |
| drop database 数据库名 | 从MySql中永久的删除某个数据库 |

```mysql
-- 删除某个数据库
DROP DATABASE db1_1;
```







## DDL 操作 数据表  





常用的数据类型：  

| 类型    | 描述                                                |
| ------- | --------------------------------------------------- |
| int     | 整型                                                |
| double  | 浮点型                                              |
| varchar | 字符串型                                            |
| date    | 日期类型，给是为 yyyy-MM-dd ,只有年月日，没有时分秒 |



![image-20210331160501218](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331160501218.png)



注意：MySQL中的 char类型与 varchar类型，都对应了 Java中的字符串类型，区别在于：

char类型是**固定长度**的： 根据定义的字符串长度**分配足够的空间。**
varchar类型是**可变长度**的： **只使用字符串长度所需的空间**

比如：保存字符串 "abc"  

```
x char(10) 占用10个字节
y varchar(10) 占用3个字节
```



适用场景：

- char类型适合存储 固定长度的字符串，比如 密码 ，性别一类
- varchar类型适合存储 在一定范围内，有长度变化的字符串  



**varchar中的参数指的是数据的长度，而不是字节数，虽然汉字在utf8中是3个字节，但varchar(20)还是很可以存放20个汉字**

---

**创建表  **

```mysql
CREATE TABLE 表名(
    字段名称1 字段类型（长度），
    字段名称2 字段类型 -> 注意 最后一列不要加逗号
)；
```



```mysql
-- 创建表
CREATE TABLE category(
    cid INT,
    cname VARCHAR(20)
);

-- 创建测试表
CREATE TABLE test1(
    tid INT,
    tdate DATE
);

-- 创建一个表结构与 test1 相同的 test2表
CREATE TABLE test2 LIKE test1;
-- 查看表结构
DESC test2;
```







**查看表  **

| 命令         | 说明                       |
| ------------ | -------------------------- |
| show tables; | 查看当前数据库中的所有表名 |
| desc 表名；  | 查看数据表的结构           |



```mysql
-- 查看当前数据库中的所有表名
SHOW TABLES;
-- 显示当前数据表的结构
DESC category;
-- 查看创建表的SQL语句
SHOW CREATE TABLE category;


```





**删除表  **



| 命令                        | 说明                                               |
| --------------------------- | -------------------------------------------------- |
| drop table 表名；           | 删除表（从数据库中永久删除某一张表）               |
| drop table if exists 表名； | 判断表是否存在， 存在的话就删除,不存在就不执行删除 |



**修改表  **



```mysql
rename table 旧表名 to 新表名

alter table 表名 character set 字符集

alert table 表名 add 字段名称 字段类型
ALTER TABLE category ADD cdesc VARCHAR(20);

lter table 表名 modify 字段名称 字段类型
ALTER TABLE category MODIFY cdesc VARCHAR(50);

alter table 表名 change 旧列名 新列名 类型(长度);

alter table 表名 drop 列名;
```





## DML 操作表中数据  



SQL中的DML 用于对表中的数据进行增删改操作  



**插入数据  **



```
insert into 表名 （字段名1，字段名2...） values(字段值1，字段值2...);
```



按顺序插入全部字段，可以不写字段名  



1) 值与字段必须要**对应**，**个数相同&数据类型相同**

2）值的数据大小，必须在字段指定的长度范围内  

3）*varchar char date类型的值**必须使用单引号**，或者**双引号** 包裹。*

4）如果要插入空值，**可以忽略不写，或者插入null**

5)  如果插入**指定字段**的值，**必须要上写列名**  





----

**更改数据  **



```mysql
update 表名 set 列名 = 值

update 表名 set 列名 = 值 [where 条件表达式：字段名 = 值 ]

-- 一次修改多个属性的值
UPDATE student SET age = 20,address = '北京' WHERE sid = 2;
```







---



**删除数据 **



```
delete from 表名

delete from 表名 [where 字段名 = 值]
```



3) 如果要删除表中的所有数据,有两种做法

1. delete from 表名; 不推荐. **有多少条记录 就执行多少次删除操作**. 效率低
2. **truncate table** 表名: 推荐. 先删除整张表, 然后**再重新创建一张一模一样的表**. **效率高**  



## DQL 查询表中数据  





```
select 列名 from 表名
SELECT eid,ename FROM emp;
```



别名查询，使用关键字 as  

```mysql
# 使用 AS关键字,为列起别名
SELECT
    eid AS '编号',
    ename AS '姓名' ,
    sex AS '性别',
    salary AS '薪资',
    hire_date '入职时间', -- AS 可以省略
    dept_name '部门名称'
FROM emp;
```





使用去重关键字 `distinct`  

```mysql
-- 使用distinct 关键字,去掉重复部门信息
SELECT DISTINCT dept_name FROM emp;
```

对当前这条查询到的字段去重







运算查询 (查询结果参与运算)  





---



**条件查询**  



如果查询语句中**没有设置条件,就会查询所有的行信息**,在实际应用中,一定要指定查询条件,对记录进行过滤  



```
select 列名 from 表名 where 条件表达式

* 先取出表中的每条数据,满足条件的数据就返回,不满足的就过滤掉
```



| 运算符            | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| > < <= >= = <> != | 大于、小于、大于(小于)等于、不等于                           |
| BETWEEN ...AND... | 显示在某一区间的值 例如: 2000-10000之间： Between 2000 and 10000 |
| IN(集合)          | 集合表示多个值,使用逗号分隔,例如: name in (悟空，八戒) in中的每个数据都会作为一次条件,只要满足条件就会显示 |
| LIKE '%张%'       | 模糊查询                                                     |
| IS (Not)  NULL    | 查询某一列为NULL的值, 注: 不能写 = NULL                      |



| 运算符  | 说明             |
| ------- | ---------------- |
| And &&  | 多个条件同时成立 |
| Or \|\| | 多个条件任一成立 |
| Not     | 不成立，取反。   |









模糊查询 通配符  

| 通配符 | 说明                        |
| ------ | --------------------------- |
| %      | 表示匹配**任意多个**字符串, |
| _      | 表示匹配 **一个字符**       |







# 操作单表  



## 排序  







- **过 ORDER BY 子句,可以将查询出的结果进行排序(排序只是显示效果,不会影响真实数据)  **



```
SELECT 字段名 FROM 表名 [WHERE 字段 = 值] ORDER BY 字段名 [ASC / DESC]
```

| ASC 表示升序排序(默认) |
| ---------------------- |
| DESC 表示降序排序      |



### 排序方式  



1) 单列排序

只按照**某一个字段**进行排序, 就是单列排序  



使用 salary 字段,对emp 表数据进行排序 (升序/降序)  

```mysql
-- 默认升序排序 ASC
SELECT * FROM emp ORDER BY salary;

-- 降序排序
SELECT * FROM emp ORDER BY salary DESC;
```





2) 组合排序

同时对**多个字段**进行排序, 如果**第一个字段相同 就按照第二个字段**进行排序,以此类推  



在薪水排序的基础上,再使用id进行排序, 如果薪水相同就以id 做降序排序  

```mysql
-- 组合排序
SELECT * FROM emp ORDER BY salary DESC, eid DESC;
```





## 聚合函数  



之前我们做的查询都是横向查询，它们都是**根据条件一行一行的进行判断**，而使用聚合函数查询是纵向查询，它是对**某一列的值进行计算**，然后返回一个**单一的值**(另外聚合函数会***忽略null空值***。)；  



```
SELECT 聚合函数(字段名) FROM 表名;
```



| 聚合函数    | 作用                             |
| ----------- | -------------------------------- |
| count(字段) | 统计指定列**不为NULL的记录行数** |
| sum(字段)   | 计算指定列的数值和               |
| max(字段)   | 计算指定列的最大值               |
| min(字段)   | 计算指定列的最小值               |
| avg(字段)   | 计算指定列的平均值               |



```mysql
#1 查询员工的总数
-- 统计表中的记录条数 使用 count()
SELECT COUNT(eid) FROM emp; -- 使用某一个字段
SELECT COUNT(*) FROM emp; -- 使用 *
SELECT COUNT(1) FROM emp; -- 使用 1,与 * 效果一样


-- 下面这条SQL 得到的总条数不准确,因为count函数忽略了空值
-- 所以使用时注意不要使用带有null的列进行统计
SELECT COUNT(dept_name) FROM emp;
```

如果count中的字段为空，那么不会统计数量



```mysql
#2 查看员工总薪水、最高薪水、最小薪水、薪水的平均值

-- sum函数求和, max函数求最大, min函数求最小, avg函数求平均值
SELECT
SUM(salary) AS '总薪水',
MAX(salary) AS '最高薪水',
MIN(salary) AS '最低薪水',
AVG(salary) AS '平均薪水'
FROM emp;
```



```mysql
#3 查询薪水大于4000员工的个数
SELECT COUNT(*) FROM emp WHERE salary > 4000;
```



```mysql
#5 查询部门为'市场部'所有员工的平均薪水
SELECT
AVG(salary) AS '市场部平均薪资'
FROM emp
WHERE dept_name = '市场部';
```











## 分组  



**分组查询指的是使用 GROUP BY 语句,对查询的信息进行分组,相同数据作为一组  **



```
SELECT 分组字段/聚合函数 FROM 表名 GROUP BY 分组字段 [HAVING 条件];
```



通过性别字段 进行分组  

```mysql
-- 按照性别进行分组操作
SELECT * FROM emp GROUP BY sex; -- 注意 这样写没有意义
```



GROUP BY 分组过程  

![image-20210331184838208](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331184838208.png)



分组时可以查询**要分组的字段**, 或者使用聚合函数进行统计操作

**\* 查询其他字段没有意义**  

将要分组的字段和要求的聚合函数值一起返回。 

```mysql
# 通过性别字段 进行分组,求各组的平均薪资
SELECT sex, AVG(salary) FROM emp GROUP BY sex;
```

![image-20210331184949357](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331184949357.png)



**将  各行数据在   相同字段  有相同的值的分为一组。**

**对各组的某些数据进行聚合，返回统计结果**

```mysql
#1. 查询有几个部门
SELECT dept_name AS '部门名称' FROM emp GROUP BY dept_name;
SELECT DISTINCT dept_name AS '部门名称' FROM emp ;



#2.查询每个部门的平均薪资
SELECT
dept_name AS '部门名称',
AVG(salary) AS '平均薪资'
FROM emp GROUP BY dept_name;



#3.查询每个部门的平均薪资, 部门名称不能为null
SELECT
dept_name AS '部门名称',
AVG(salary) AS '平均薪资'
FROM emp WHERE dept_name IS NOT NULL GROUP BY dept_name;
```





- 查询平均薪资大于6000的部门.  

分析:

1) 需要在分组后,***对数据进行过滤***,使用 关键字 **having**

2) 分组操作中的having子语句，是用于在**分组后对数据进行过滤的**，作用**类似于where条件**。  



```mysql
# 查询平均薪资大于6000的部门

-- 需要在分组后再次进行过滤,使用 having
SELECT
dept_name ,
AVG(salary)
FROM emp WHERE dept_name IS NOT NULL GROUP BY dept_name HAVING AVG(salary) >
6000 ;
```



where 与 having的区别  

| 过滤方式 | 特点                                                         |
| -------- | ------------------------------------------------------------ |
| where    | where 进行**分组前的过滤** where **后面不能写 聚合函数**,  只能进行单纯的判断或者模糊查询 |
| having   | having 是分组后的过滤 ***having 后面可以写 聚合函数***       |







## limit关键字  



limit 关键字的作用

limit是限制的意思,用于 **限制返回的查询结果的行数** (可以通过limit**指定查询多少行数据**)

limit 语法是 MySql的方言,***用来完成分页***  

```
SELECT 字段1,字段2... FROM 表名 LIMIT offset , length;
```



| limit offset , length; 关键字可以接受一个 或者两个 为0 或者正整数的参数 |
| ------------------------------------------------------------ |
| offset **起始行数**, **从0开始记数**, 如果省略 则默认为 0.   |
| length 返回的行数                                            |



```mysql
# 查询emp表中的前 5条数据
-- 参数1 起始值,默认是0 , 参数2 要查询的条数
SELECT * FROM emp LIMIT 5;
SELECT * FROM emp LIMIT 0 , 5;

# 查询emp表中 从第4条开始,查询6条
-- 起始值默认是从0开始的.
SELECT * FROM emp LIMIT 3 , 6;
```



分页操作

```mysql
-- 分页操作 每页显示3条数据
SELECT * FROM emp LIMIT 0,3; -- 第1页
SELECT * FROM emp LIMIT 3,3; -- 第2页 2-1=1 1*3=3
SELECT * FROM emp LIMIT 6,3; -- 第三页

-- 分页公式 起始索引 = (当前页 - 1) * 每页条数
-- offset索引从0开始，而数据库条数是从1开始
-- limit是MySql中的方言
```







# SQL约束  



1) 约束的作用:

对表中的数据进行**进一步的限制**，从而**保证数据的正确性、有效性、完整性.**

违反约束的***不正确数据,将无法插入到表中***



2) 常见的约束  

| 约束名 | 约束关键字      |
| ------ | --------------- |
| 主键   | primary key     |
| 唯一   | unique          |
| 非空   | **not null**    |
| 外键   | **foreign key** |



## 主键约束  



| 特点 | 不可重复 唯一 非空                                       |
| ---- | -------------------------------------------------------- |
| 作用 | 用来**(唯一)表示**数据库中的    **每一条(每行数据)记录** |



```mysql
字段名 字段类型 primary key


# 方式1 创建一个带主键的表
CREATE TABLE emp2(
    -- 设置主键 唯一 非空
    eid INT PRIMARY KEY,
    ename VARCHAR(20),
    sex CHAR(1)
);
-- 删除表
DROP TABLE emp2;


-- 方式2 创建一个带主键的表
CREATE TABLE emp2(
    eid INT ,
    ename VARCHAR(20),
    sex CHAR(1),
    -- 指定主键为 eid字段
    PRIMARY KEY(eid)
);


-- 方式3 创建一个带主键的表
CREATE TABLE emp2(
    eid INT ,
    ename VARCHAR(20),
    sex CHAR(1)
) 
-- 创建的时候不指定主键,然后通过 DDL语句进行设置
ALTER TABLE emp2 ADD PRIMARY KEY(eid);
```





![image-20210331194709371](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331194709371.png)



3) 哪些字段可以作为主键 ?

通常针对业务去设计主键,**每张表都设计一个主键id**
**主键是给数据库和程序使用的,跟最终的客户无关,**所以主键**没有意义**没有关系,只要能够保证不重复就好,比如 **身份证就可以作为主键.**  







- 删除 表中的主键约束  

```mysql
-- 使用DDL语句 删除表中的主键
ALTER TABLE emp2 DROP PRIMARY KEY;
DESC emp2;
```





- 主键的自增

  注: 主键如果让我们自己添加很有可能重复,我们通常希望在**每次插入新记录时**,数据库***自动生成主键字段的值.***  



```mysql
AUTO_INCREMENT 表示自动增长(字段类型必须是整数类型)

-- 创建主键自增的表
CREATE TABLE emp2(
    -- 关键字 AUTO_INCREMENT,主键类型必须是整数类型
    eid INT PRIMARY KEY AUTO_INCREMENT,
    ename VARCHAR(20),
    sex CHAR(1)
);
```

**系统会保存上次记录到的值，而不是去找表中最大的自增id去+1**

创建主键自增的表,自定义自增其实值

```mysql
-- 创建主键自增的表,自定义自增其实值
CREATE TABLE emp2(
eid INT PRIMARY KEY AUTO_INCREMENT,
ename VARCHAR(20),
sex CHAR(1)
)AUTO_INCREMENT=100;
```









### DELETE和TRUNCATE对自增长的影响  



| 清空表数据的方式 | 特点                                                         |
| ---------------- | ------------------------------------------------------------ |
| DELETE           | 只是删除表中所有数据,对自增没有影响，**原表中记录自增的数据并没有重置** |
| TRUNCATE         | truncate 是将整个表删除掉,然后**创建一个新的表** 自增的主键,重新从 1开始 |









## 非空约束  



非空约束的特点: 某一列不予许为空  



```mysql
字段名 字段类型 not null  

# 非空约束
CREATE TABLE emp2(
    eid INT PRIMARY KEY AUTO_INCREMENT,
    -- 添加非空约束, ename字段不能为空
    ename VARCHAR(20) NOT NULL,
    sex CHAR(1)
);
-- Column 'ename' cannot be null
```



## 唯一约束  





唯一约束的特点: 表中的某一列的值不能重复( ***对null不做唯一的判断*** )  

```mysql
#创建emp3表 为ename 字段添加唯一约束
CREATE TABLE emp3(
    eid INT PRIMARY KEY AUTO_INCREMENT,
    ename VARCHAR(20) UNIQUE,
    sex CHAR(1)
);
-- Duplicate entry 'aa' for key 'ename'
```



| 主键约束与唯一约束的区别:                                  |
| ---------------------------------------------------------- |
| 1. 主键约束 唯一且不能够为空                               |
| 2. 唯一约束,唯一 **但是可以为空**                          |
| 3. 一个表中只能有**一个主键** , 但是可以有**多个唯一约束** |

**唯一约束可以为空**

![image-20210331200423759](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331200423759.png)





## 外键约束  



**FOREIGN KEY** 表示**外键约束**，将在多表中涉及







## 默认值  



默认值约束 用来指定某列的默认值  



```mysql
-- 创建带有默认值的表
CREATE TABLE emp4(
    eid INT PRIMARY KEY AUTO_INCREMENT,
    -- 为ename 字段添加默认值
    ename VARCHAR(20),
    sex CHAR(1) DEFAULT '女'
);

INSERT INTO emp4(ename,sex) VALUES("aaa",DEFAULT);
INSERT INTO emp4(sex) VALUES(DEFAULT);
INSERT INTO emp4(ename) VALUES("aaa"); -- 或者直接不指定
```













# 数据库事务  





## 什么是事务  



**事务是一个整体**,由**一条或者多条SQL 语句**组成,这些SQL语句要么  ***都***  执行成功,要么都执行失败, 只要有一条SQL出现异常,整个操作就会回滚,整个业务执行失败  

```
比如: 银行的转账业务,张三给李四转账500元 , 至少要操作两次数据库, 张三 -500, 李四 + 500,这中
间任何一步出现问题,整个操作就必须全部回滚, 这样才能保证用户和银行都没有损失
```





- 回滚

  即在事务运行的过程中发生了**某种故障**，事务**不能继续执行**，系统将事务中**对数据库的所有已完成的操作全部撤销**，**滚回到事务开始时的状态**。（在提交之前执行）  







## 模拟转账操作  



```mysql
-- 创建账户表
CREATE TABLE account(
    -- 主键
    id INT PRIMARY KEY AUTO_INCREMENT,
    -- 姓名
    NAME VARCHAR(10),
    -- 余额
    money DOUBLE
);


-- 添加两个用户
INSERT INTO account (NAME, money) VALUES ('tom', 1000), ('jack', 1000);
```



模拟tom 给 jack 转 500 元钱，一个转账的业务操作最少要执行下面的 2 条语句  

```mysql
-- tom账户 -500元
UPDATE account SET money = money - 500 WHERE NAME = 'tom';
-- jack账户 + 500元
UPDATE account SET money = money + 500 WHERE NAME = 'jack';
```



> 假设当tom 账号上 -500 元,服务器崩溃了。jack 的账号并没有+500 元，数据就出现问题了。
>
> 我们要**保证整个事务执行的完整性,要么都成功, 要么都失败**. 这个时候我们就要学习如何操作事务.  





## MySql事务操作  



MYSQL 中可以有两种方式进行事务的操作：

- 手动提交事务
- 自动提交事务  



### 手动提交事务  



**语法格式  **

| 功能     | 语句                           |
| -------- | ------------------------------ |
| 开启事务 | start transaction; 或者 BEGIN; |
| 提交事务 | commit;                        |
| 回滚事务 | rollback;                      |



- START TRANSACTION
  - 这个语句显式地标记一个**事务的起始点。**
- COMMIT
  - 表示**提交事务**，即提交事务的所有操作，具体地说，就是将事务中所有对数据库的更新都写到磁盘上的物理数据库中，事务正常结束。
- ROLLBACK
  - 表示撤销事务，即在事务运行的过程中发生了某种故障，事务不能继续执行，系统将事务中对数据库的所有已完成的操**作全部撤销**，**回滚到事务开始时的状态**  



---

**手动提交事务流程  **

- 执行成功的情况： 开启事务 -> 执行多条 SQL 语句 -> 成功提交事务
- 执行失败的情况： 开启事务 -> 执行多条 SQL 语句 -> 事务的回滚  



不去提交事务 **直接关闭窗口**,**发生回滚操作**,数据没有改变  













----

- **自动提交事务  **



MySQL 默认**每一条 DML(增删改)语句都是一个单独的事务**，***每条语句都会自动开启一个事务***，语句执行完毕 **自动提交事务**，MySQL 默认开始自动提交事务

MySQL默认是自动提交事务  



**取消自动提交  **

MySQL默认是自动提交事务,设置为手动提交.  

![image-20210331204916466](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331204916466.png)

| on ：自动提交  |
| -------------- |
| off : 手动提交 |

```
SET @@autocommit=off;
```





## 事务的四大特性 ACID  



| 特性   | 含义                                                         |
| ------ | :----------------------------------------------------------- |
| 原子性 | 每个事务都是一个整体，**不可再拆分**，事务中所有的 SQL 语句要么都执行成功， 要么都 失败。 |
| 一致性 | 事务在执行前数据库的状态与执行后数据库的**状态保持一致**。如：转账前2个人的 总金额 是 2000，转账后 2 个人总金额也是 2000. |
| 隔离性 | **事务与事务之间不应该相互影响，执行时保持隔离的状态.**         事务是并发控制机制，他们交错使用时也能提供一致性。隔离让我们隐藏来自外部世界未提交的状态变化，一个失败的事务不应该破坏系统的状态。隔离是通过用悲观或乐观锁机制实现的。 |
| 持久性 | 一旦事务执行成功，对数据库的**修改是持久的**。就算关机，数据也是要保存下来的 |







## Mysql 事务隔离级别  





### 数据并发访问  

一个数据库可能拥有**多个访问客户端**,这些客户端都可以**并发方式访问数据库**. 数据库的相同数据可能被**多个事务同时访问**,如果**不采取隔离措施,就会导致各种问题, 破坏数据的完整性**  





### 并发访问会产生的问题  



事务在操作时的理想状态： **所有的事务之间保持隔离，互不影响**。因为并发操作，多个用户同时访问同一个 数据。可能引发并发访问的问题  



| 并发访问的问题 | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| 脏读           | 一个事务读取到了另一个事务中**尚未提交的数据**               |
| 不可重复 读    | 一个事务中***两次读取的数据内容不一致***  , **要求的是在一个事务中多次读取时数据是一 致的**. 这是进行 **update 操作**时引发的问题 |
| 幻读           | 一个事务中,某一次的 select 操作**得到的结果所表征的数据状态**, **无法支撑后续的业务 操作（在这期间数据库该条数据的状态进行了改变，并发访问导致的问题）**. 查询得到的数据状态不准确,导致幻读 |





### 四种隔离级别  

通过设置隔离级别,可以防止上面的三种并发问题.
MySQL数据库有四种隔离级别 上面的级别最低，下面的级别最高。

✔ 会出现问题
✘ 不会出现问题  



| 级 别 | 名字      | 隔离级别         | 脏 读 | 不可重复 读 | 幻 读 | 数据库的默认隔离级 别 |
| ----- | --------- | ---------------- | ----- | ----------- | ----- | --------------------- |
| 1     | 读未提 交 | read uncommitted | ✔     | ✔           | ✔     |                       |
| 2     | 读已提 交 | read committed   | ✘     | ✔           | ✔     | Oracle和SQLServer     |
| 3     | 可重复 读 | repeatable read  | ✘     | ✘           | ✔     | MySql                 |
| 4     | 串行化    | serializable     | ✘     | ✘           | ✘     |                       |





### 隔离级别相关命令  



查看隔离级别  

```
select @@tx_isolation;
SELECT @@transaction_isolation;  新版
```

mysql默认：可重复读，可以   防止脏读和不可重复读

![image-20210331210756380](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331210756380.png)



设置事务隔离级别，需要退出 MySQL 再重新登录才能看到隔离级别的变化  

```
set global transaction isolation level 级别名称;
read uncommitted 读未提交
read committed 读已提交
repeatable read 可重复读
serializable 串行化

set global transaction isolation level read uncommitted;
```





## 隔离性问题演示  



###  脏读演示  

脏读: 一个事务读取到了另一个事务中尚未提交的数据  



![image-20210331211719186](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331211719186.png)



A进行回滚：

![image-20210331211901701](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331211901701.png)

B再次查询，发现数据又变回去了。——**脏读！！！！**



### 解决脏读问题  



脏读非常危险的，比如张三向李四购买商品，张三开启事务，向李四账号转入 500 块，然后打电话给李四说钱 已经转了。李四一查询钱到账了，发货给张三。张三收到货后回滚事务，李四的再查看钱没了  。。。。。



将**全局的隔离级别进行提升为: read committed**  



```mysql
set global transaction isolation level read committed;
SELECT @@transaction_isolation;
```

![image-20210331212152870](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331212152870.png)



演示略~~





### 不可重复读演示  



不可重复读: 同一个事务中,进行查询操作,但是每次读取的数据内容是不一样的  



这时候就不会出现脏读

![image-20210331212544628](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331212544628.png)





此时对A进行提交？B读到的数据还是发生了改变！

![image-20210331212621969](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331212621969.png)



两次查询输出的结果不同，到底哪次是对的？  



**不知道以哪次为准**。 很多人认为这种情况就对了，无须困惑， **当然是后面的为准？？×**

我们可以考虑这样一种情况:  

> 比如银行程序需要将**查询结果**分别输出到电脑屏幕和发短信给客户，结果在一个事务中针对不同的输出目的地进行的两次查询不一致，导致文件和屏幕中的结果不一致，银行工作 人员就不知道以哪个为准了  







### 解决不可重复读问题  

将全局的隔离级别进行提升为： repeatable read  

```
-- 设置事务隔离级别为 repeatable read
set global transaction isolation level repeatable read;
```



演示略~~









### 幻读演示  



幻读: select 某记录是否存在，不存在，**准备插入此记录，但执行 insert 时发现此记录已存在**，
**无法插入**，此时就发生了幻读。   

**虽然解决了不可重复读的问题，在当前事务中：本地查询到没有插入这条数据，这不会改变。但其他的事务可能已经插入了这条数据，但本事务并不知道——因为设置了可重复读的隔离级别。在插入时会发现无法插入！**





![image-20210331213756969](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331213756969.png)

明明刚才读的时候没有？为什么会重复呢？？？



### 解决幻读问题  



将事务隔离级别设置到最高 **SERIALIZABLE** ，以挡住幻读的发生  



如果一个事务，使用了SERIALIZABLE——**可串行化隔离级别**时，**在这个事务没有被提交之前 , 其他的线程，只能等到当前操作完成之后**，**才能进行操作**，这样会**非常耗时**，而且，影响数据库的性能，数据库不会使用这种隔离级别  

```
set global transaction isolation level SERIALIZABLE;
```



——>在一个事务未提交时，另一个事务无法执行操作，会被停滞，**直到上一个事务提交后，才能进行操作**







串行化就相当于给操作的记录上一个**共享锁（读写锁）**，即当读某条记录时就占用这条记录的读锁，此时其它事务一样可以申请到这条记录的读锁来读取，**但是不能写**（**读锁被占的话，写锁就不能被占**；**读锁可以被多个事务同时占有**）



串行化相当于加 **共享锁**，别的事务只是不能 update insert delete， 但还是可以读的





![image-20210331214738021](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210331214738021.png)







# 多表



实际开发中，一个项目通常需要很多张表才能完成。

例如一个商城项目的数据库,需要有很多张表：用户表、分类表、商品表、订单表....  



## 单表的缺点  





```mysql
-- 创建emp表 主键自增
CREATE TABLE emp(
eid INT PRIMARY KEY AUTO_INCREMENT,
ename VARCHAR(20),
age INT ,
dep_name VARCHAR(20),
dep_location VARCHAR(20)
);
```



```
-- 添加数据
INSERT INTO emp (ename, age, dep_name, dep_location) VALUES ('张百万', 20, '研发部', '广州');
INSERT INTO emp (ename, age, dep_name, dep_location) VALUES ('赵四', 21, '研发部', '广州');
INSERT INTO emp (ename, age, dep_name, dep_location) VALUES ('广坤', 20, '研发部', '广州');
INSERT INTO emp (ename, age, dep_name, dep_location) VALUES ('小斌', 20, '销售部', '深圳');
INSERT INTO emp (ename, age, dep_name, dep_location) VALUES ('艳秋', 22, '销售部', '深圳');
INSERT INTO emp (ename, age, dep_name, dep_location) VALUES ('大玲子', 18, '销售部', '深圳');
```

1) 冗余, 同一个字段中出现大量的重复数据  

![image-20210401083520469](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401083520469.png)





## 解决方案  



设计为两张表  

\1. 多表方式设计
department 部门表 : id, dep_name, dep_location
employee 员工表: eid, ename, age, dep_id  



```mysql
-- 创建部门表
-- 一方,主表
CREATE TABLE department(
    id INT PRIMARY KEY AUTO_INCREMENT,
    dep_name VARCHAR(30),
    dep_location VARCHAR(30)
);
-- 创建员工表
-- 多方 ,从表
CREATE TABLE employee(
    eid INT PRIMARY KEY AUTO_INCREMENT,
    ename VARCHAR(20),
    age INT,
    dept_id INT
);
```



```mysql
-- 添加2个部门
INSERT INTO department VALUES(NULL, '研发部','广州'),(NULL, '销售部', '深圳');
SELECT * FROM department;

-- 添加员工,dep_id表示员工所在的部门
INSERT INTO employee (ename, age, dept_id) VALUES ('张百万', 20, 1);
INSERT INTO employee (ename, age, dept_id) VALUES ('赵四', 21, 1);
INSERT INTO employee (ename, age, dept_id) VALUES ('广坤', 20, 1);
INSERT INTO employee (ename, age, dept_id) VALUES ('小斌', 20, 2);
INSERT INTO employee (ename, age, dept_id) VALUES ('艳秋', 22, 2);
INSERT INTO employee (ename, age, dept_id) VALUES ('大玲子', 18, 2);
SELECT * FROM employee;
```



1) 员工表中有一个字段dept_id 与**部门表中的主键对应**,员工表的这个字段就叫做 **外键**
2) 拥有外键的员工表 被称为 **从表** , 与**外键对应的主键**所在的表叫做 **主表**  

![image-20210401083907909](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401083907909.png)





**多表设计上的问题**  



当我们在 员工表的 dept_id 里面输入不存在的部门id ,数据依然可以添加 显然这是不合理的  



```
-- 插入一条 不存在部门的数据3
INSERT INTO employee (ename,age,dept_id) VALUES('无名',35,3);
```

实际上我们应该保证,员工表所添加的 dept_id , 必须在部门表中存在  

![image-20210401084108235](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401084108235.png)



**使用外键约束,约束 dept_id ,   必须是 部门表中存在的id  **





## 外键约束  



- 外键指的是在 **从表** 中 与 **主表 的主键**对应的那个字段,比如员工表的 dept_id,就是外键

- 使用外键约束可以让两张表之间**产生一个对应关系**,从而**保证主从表的引用的完整性**  



多表关系中的主表和从表

- 主表: 主键id所在的表, **约束别人的表**
- 从表: 外键所在的表, **被约束的表**  



![image-20210401084255612](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401084255612.png)



---

- **创建外键约束  **



新建表时添加外键  

```
[CONSTRAINT] [外键约束名称] FOREIGN KEY(外键字段名) REFERENCES 主表名(主键字段名)
```

已有表添加外键  

```
ALTER TABLE 从表 ADD [CONSTRAINT] [外键约束名称] FOREIGN KEY (外键字段名) REFERENCES 主表(主 键字段名);
```



```mysql
-- 重新创建 employee表,添加外键约束
CREATE TABLE employee(
    eid INT PRIMARY KEY AUTO_INCREMENT,
    ename VARCHAR(20),
    age INT,
    dept_id INT,
    -- 添加外键约束
    CONSTRAINT emp_dept_fk FOREIGN KEY(dept_id) REFERENCES department(id)
);
```



![image-20210401084849160](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401084849160.png)

添加外键约束,就会产生***强制性的外键数据检查, 从而保证了数据的完整性和一致性***  



这两个表的字段就**关联在了一起**

![image-20210401085130818](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401085130818.png)





---

- **删除外键约束  **



```mysql
alter table 从表 drop foreign key 外键约束名称

-- 删除employee 表中的外键约束,外键约束名 emp_dept_fk
ALTER TABLE employee DROP FOREIGN KEY emp_dept_fk;
```



再将外键 添加回来  

```
-- 可以省略外键名称, 系统会自动生成一个
ALTER TABLE employee ADD FOREIGN KEY (dept_id) REFERENCES department (id);
```







----

- **外键约束的注意事项  **



1. 从表外键类型必须与主表主键**类型一致** 否则创建失败  

2. 添加数据时, 应该**先添加主表中的数据.**  

3. 删除数据时,应该**先删除从表**中的数据.  **在从表中有对这个外键数据的引用，还不能删除。需要先删除从表中对外键的引用。**



```mysql
-- 删除数据时 应该先删除从表中的数据
-- 报错 Cannot delete or update a parent row: a foreign key constraint fails
-- 报错原因 不能删除主表的这条数据,因为在从表中有对这条数据的引用
DELETE FROM department WHERE id = 3;


-- 先删除从表的关联数据
DELETE FROM employee WHERE dept_id = 3;
-- 再删除主表的数据
DELETE FROM department WHERE id = 3;
```









### 级联删除操作  





如果想实现**删除主表数据的同时,也删除掉从表数据,**可以使用级联删除操作  



```
创建表示添加  级联删除
ON DELETE CASCADE
```



```mysql
-- 重新创建添加级联操作
CREATE TABLE employee(
eid INT PRIMARY KEY AUTO_INCREMENT,
ename VARCHAR(20),
age INT,
dept_id INT,
CONSTRAINT emp_dept_fk FOREIGN KEY(dept_id) REFERENCES department(id)
-- 添加级联删除
ON DELETE CASCADE
);
-- 添加数据
INSERT INTO employee (ename, age, dept_id) VALUES ('张百万', 20, 1);
INSERT INTO employee (ename, age, dept_id) VALUES ('赵四', 21, 1);
INSERT INTO employee (ename, age, dept_id) VALUES ('广坤', 20, 1);
INSERT INTO employee (ename, age, dept_id) VALUES ('小斌', 20, 2);
INSERT INTO employee (ename, age, dept_id) VALUES ('艳秋', 22, 2);
INSERT INTO employee (ename, age, dept_id) VALUES ('大玲子', 18, 2);
```





# 多表关系设计  





实际开发中，**一个项目通常需要很多张表才能完成**。例如：一个商城项目就需要分类表(category)、商品表(products)、订单表(orders)等多张表。且这些表的数据之间存在一定的关系



| 表与表之间的三种关系                                |
| --------------------------------------------------- |
| 一对多关系: 最常见的关系, 学生对班级,员工对部门     |
| 多对多关系: 学生与课程, 用户与角色                  |
| 一对一关系: 使用较少,因为一对一关系可以合成为一张表 |









## 一对多关系(常见)  



一对多关系（1:n）

- 例如：班级(主键id)和学生（外键->班级id），部门和员工，客户和订单，分类和商品

一对多建表原则

- 在从表(多方)创建一个字段,**字段作为外键指向主表(一方)的主键**  



**“多”指向“一”的那一方的主键**

![image-20210401090640446](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401090640446.png)





## 多对多关系(常见)  



多对多（m:n）

- 例如：老师和学生，学生和课程，用户和角色

多对多关系建表原则

- 需要创建**第三张表**，中间表中**至少两个字段**，这两个字段**分别作为外键指向各自一方的 主键。**  

**中间表来联系两个表，**

**不然每个学生的信息需要对应多个课程表信息，学生的信息就要有多行，每行对应一个不同的课程。会产生冗余信息**

![image-20210401092352371](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401092352371.png)



## 一对一关系  



一对一（1:1）

- 在实际的开发中应用不多.因为一对一**可以创建成一张表**。（不会有冗余，**都是一一对应的**）

一对一建表原则

- 外键唯一 主表的主键和从表的外键（唯一），形成主外键关系，外键唯一 UNIQUE  

**任意一方添加外键都可以。**

![image-20210401093755337](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401093755337.png)



## 设计 省&市表  





省和市之间的关系是 一对多关系,一个省包含多个市  



```mysql
#创建省表 (主表,注意: 一定要添加  主键约束)
CREATE TABLE province(
	id INT PRIMARY KEY auto_increment,
	name VARCHAR(20),
	description VARCHAR(20)
);

#创建市表 (从表,注意: 外键类型一定要与主表主键一致)
CREATE TABLE city(
	cid int PRIMARY KEY auto_increment,
	name VARCHAR(20),
	description VARCHAR(20),
	pid INT,
	-- 添加外键约束
	CONSTRAINT pro_city_fk FOREIGN KEY (pid) REFERENCES province(id)
)
```



![image-20210401095111058](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401095111058.png)





## 设计 演员与角色表  





演员与角色 是多对多关系, 一个演员可以饰演多个角色, 一个角色同样可以被不同的演员扮演  



![image-20210401095207482](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401095207482.png)





```mysql
CREATE TABLE actor(
	id INT PRIMARY KEY auto_increment,
	name VARCHAR(20)
);

CREATE TABLE role(
	id INT PRIMARY KEY auto_increment,
	name VARCHAR(20)
);

CREATE TABLE actor_role(
	id INT PRIMARY KEY auto_increment,
	
	aid INT,
	rid INT,
	FOREIGN KEY (aid) REFERENCES actor(id),
	FOREIGN KEY (rid) REFERENCES role(id)
);
```

也可以手动添加外键约束：

```mysql
-- 为中间表的aid字段,添加外键约束 指向演员表的主键
ALTER TABLE actor_role ADD FOREIGN KEY(aid) REFERENCES actor(id);
-- 为中间表的rid字段, 添加外键约束 指向角色表的主键
ALTER TABLE actor_role ADD FOREIGN KEY(rid) REFERENCES role(id);
```



![image-20210401095647866](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401095647866.png)









# 多表查询  



DQL: 查询多张表,获取到需要的数据

比如 我们要查询   **家电分类下 都有哪些商品**,那么我们就需要查询**分类与商品这两张表**  



创建分类表与商品表  

```mysql
#分类表 (一方 主表)
CREATE TABLE category (
    cid VARCHAR(32) PRIMARY KEY ,
    cname VARCHAR(50)
);
#商品表 (多方 从表)
CREATE TABLE products(
    pid VARCHAR(32) PRIMARY KEY ,
    pname VARCHAR(50),
    price INT,
    flag VARCHAR(2), #是否上架标记为：1表示上架、0表示下架
    category_id VARCHAR(32),
    -- 添加外键约束
    FOREIGN KEY (category_id) REFERENCES category (cid)
);
```



插入数据  



```mysql
#分类数据
INSERT INTO category(cid,cname) VALUES('c001','家电');
INSERT INTO category(cid,cname) VALUES('c002','鞋服');
INSERT INTO category(cid,cname) VALUES('c003','化妆品');
INSERT INTO category(cid,cname) VALUES('c004','汽车');

#商品数据
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p001','小米电视机',5000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p002','格力空调',3000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p003','美的冰箱',4500,'1','c001');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p004','篮球鞋',800,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p005','运动裤',200,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p006','T恤',300,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p007','冲锋衣',2000,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p008','神仙水',800,'1','c003');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p009','大宝',200,'1','c003');
```







## 笛卡尔积  



交叉连接查询,因为会产生笛卡尔积,所以 基本不会使用  

**会产生所有组合**

```
SELECT 字段名 FROM 表1, 表2;
```



使用交叉连接查询 商品表与分类表  

```
SELECT * FROM category , products;
```



观察查询结果,产生了笛卡尔积 (得到的结果是无法使用的)  

![image-20210401100409470](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401100409470.png)

表A中的每一行都和表B中的每一行进行组合，**没有条件限制，全部组合一遍 A.length * B.length个结果，结果无用**

![image-20210401100445499](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401100445499.png)



---

笛卡尔积

假设集合A={a, b}，集合B={0, 1, 2}，则两个集合的笛卡尔积为{(a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b, 2)}  

![image-20210401100535315](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401100535315.png)







## 多表查询的分类  





### 内连接查询  



内连接的特点:

通过**指定的条件**去匹配两张表中的数据, **匹配上就显示,匹配不上就不显示**

- 比如通过: **从表的外键 = 主表的主键** 方式去匹配  



---

1. 隐式内连接

from子句 后面直接写 **多个表名** 使用**where指定连接条件的** 这种连接方式是 **隐式内连接.**

使用where条件过滤无用的数据  



```
SELECT 字段名 FROM 左表, 右表 WHERE 连接条件;
```



查询所有商品信息和**对应的分类**信息  

```mysql
SELECT * from products p, category c where p.category_id=c.cid
```



![image-20210401100852382](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401100852382.png)



查询商品表的商品名称 和 价格,以及商品的分类信息  

**可以通过给表起别名的方式, 方便我们的查询(有提示)**  



```mysql
SELECT 
p.pname,p.price, c.cname  
from products p, category c 
where p.category_id=c.cid
```



查询 格力空调是属于哪一分类下的商品  

```mysql
SELECT p.pname, c.cname  from products p, category c 
where p.pname='格力空调' AND p.category_id=c.cid
```

![image-20210401101426092](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401101426092.png)







---



2. **显式内连接  **



使用 `inner join ...on` 这种方式, 就是**显式内连接**

语法格式  

```mysql
SELECT 字段名 FROM 左表 [INNER] JOIN 右表 ON 条件
-- inner 可以省略
```



查询所有商品信息和对应的分类信息  :

```mysql
SELECT * from category c INNER JOIN products p ON c.cid = p.category_id
```



查询鞋服分类下,价格大于500的商品名称和价格  :

**先要将外键和主键连接 ——> 来拼接这两张表，再去设置查询条件**

```mysql
# 查询鞋服分类下,价格大于500的商品名称和价格
-- 我们需要确定的几件事
-- 1.查询几张表 products & category

-- 2.表的连接条件 从表.外键 = 主表的主键
-- 3.查询的条件 cname = '鞋服' and price > 500
-- 4.要查询的字段 pname price

SELECT
	p.pname,
	p.price 
FROM
	category c
	INNER JOIN products p ON c.cname = '鞋服' 
	AND p.price > 500 
	AND c.cid = p.category_id
```



**on和where没啥区别。。。**







### 外连接查询  



#### 左外连接  



左外连接 , 使用 **LEFT OUTER JOIN , OUTER 可以省略**

左外连接的特点: 

- 以**左表为基准**, 匹配右边表中的数据,如果匹配的上,就展示匹配到的数据
- 如果匹配不到, 左表中的数据正常展示, **右边的展示为null.**  



```mysql
-- 左外连接查询
SELECT * FROM category c LEFT JOIN products p ON c.`cid`= p.`category_id`;
```



![image-20210401103328908](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401103328908.png)

反过来：

![image-20210401103404673](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401103404673.png)



查询每个分类下的商品个数  



```mysql
# 查询每个分类下的商品个数
/*
1.连接条件: 主表.主键 = 从表.外键
2.查询条件: 每个分类 需要分组
3.要查询的字段: 分类名称, 分类下商品个数
*/

SELECT
	c.`cname` AS '分类名称',
	COUNT( p.`pid` ) AS '商品个数' -- 每个分组中的商品的id作为count计数
FROM
	category c
	LEFT JOIN products p ON c.cid = p.category_id 
GROUP BY
	c.cname
```







#### 右外连接  

右外连接 , 使用 RIGHT OUTER JOIN , OUTER 可以省略

右外连接的特点

- 以右表为基准，匹配左边表中的数据，如果能匹配到，展示匹配到的数据
- 如果匹配不到，右表中的数据正常展示, 左边展示为null  



```
-- 右外连接查询
SELECT * FROM products p RIGHT JOIN category c ON p.`category_id` = c.`cid`;
```







### 连接方式总结



![image-20210401105135182](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401105135182.png)



- 内连接: inner join , 只获取两张表中 **交集部分**的数据.
- 左外连接: left join , 以左表为基准 ,查询**左表的所有数据**, 以及**与右表有交集的部分**
- 右外连接: right join , 以右表为基准,查询右表的所有的数据,以及与左表有交集的部分  





# 子查询 (SubQuery)  





## 什么是子查询  



子查询概念

- 一条select 查询语句的结果, 作为另一条 select 语句的一部分

子查询的特点

- 子查询必须放在小括号中
- 子查询一般作为父查询的查询条件使用

子查询常见分类

- where型 子查询: 将**子查询的结果——>比如聚合结果**, 作为父查询的**比较条件**
- from型 子查询 : 将子查询的结果, 作为 **一张表**,提供给父层查询使用
- exists型 子查询: 子查询的结果是**单列多行**, 类似一个**数组**, 父层查询使用 **IN 函数** ,包含子查询的结果  





## 子查询的结果作为查询条件  



```
SELECT 查询字段 FROM 表 WHERE 字段=（子查询）;
```



通过子查询的方式, 查询价格最高的商品信息  



```mysql
# 通过子查询的方式, 查询价格最高的商品信息
-- 1.先查询出最高价格
SELECT MAX(price) FROM products;

-- 2.将最高价格作为条件,获取商品信息
select * from products 
where price = 
(SELECT max(price) from products)
```







查询化妆品分类下的 商品名称 商品价格  



```mysql
#查询化妆品分类下的 商品名称 商品价格

-- 先查出化妆品分类的 id
SELECT cid FROM category WHERE cname = '化妆品';

-- 根据分类id ,去商品表中查询对应的商品信息
SELECT
    p.`pname`,
    p.`price`
FROM products p
WHERE p.`category_id` = (SELECT cid FROM category WHERE cname = '化妆品');
```

**也可以不使用子查询，但需要在from加载两个表，对这两个表进行内连接，条件为  `c.cname='化妆品' and c.cid=p.category_id...`**



查询小于平均价格的商品信息  

```mysql
SELECT
	* 
FROM
	products 
WHERE
	price < ( SELECT avg( price ) FROM products );
```







## 子查询的结果作为一张表  



```
SELECT 查询字段 FROM （子查询）表别名 WHERE 条件;  
```



查询商品中,价格大于500的商品信息,包括 商品名称 商品价格 商品所属分类名称  



```mysql
-- 1. 先查询分类表的数据
SELECT * FROM category;

-- 2.将上面的查询语句 作为一张表使用
SELECT
	p.`pname`,
	p.`price`,
	c.cname
	-- 子查询作为一张表使用时 要起别名 才能访问表中字段
	-- 其实直接使用category就完事了。。。
FROM products p INNER JOIN (SELECT * FROM category) c 
ON p.category_id = c.cid 
WHERE p.price>500
```

注意： 当子查询作为一张表的时候，**需要起别名，否则无法访问表中的字段**  





## 子查询结果是单列多行  



子查询的结果类似一个数组, 父层查询使用 IN 函数 ,包含子查询的结果  

```
SELECT 查询字段 FROM 表 WHERE 字段 IN （子查询）;
```



查询价格小于两千的商品 ,   来自于哪些分类(名称)  



```mysql
-- 先查询价格小于2000 的商品的,分类ID
SELECT DISTINCT category_id FROM products WHERE price < 2000;

-- 在根据分类的id信息,查询分类名称
-- 报错: Subquery returns more than 1 row
-- 子查询的结果 大于一行 不能使用where条件=来比较
SELECT * FROM category
WHERE cid = (SELECT DISTINCT category_id FROM products WHERE price < 2000);

-- 使用in函数
-- 子查询获取的是单列多行数据
SELECT * FROM category
WHERE cid 
IN (SELECT DISTINCT category_id FROM products WHERE price < 2000);
```





查询家电类 与 鞋服类下面的全部商品信息  

```mysql
# 查询家电类 与 鞋服类下面的全部商品信息
-- 先查询出家电与鞋服类的 分类ID
SELECT cid FROM category WHERE cname IN ('家电','鞋服');

-- 根据cid 查询分类下的商品信息
SELECT * FROM products
WHERE category_id IN (SELECT cid FROM category WHERE cname IN ('家电','鞋服'));
```





1. 子查询如果查出的是**一个字段**(单列), 那就在where后面作为条件使用.IN
2. 子查询如果查询出的是多个字段(多列), 就当做**一张表**使用(**要起别名**)  





# 数据库设计  





## 数据库三范式(空间最省)  



概念: 三范式就是**设计数据库的规则**.

- 为了建立**冗余较小、结构合理**的数据库，设计数据库时必须遵循一定的规则。在关系型数据库中这种规则就称为**范式**。范式是**符合某一种设计要求**的总结。要想设计一个结构合理的关系型数据库，必须满足一定的范式
- 满足最低要求的范式是第一范式（1NF）。在第一范式的基础上进一步满足更多规范要求的称为第二范式（2NF） ， 其余范式以此类推。一般说来，数据库只需满足第三范式(3NF）就行了  





### 第一范式 1NF  



概念:

- 原子性, 做到列不可拆分
- 第一范式是最基本的范式。数据库表里面**字段都是单一属性的，不可再分**, 如果数据表中每个字段都是**不可再分的最小数据单元**，则满足第一范式。  



地址信息表中, contry这一列,**还可以继续拆分**,不符合第一范式  

**每列存储的信息可以    拆分出来  另一个属性表示的另一个字段**

![image-20210401122442716](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401122442716.png)





### 第二范式 2NF  



概念:

- 在第一范式的基础上更进一步，目标是**确保表中的每列都和主键相关**。
- **一张表只能描述一件事**.

示例:

- 学员信息表中其实在描述两个事物 , **一个是学员的信息,一个是课程信息**
- 如果放在一张表中,会导致**数据的冗余**,如果删除学员信息, **成绩的信息也被删除——>耦合**  
- 如果删除课程信息，学院的信息也会被删除



![image-20210401123126337](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401123126337.png)



### 第三范式 3NF  



概念:

- 消除**传递依赖**
- 表的信息，如果能够被推导出来，就**不应该单独的设计一个字段来存放**

示例

- 通过number 与 price字段就可以计算出总金额,不要在表中再做记录(空间最省)  



![image-20210401123652757](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401123652757.png)







## 数据库反三范式  



反范式化指的是通过**增加冗余或重复的数据**来提高数据库的读性能

**浪费存储空间,节省查询时间** (以***空间换时间***  )  





**冗余字段：**

- 设计数据库时，某一个字段属于一张表，但它**同时出现在另一个或多个表**，且完全等同于它在其本来所属表的意义表示，那么这个**字段就是一个冗余字段**  



**反三范式示例  **



![image-20210401124549739](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401124549739.png)

使用场景：

- 当需要查询“订单表”所有数据并且只需要“用户表”的name字段时, 没有冗余字段 就需要**去join**
  **连接用户表**,假设表中数据量非常的大, 那么会这次**连接查询就会非常大的消耗系统的性能**.
- 这时候冗余的字段就可以派上用场了, **有冗余字段我们查一张表**就可以了.  



创建一个关系型数据库设计，我们有两种选择：

1. 尽量遵循范式理论的规约，尽可能少的冗余字段，让数据库**设计看起来精致、优雅**、让人心醉。。。。
2. 合理的**加入冗余字段这个润滑剂**，减少join，让**数据库执行性能更高更快**。  











# MySQL 索引  





## 什么是索引  



在数据库表中，对字段建立索引可以大大**提高查询速度**。通过善用这些索引，可以令MySQL的查询和运行更加高效。

如果合理的设计且使用索引的MySQL是一辆兰博基尼的话，那么没有设计和使用索引的MySQL就是一个人力三轮车。拿汉语字典的目录页（索引）打比方，我们可以按拼音、笔画、偏旁部首等排序的目录（索引）快速查找到需要的字  



## 常见索引分类  



| 索引名称                   | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| **主键索引** (primary key) | 主键是一种**唯一性**索引,每个表只能有一个主键, 用于**标识数据表中的*每一条记录*** |
| **唯一索引** (unique)      | 唯一索引指的是 索引**列的所有值都只能出现一次**, 必须唯一.   |
| **普通索引** (index)       | 最常见的索引,作用就是 加快对数据的访问速度                   |



MySql将一个表的索引都保存在同一个索引文件中, 如果对数据进行增删改操作,MySql都会**自动的更新索引.**  



![image-20210401125455156](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401125455156.png)



在my.ini中修改data路径如下：

![image-20210401125950054](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401125950054.png)





### 主键索引 (PRIMARY KEY)  



特点: 主键是一种**唯一性索引**,每个表只能有一个主键,用于标识数据表中的某一条记录。

一个表可以没有主键，但最多只能有一个主键，并且**主键值不能包含NULL**。  

**主键属性   本身就是一个索引了**

```
ALTER TABLE 表名 ADD PRIMARY KEY ( 列名 )
```





### 唯一索引(UNIQUE)  



特点: 索引列的所有值都只能出现一次, 必须唯一.

唯一索引可以**保证数据记录的唯一性**。事实上，在许多场合，人们创建唯一索引的目的往往不是为了提高访问速度，而只是为了**避免数据出现重复**。  



```
create unique index 索引名 on 表名(列名(长度))
ALTER TABLE 表名 ADD UNIQUE ( 列名 )
```

```
CREATE UNIQUE INDEX ind_cname ON category(cname);

```

![image-20210401131052236](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401131052236.png)



唯一索引保证了数据的唯一性,索引的效率也提升了  







### 普通索引 (INDEX)  



普通索引（由关键字KEY或INDEX定义的索引）的唯一任务是**加快对数据的访问速度**。因此，应该只为那些**最经常出现在查询条件**（WHERE column=）或**排序条件**（ORDERBY column）中的数据列创建索引  



使用create index 语句创建: 在已有的表上创建索引  

```mysql
create index 索引名 on 表名(列名[长度])

ALTER TABLE 表名 ADD INDEX 索引名 (列名)
ALTER TABLE products ADD INDEX ind_pname (pname)
```





### 删除索引  

由于索引会**占用一定的磁盘空间**，因此，为了避免影响数据库的性能，应该及时删除不再使用的索引  

```mysql
ALTER TABLE table_name DROP INDEX index_name;

ALTER TABLE demo01 DROP INDEX dname_indx;
```







## 索引性能测试  



查询 test_index 表中的总记录数  

```
SELECT COUNT(*) FROM test_index;
```

![image-20210401133700214](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401133700214.png)

表中有 500万条数据  



为 dname字段 添加索引  

```
#添加索引
ALTER TABLE test_index ADD INDEX dname_indx(dname);
```



注意: 一般我们都是在创建表的时候 就确定需要添加索引的字段  

执行分组查询  

```
SELECT * FROM test_index where dname='name900000';
```

12s -> 0.3s



## 索引的优缺点总结  



添加索引首先应考虑在 **where** 及 **order by** 涉及的列上建立索引。

索引的优点

1. 大大的提高查询速度
2. 可以**显著的减少查询中分组和排序的时间**。

索引的缺点

1. **创建索引和维护索引需要时间**，而且数据量越大时间越长
2. 当对表中的数据进行增加，修改，删除的时候，**索引也要同时进行维护**，降低了**数据的维护速度**  











# MySQL 视图  



## 什么是视图  



1. 视图是一种虚拟表。
2. 视图建立在已有表的基础上, 视图赖以建立的这些表称为**基表**。
3. 向视图提供数据内容的语句为 **SELECT 语句**, 可以将视图**理解为存储起来的 SELECT 语句.**
4. 视图向用户**提供基表数据的另一种表现形式**  



## 视图的作用  



- **权限控制**时可以使用
  - 比如,某几个列可以运行用户查询,其他列不允许,可以开通视图 查询特定的列, 起到权限控制的作用
- 简化复杂的多表查询
  - 视图 本身就是**一条查询SQL**,我们可以**将一次复杂的查询 构建成一张视图**, 用户只要查询视图就可以**获取想要得到的信息**(**不需要再编写复杂的SQL)**
  - 视图主要就是为了**简化多表的查询**  



## 视图的使用  



```
create view 视图名 [column_list] as select语句;

view: 表示视图
column_list: 可选参数，表示属性清单，指定视图中各个属性的名称，默认情况下，与SELECT语句中查询的属性相同
as : 表示视图要执行的操作
select语句: 向视图提供数据内容
```



```mysql
#1. 先编写查询语句
#查询所有商品 和 商品的对应分类信息
SELECT * FROM products p LEFT JOIN category c ON p.`category_id` = c.`cid`;

#2.基于上面的查询语句,创建一张视图
CREATE VIEW products_category_view
AS SELECT * FROM products p LEFT JOIN category c ON p.`category_id` = c.`cid`;
```



查询视图 ,**当做一张*只读* 的表**操作就可以  

```
SELECT * FROM products_category_view
```



- **通过视图进行查询  **



![image-20210401140721091](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401140721091.png)



- 查询各个分类下的商品平均价格  

```mysql
#通过 多表查询
SELECT
cname AS '分类名称',
AVG(p.`price`) AS '平均价格'
FROM products p LEFT JOIN category c ON p.`category_id` = c.`cid`
GROUP BY c.`cname`;

# 通过视图查询 可以省略连表的操作
SELECT
cname AS '分类名称',
AVG(price) AS '平均价格'
FROM products_category_view GROUP BY cname;
```

**通过视图查询 可以省略连表的操作**







- 查询鞋服分类下最贵的商品的全部信息  

```mysql
#通过连表查询

#1.先求出鞋服分类下的最高商品价格
SELECT
MAX(price) AS maxPrice
FROM
products p LEFT JOIN category c ON p.`category_id` = c.`cid`
WHERE c.`cname` = '鞋服'

#2.将上面的查询 作为条件使用
SELECT * FROM products p LEFT JOIN category c ON p.`category_id` = c.`cid`
WHERE c.`cname` = '鞋服' AND p.`price` =
(SELECT
MAX(price) AS maxPrice
FROM
products p LEFT JOIN category c ON p.`category_id` = c.`cid`
WHERE c.`cname` = '鞋服');



#通过视图查询
SELECT * FROM products_category_view pcv
WHERE pcv.`cname` = '鞋服'
AND pcv.`price` = (SELECT MAX(price) FROM products_category_view WHERE cname ='鞋服')
```



## 视图与表的区别  





- 视图是**建立在表的基础上**，表存储数据库中的数据，而**视图只是做一个数据的展示**
- 通过视图**不能改变表中数据**（一般情况下视图中的数据都是表中的列 经过计算得到的结果**,不允许更新**）
- 删除视图，**表不受影响**，而**删除表，视图不再起作用**  









# MySQL 存储过程  







## 什么是存储过程  





- MySQL 5.0 版本开始支持存储过程。
- 存储过程（Stored Procedure）是一种在数据库中**存储复杂程序**，以便**外部程序调用**的一种数据库对象。存储过程是为了完成特定功能的**SQL语句集**，经编译创建并**保存在数据库**中，用户可通过指定存储过程的名字并给定参数(需要时)来调用执行。  



**简单理解: 存储过程其实就是一堆 SQL 语句的合并。中间加入了一些逻辑控制。  **





## 存储过程的优缺点  



优点:

- 存储过程一旦调试完成后，就可以稳定运行，（前提是，业务需求要相对稳定，没有变化）
- 存储过程**减少业务系统与数据库的交互**，降低耦合，**数据库交互更加快捷**（应用服务器，与数据库服务器不在同一个地区）

缺点:

- 在互联网行业中，大量使用MySQL，MySQL的存储过程与Oracle的相比较弱，所以**较少使用**，并且互联网行业需求变化较快也是原因之一
- 尽量在简单的逻辑中使用，**存储过程移植十分困难**，数据库集群环境，保证各个库之间存储
  过程变更一致也十分困难。
- 阿里的代码规范里也提出了禁止使用存储过程，存储过程维护起来的确麻烦；  

![image-20210401142148423](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401142148423.png)





## 存储过程的创建方式  



### 方式1  



创建商品表 与 订单表  



```mysql
# 商品表
CREATE TABLE goods(
gid INT,
NAME VARCHAR(20),
num INT -- 库存
);

#订单表
CREATE TABLE orders(
oid INT,
gid INT,
price INT -- 订单价格
);

# 向商品表中添加3条数据
INSERT INTO goods VALUES(1,'奶茶',20);
INSERT INTO goods VALUES(2,'绿茶',100);
INSERT INTO goods VALUES(3,'花茶',25);
```



创建简单的存储过程  

```mysql
DELIMITER $$ -- 声明语句结束符，可以自定义 一般使用$$
CREATE PROCEDURE 过程名称() -- 声明存储过程
BEGIN -- 开始编写存储过程
	-- 要执行的操作
END $$ -- 存储过程结束
```



编写存储过程, 查询所有商品数据  

```mysql
DELIMITER $$
CREATE PROCEDURE goods_proc()
BEGIN
	select * from goods;
END $$
```



调用存储过程  

```mysql
-- 调用存储过程 查询goods表所有数据
call goods_proc;
```

![image-20210401143333054](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401143333054.png)

其实就是个函数



### 方式2  



IN 输入参数：表示**调用者向存储过程传入值**  

```
CREATE PROCEDURE 存储过程名称(IN 参数名 参数类型)
```



创建接收参数的存储过程  

接收一个商品id, 根据id删除数据  

```mysql
DELIMITER $$

CREATE PROCEDURE goods_proc02(IN goods_id INT)
BEGIN
	DELETE FROM goods WHERE gid = goods_id ;
END $$
```





```mysql
# 删除 id为2的商品
CALL goods_proc02(2);
```





### 方式3  



变量赋值  

OUT 输出参数：表示存储过程向调用者传出值  



向订单表 插入一条数据, **返回1,表示插入成功**  

```mysql
# 创建存储过程 接收参数插入数据, 并返回受影响的行数
DELIMITER $$
CREATE PROCEDURE orders_proc(IN o_oid INT , IN o_gid INT ,IN o_price INT, OUT out_num INT)
BEGIN
    -- 执行插入操作
    INSERT INTO orders VALUES(o_oid,o_gid,o_price);
    -- 设置 num的值为 1
    SET @out_num = 1;
    -- 返回 out_num的值
    SELECT @out_num;
END $$
```



```mysql
# 调用存储过程插入数据,获取返回值
CALL orders_proc(1,2,30,@out_num);
```







# MySQL触发器  



## 什么是触发器  



触发器（trigger）是MySQL提供给程序员和数据分析员来保证数据完整性的一种方法，它是与表事件相关的特殊的存储过程，它的执行不是由程序调用，也不是手工启动，而是**由事件来触发**，**比如当对一个表进行操作（insert，delete， update）时就会激活它执行。**  



**简单理解: 当我们执行一条sql语句的时候，这条sql语句的执行会自动去触发执行其他的sql语句。  **





**触发器创建的四个要素  **

1. 监视地点（table）表
2. 监视事件（insert/update/delete）操作
3. 触发时间（before/after）时间
4. 触发事件（insert/update/delete） 操作





## 创建触发器  





```mysql
delimiter $ -- 将Mysql的结束符号从 ; 改为 $,避免执行出现错误
CREATE TRIGGER Trigger_Name -- 触发器名，在一个数据库中触发器名是唯一的
before/after（insert/update/delete） -- 触发的时机 和 监视的事件
on table_Name -- 触发器所在的表
for each row -- 固定写法 叫做行触发器, 每一行受影响，触发事件都执行
begin
	-- begin和end之间写触发事件
end
$ -- 结束标记
```





需求: 在下订单的时候，对应的商品的库存量要相应的减少，卖出商品之后减少库存量。  



```mysql
-- 1.修改结束标识
DELIMITER $
-- 2.创建触发器
CREATE TRIGGER t1
-- 3.指定触发的时机,和要监听的表
AFTER INSERT 
ON orders
-- 4.行触发器 固定写法
FOR EACH ROW
-- 4.触发后具体要执行的事件
BEGIN
    -- 订单+1 库存-1
    UPDATE goods SET num = num -1 WHERE gid = 1;
END$
```



向订单表中添加一条数据  

```mysql
INSERT INTO orders VALUES(1,1,25);
```

![image-20210401150352138](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401150352138.png)

goods表中的数据随之 -1  







# DCL(数据控制语言)  





MySql默认使用的都是 **root 用户**，超级管理员，拥有**全部的权限**。除了root用户以外，我们还可以通过DCL语言来**定义一些权限较小的用户,** 分配**不同的权限**来管理和维护数据库。  





## 创建用户  



```
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
```

| 参数   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| 用户名 | 创建的新用户,登录名称                                        |
| 主机名 | 指定该用户**在哪个主机上可以登陆**，本地用户可用 localhost 如果想让该用户可以 **从任意远程主机登陆**，可以**使用通配符 %** |
| 密码   | 登录密码                                                     |



创建 admin1 用户，只能在 localhost 这个服务器登录 mysql 服务器，密码为 123456  

```
CREATE USER 'admin1'@'localhost' IDENTIFIED BY '123456';
```



创建的用户在名字为 mysql的 数据库中的 user表中  

![image-20210401182522515](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401182522515.png)



创建 admin2 用户可以在任何电脑上登录 mysql 服务器，密码为 123456  

```
CREATE USER 'admin2'@'%' IDENTIFIED BY '123456';
```

**% 表示 用户可以在任意电脑登录 mysql服务器**  





## 用户授权  



创建好的用户,需要进行授权  

```
GRANT 权限 1, 权限 2... ON 数据库名.表名 TO '用户名'@'主机名';
```



| 参数 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| 权限 | 授予用户的权限，如 CREATE、ALTER、SELECT、INSERT、UPDATE 等。 如果要授 予所有的权限则使用 ALL |
| ON   | 用来指定权限针对哪些库和表。                                 |
| TO   | 表示将权限赋予某个用户。                                     |



给 admin1 用户分配对 **db4 数据库中 products 表**的 操作权限：**查询**  

```
GRANT SELECT ON db4.goods TO 'admin1'@'localhost';
```

![image-20210401183156373](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401183156373.png)

发现admin1只能看到db4的goods表，其他都看不到

**只有select权限，不能修改**

![image-20210401183229340](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401183229340.png)





给 admin2 用户分配**所有权限**，对所有数据库的**所有表**  

```
GRANT ALL ON *.* TO 'admin2'@'%';
```



## 查看权限  

```
SHOW GRANTS FOR '用户名'@'主机名';

-- 查看root用户的权限
SHOW GRANTS FOR 'root'@'localhost';
```





## 删除用户  



```
-- 删除 admin1 用户
DROP USER 'admin1'@'localhost';  


```





## 查询用户



选择名为 mysql的数据库, 直接查询 user表即可  

```
-- 查询用户
SELECT * FROM USER;
```







# 数据库备份&还原  





进入到Mysql安装目录的 bin目录下, 打开命令行  

```
mysqldump -uroot -p db4 > D:/db4.sql
```

![image-20210401185852359](../picture/MySQL%E5%9F%BA%E7%A1%80/image-20210401185852359.png)

恢复数据 还原 db4 数据库中的数据

**没有创建数据库的操作，需要手动创建后再恢复**

注意：还原的时候需要先创建一个 db4数据库  

```
mysql> source sql文件地址
```









# TIP



## **CHAR(M), VARCHAR(M)不同之处**



CHAR(M)定义的列的长度为固定的，M取值可以为0～255之间，当保存CHAR值时，在它们的右边填充空格以达到指定的长度。当检 索到CHAR值时，***尾部的空格被删除掉***。在存储或检索过程中不进行大小写转换。CHAR存储定长数据很方便，**CHAR字段上的索引效率级高**，比如定义 char(10)，那么不论你存储的数据是否达到了10个字节，都要占去10个字节的空间,不足的自动用空格填充。

VARCHAR(M)定义的列的长度为**可变长字符串**，M取值可以为0~65535之间，(VARCHAR的最大有效长度由**最大行大小**和**使用 的字符集**确定。**整体最大长度是65,532字节**）。VARCHAR值保存时只保存需要的**字符数**，另加一个字节来**记录长度**(如果列声明的长度超过255，则 **使用两个字节**)。VARCHAR值保存时**不进行填充**。当值保存和检索时**尾部的空格仍保留**，符合标准SQL。varchar存储变长数据，但存储效率没有 CHAR高。如果一个字段可能的值是**不固定长度的**，我们只知道它不可能超过10个字符，把它定义为 VARCHAR(10)是最合算的。VARCHAR类型的**实际长度是它的值的实际长度+1**。为什么"+1"呢？这一个字节**用于保存实际使用了多大的长度**。 从空间上考虑，用varchar合适；从效率上考虑，用char合适，关键是根据实际情况找到权衡点。



CHAR和VARCHAR最大的不同就是一个是固定长度，一个是可变长度。由于是可变长度，因此实际存储的时候是实际字符串再加上一个记录 **字符串长度的字节**(如果超过255则需要两个字节)。如果分配给CHAR或VARCHAR列的值超过列的最大长度，则对值进行裁剪以使其适合。如果被裁掉 的字符不是空格，则会产生一条警告。如果裁剪非空格字符，则**会造成错误**(而不是警告)并**通过使用严格SQL模式禁用值的插入。**





## mysql 事务中如果有sql语句出错，会导致自动回滚吗





 一、如果事务中，有某条sql语句执行时报错了，我们没有手动的commit，那整个事务会自动回滚吗？

答案：会。

![img](https://img2018.cnblogs.com/blog/140030/201812/140030-20181227132544138-1829140247.png)

当我们**直接把窗口关掉**，新开窗口再去查询表name时，表中没有第一次插入的记录，说明整个事务被回滚了。

 

二、如果事务中，有某条sql语句执行时报错了，我们手动的commit，那整个事务还会回滚吗？

答案：不会。

事务中如果**有sql执行错误**，但是我们**commit了**，mysql**仍然会执行成功的语句**，**并不会把整个事务自动回滚。**





三、如果事务中，有某条sql语句执行时报错了，我们重新又开启一个事务，那之前事务还会回滚吗？

答案：不会。

![img](../picture/MySQL%E5%9F%BA%E7%A1%80/140030-20181227134149465-1132416156.png)

我们在该会话中，重新用 begin; 开启了一个新事务，新开的事务会将该会话中未提交的事务提交(相当于commit)，所以上一个事务被提交了，导致语句一记录写入表中。













































































































































































































