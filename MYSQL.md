# MYSQL

## 1、 MYSQL介绍

+ JavaEE：企业级Java开发 Web

+ 前端： 页面 展示 数据

+ 后台：链接数据库JDBC，链接前端(控制，控制视图跳转，给前端传递数据)

### 1.1 什么是数据库

概念：数据仓库，软件，安装在操作系统上，可以存储和管理大量数据

### 1.2 数据库分类

**关系型数据库：(SQL)**

+ MySQL，Oracle，Sql Server，DB2，SQLite

+ 通过表盒表之间，行和列之间的关系进行数据存储

 __非关系型数据库：(Not Only SQL)__

+ Redis,MongDB
+ 非关系型数据库，对象存储，通过对象自身的属性决定

### 1.3 链接数据库

```mysql
update user set password=password('123456')where user='root'; --修改密码
flush privileges; --刷新数据库
show databases; --显示所有数据库
use dbname；--打开某个数据库
show tables; --显示数据库mysql中所有的表
describe user; --显示表mysql数据库中user表的列信息
create database name; --创建数据库
use databasename; --选择数据库
exit; --退出Mysq
? --命令关键词 : 寻求帮助
-- 表示注释
```

__数据库定义语言__

__DDL 	数据库定义语言__

__DML 	数据库操作语言__

__DQL 	数据库查询语言__

__DCL 	数据库控制语言__



## 2、操作数据库

操作数据库->操作数据库中的表->操作数据库中的数据

<font color="red">mysql关键字不区分大小写</font>



### 2.1 数据库指令

__创建数据库__

```mysql
CREATE DATABASE [IF NOT EXISTS] ts;
```

__删除数据库__

```mysql
DROP DATABASE [IF EXISTS] ts;
```

__切换数据库__

```mysql
-- 如果表名或字段名是特殊字符，需要带``
USE `school`
```

__查看所有数据库的表__

```mysql
SHOW DATABASE -- 查看所有数据库
```



### 数据库的列类型

NULL值：和任何值计算结果都是NULL

**常用数值：**

int 4字节         

float 4字节

double 4字节       

decimal 字符串形式的浮点数

**字符串(长度单位为字符)：**

char       固定            0-255   

varchar 可变字符串 0-65535                   常用String

tinytext 微型文本     2^8-1 

text        文本串        2^16-1                     保存大文本

**时间日期：**

date        	YYYY-MM-DD	日期格式

time			HH:mm:ss	  时间格式				

datetime    YYYY-MM-DD HH：mm：ss          最常用时间格式

timestamp 时间戳           1970.1.1到现在的毫秒数！

year    年份表示



### 2.3、数据库的字段属性

**Unsigned：**无符号整数，该列不能为负数

**zerofill：**0填充，不足的位数用0填充，int(3) 5---005

**自增：**在上一条记录的基础上+1，通常用来设计唯一的主键~ index，必须为整数类型，可以自定义自增起始值和步长

**非空：** null || not null 。

**默认：**设置默认值

### 2.4、创建数据库表

```mysql
-- 例子
CREATE TABLE IF NOT EXISTS `student` ( 
    `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
    `name` VARCHAR(20) NOT NULL DEFAULT '匿名' COMMENT '姓名',
    `pwd` VARCHAR(30) NOT NULL DEFAULT '123456' COMMENT '密码',
    `sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
    `birthday` DATETIME DEFAULT NULL COMMENT '生日',
    `address` VARCHAR(100) DEFAULT NULL COMMENT '地址',
    PRIMARY KEY(`id`) 
)ENGINE=INNODB DEFAULT CHARSET=utf8; 
```

```sql
-- 创建表语句格式
CREATE TALBE [IF NOT EXISTS] `表名`(
 `字段名` 列类型 [属性] [索引] [注释],
 `字段名` 列类型 [属性] [索引] [注释],
 `字段名` 列类型 [属性] [索引] [注释]
)[表类型] [字符集设置] [注释]


```

**常用命令**

```sql
SHOW CREATE DATABASE `数据库名` --查看建库语句
SHOW CREATE TABLE `表名` --查看建表语句
DESC `表名` --显示表结构


```

### 2.5、数据表的类型

|            |                 INNODB                 |      MYISAM      |
| ---------- | :------------------------------------: | :--------------: |
| 事务支持   |                  支持                  |      不支持      |
| 数据行锁定 |                  支持                  |      不支持      |
| 外键约束   |                  支持                  |      不支持      |
| 全文索引   |                 不支持                 |       支持       |
| 表空间大小 |          大，约为MYISAM的两倍          |        小        |
| 优劣势对比 | 安全性高，支持事务处理，多表多用户操作 | 节约空间，速度快 |

