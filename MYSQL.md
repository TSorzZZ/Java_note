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
ALTER TABLER teacher1 ADD  age INT(11)
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