物理空间位置  

InnoDB：.frm文件和上级目录ibdata1文件

MyISAM：.frm表结构定义  .MYD 数据文件  .MYI 索引文件

**设置数据库表的字符集编码：**

```mysql
CHARSET=utf8
```



### 2.6、表的修改和删除

```sql
-- 修改表名
ALTER TABLE teacher RENAME AS teacher1
-- 增加表字段
ALTER TABLE teacher1 ADD  age INT(11)
-- 修改表字段
	-- 修改约束
ALTER TABLE teacher1 MODIFY age VARCHAR(11)
	-- 字段重命名
ALTER TABLER teacher1 CHANGE age age1 INT(11)

-- 删除表字段
ALTER TABLE teacher1 DROP age1

-- 删除表
DROP TABLE IF EXISTS teacher1
```

**注意：**

>字段名最好都用``包裹
>
>注释 -- /**/
>
>sql关键字不敏感



## 3、MySQL数据管理

### 3.1、外键

```mysql
-- 创建外键的方式一 : 创建子表同时创建外键

-- 年级表 (id\年级名称)
CREATE TABLE `grade` (
`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级ID',
`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
PRIMARY KEY (`gradeid`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

-- 学生信息表 (学号,姓名,性别,年级,手机,地址,出生日期,邮箱,身份证号)
CREATE TABLE `student` (
`studentno` INT(4) NOT NULL COMMENT '学号',
`studentname` VARCHAR(20) NOT NULL DEFAULT '匿名' COMMENT '姓名',
`sex` TINYINT(1) DEFAULT '1' COMMENT '性别',
`gradeid` INT(10) DEFAULT NULL COMMENT '年级',
`phoneNum` VARCHAR(50) NOT NULL COMMENT '手机',
`address` VARCHAR(255) DEFAULT NULL COMMENT '地址',
`borndate` DATETIME DEFAULT NULL COMMENT '生日',
`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
`idCard` VARCHAR(18) DEFAULT NULL COMMENT '身份证号',
PRIMARY KEY (`studentno`),
KEY `FK_gradeid` (`gradeid`),
CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`)
) ENGINE=INNODB DEFAULT CHARSET=utf8


-- 创建外键方式二 : 创建子表完毕后,修改子表添加外键
ALTER TABLE `student`
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`);

```



### 3.2、DML语言

### 3.3、添加

基本语法：

```mysql
-- insert into `表名` (字段1,字段2...) values (v1,v2...)，(v1,v2...)
-- 主键如果自增  可以省略
-- 字段可以省略  但是value要一一对应
```



### 3.4、修改

```mysql
--修改name字段
UPDATE `student` SET `name`='超级萌萌' WHERE NAME='22'

--不指定条件的情况下，会改动所有表
UPDATE 	`student` SET `name`='帅气的萌萌' 

--修改多个属性
UPDATE `student` SET `name`='超级无敌萌',`pwd`='988',`email`='@qqq.com' WHERE id=1;

--语法:
--update `表名` set `字段名` ='values' where 条件
```

**where条件语句**

### 3.4、删除

```mysql
--删除数据(避免这样写)
DELETE FROM `student`;
--删除一条数据
DELETE FROM `student` WHERE id=1;
--清空student
truncate `student`
```

**delete 和 truncate 区别**

- 相同:都能删除数据，都不会删除表结构

- 不同:

  1)TRUNCATE 重新设置自增列，计数器会归零

  2)不会影响事务

**delete删除的问题，重启数据库现象**

- innoDB 自增列会从1开始(存在内存当中，断电即失)
- MyISAM继续从上一个自增量开始(存在文件中，不会丢失)



## 4、数据库查询语句（Data Query Language）

### 4.1、基本语法

```mysql
SELECT [ALL | DISTINCT]
{* | table.* | [table.field1[as alias1][,table.field2[as alias2]][,...]]}
FROM table_name [as table_alias]
    [left | right | inner join table_name2]  -- 联合查询
    [WHERE ...]  -- 指定结果需满足的条件
    [GROUP BY ...]  -- 指定结果按照哪几个字段来分组
    [HAVING]  -- 过滤分组的记录必须满足的次要条件
    [ORDER BY ...]  -- 指定查询记录按一个或多个条件排序
    [LIMIT {[offset,]row_count | row_countOFFSET offset}];
    --  指定查询的记录从哪条至哪条
```

```mysql
-- 查询所有学生 SELECT 字段 FROM 表
SELECT * From `student`

-- 查询指定字段
SELECT `studentno`,`studentname` FROM `student`

-- 别名，给结果起一个名字 AS, 可以给字段起别名，也可以给表起别名
SELECT `studentno` AS 学号, `studentname` AS 学生姓名 FROM `student` AS s

-- 函数 Concat(a,b)
SELECT CONCAT('姓名:', `studentname`) AS 新名字 FROM `student`

-- 发现重复数据，去重
SELECT DISTINCT `studentno` FROM `result` 

SELECT VERSION() -- 查询系统版本 (函数）
SELECT 100*3-1 AS 计算结果 -- 用来计算 （表达式）
SELECT @@auto_increment_increment -- 查询自增的步长 （变量）

-- 学员考试成绩 +1 分查看
SELECT `studentno`,`studentresult`+ 1 AS '提分后' FROM `result`

```

### 4.2、模糊查询

|   运算符    |       语法        | 功能 |
| :---------: | :---------------: | :--: |
|   is null   |                   |      |
| is not null |                   |      |
| between and | a between b and c |      |
|  **like**   |     a like b      |      |
|   **In**    |  a in(a1,a2....)  |      |

```mysql
-- =======================模糊查询======================
-- 查询姓刘的同学
-- like 结合 %(代表0到任意个字符） _（一个字符）
SELECT `studentno`, `studentname` FROM `student`
WHERE `studentname` LIKE '张%'

-- 查询修张的同学，名字后面只有一个字的
SELECT `studentno`, `studentname` FROM `student`
WHERE `studentname` LIKE '张_'

-- 查询修张的同学，名字后面只有两个个字的
SELECT `studentno`, `studentname` FROM `student`
WHERE `studentname` LIKE '张__'

-- 查询名字中间带有伟的 %伟%
SELECT `studentno`, `studentname` FROM `student`
WHERE `studentname` LIKE '%伟%'

-- =====in =====
-- 查询1001，1002，1003号学员
SELECT `studentno`, `studentname` FROM `student`
WHERE `studentno` IN(1001, 1002, 1003)

-- 查询在北京的学员
SELECT `studentno`, `studentname` FROM `student`
WHERE  `address` IN('北京朝阳');

SELECT `studentno`, `studentname` FROM `student`
WHERE  `address` IN('北京'); -- 查不出来


-- ====== null  not null ====

-- 查询地址为空的学生 null ''
SELECT `studentno`, `studentname` FROM `student`
WHERE  `address` = '' OR `address` IS NULL


-- 查询有出生日期 不为空
SELECT `studentno`, `studentname` FROM `student`
WHERE  `borndate` IS NOT NULL

```

### 4.3、连接查询

<img src="https://img-blog.csdnimg.cn/20210208165037862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTQ4MzMyOA==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述" style="zoom:150%;" />

```mysql
SELECT s.studentno , s.studentname, subjectno, studentresult
FROM student AS s
INNER JOIN result AS r
WHERE s.studentno = r.studentno

SELECT student.studentno, student.studentname, result.subjectno, result.studentresult
FROM student
INNER JOIN result
WHERE student.studentno = result.studentno

-- Right Join
SELECT s.studentno, studentname, subjectno, studentresult
FROM student s
RIGHT JOIN result r
ON s.studentno = r.studentno

-- Left Join
SELECT s.studentno, studentname, subjectno, studentresult
FROM student s
LEFT JOIN result r
ON s.studentno = r.studentno

-- 查询缺考的同学
-- Left Join
SELECT s.studentno, studentname, subjectno, studentresult
FROM student s
LEFT JOIN result r
ON s.studentno = r.studentno
WHERE studentresult IS NULL

-- 这里发现 = null查询不到想要的结果，即这种写法要规避
-- SQL实际上使用is null 和 is not null判断字段为空，注意为空不代表为空字符串和0
-- 而null = null和null<>null其实返回的都是false，任何值和null做运算的结果都是false
SELECT s.studentno, studentname, subjectno, studentresult
FROM student s
LEFT JOIN result r
ON s.studentno = r.studentno
WHERE studentresult = NULL


- 思考题（查询了参加考试的同学信息：学号，学生姓名，科目名，分数）
/*思路
1.分析需求，分析查询的字段来自哪些表，student,result, subject（连接查询）
2.确定使用哪种连接查询？ 7种
确定交叉点（这两个表中的哪个数据是相同的）
判断条件： 学生表中的studentno = 成绩表 studentno
*/
SELECT s.studentno, studentname, subjectname, studentresult
FROM student s
RIGHT JOIN result r
ON s.studentno = r.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno

-- 我要查询那些数据 select 。。。
-- 从那几个表中查FROM表 xxx join 连接的表 on 交叉条件
-- 假设存在一种多张表查询，从两张慢慢增加

-- from a left jion b
-- from a rigth join b

-- 查询科目所属的年级（科目名称，年级名称）
SELECT `subjectname`, gradename
FROM `subject` sub
INNER JOIN grade g
ON sub.gradeid = g.gradeid

-- 查询了参加 高等数据-1 考试的同学信息： 学号，学生姓名，科目名称，分数
SELECT s.studentno, studentname, `subjectname`, studentresult
FROM student s
INNER JOIN result r
ON s.studentno = r.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE `subjectname` = '高等数学-1'

```

**自连接：表自己与表自己连接（一张表拆成两张表分析）**

```mysql
SELECT a.`categoryname` AS '父栏目' , b.`categoryname` AS '子栏目'
FROM `category` AS a, `category` AS b
WHERE a.`categoryid` = b.`pid`
```



### 4.4、分类和排序

```mysql
-- 排序： 升序 ASC， 降序 DESC

-- order BY 字段 升序降序
-- 根据查询结果根据 成绩降序	排序
SELECT s.studentno, studentname, `subjectname`, studentresult
FROM student s
INNER JOIN result r
ON s.studentno = r.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE `subjectname` = '高等数学-1'
ORDER BY studentresult ASC

-- 为什么要分页？
-- 缓解数据库压力，给人的体验更好

-- 分页，每页只显示五条数据
-- 语法：limit 起始值，页面的大小
-- 网页应用： 当前页，总的页数，页面的大小
-- LIMIT 0,5 1~5

SELECT s.studentno, studentname, `subjectname`, studentresult
FROM student s
INNER JOIN result r
ON s.studentno = r.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE `subjectname` = '高等数学-1'
ORDER BY studentresult ASC
LIMIT 5,5

-- 第一页 LIMIT 0,5  （1 - 1）* 5
-- 第二页 LIMIT 5,5 （2 - 1）* 5
-- 第三页 LIMIT 10,5 （3 - 1）* 5
-- 第四页 LIMIT 15,5 （4 - 1）* 5  （n - 1)*pagesize, pagesize
-- [pagesize:页面大小]
-- [（n - 1)*pagesize 起始值]
-- [n: 当前页]
-- [数据总数/页面大小 + 1= 总页数]


-- 查询 JAVA第一学年 课程成绩排名前十的学生，并且分数要大于80的学生（学号，姓名，课程名称，分数）
SELECT s.studentno, studentname, `subjectname`, studentresult
FROM student s
INNER JOIN result r
ON s.studentno = r.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname = 'Java程序设计-1' AND studentresult > 80
ORDER BY studentresult DESC
LIMIT 0,10


```



### 4,5、 分组和过滤

```mysql
-- 查询不同课程的平均分，最高分，最低分
-- 核心：（根据不同的课程分组）
SELECT `subjectname`, AVG(studentresult) AS 平均分, MAX(studentresult) AS 最高分, MIN(studentresult) AS 最低分
FROM result r
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
GROUP BY r.subjectno -- 通过什么字段分组
HAVING 平均分 > 60

```



## 5、MySQL中的函数

### 5.1、常用函数

```sql
-- 数学运算
select ABS(-8)	-- 绝对值
select CEILING(9.4) -- 上取整
select floor(9.4)	-- 下取证
select RAND()	-- 0-1之间的随机数
select SIGN(10)	-- 符号函数

-- 字符串函数
select CHAR_LENGTH()	-- 字符串长度
select CONCAT(s1,s2,s3...)	-- 字符串拼接
select INSERT(str,pos,newstr)	-- 指定位置插入
select replace(str1,substr,str2)
select UPPER()
select LOWER()
select substr(str,pos,len)
select reverse()

-- 时间和日期函数
select CURRENT_DATE() -- 获取当前日期
select CURRENT_DATE() -- 获取当前日期
select NOW()	-- 获取当前时间
select LOCALTIME() -- 获取本地时间
select SYSDATE() -- 系统时间
select YEAR()
select MONTH()
select DAY()
select HOUR()
select MINUTE()
select SECOND()

-- 系统
SELECT SYSTEM_USER()
SELECT USER()
SELECT VERSION()

```

### 5.2、常用函数

```mysql
select COUNT()	-- 计数
select SUM()	-- 累加
select AVG()	-- 均值
select MAX()	-- 最大值
select MIN()	-- 最小值
```





