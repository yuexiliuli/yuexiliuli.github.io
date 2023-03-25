---
title: SQL Server创建表语句
urlname: df434s
date: '2021-03-17 21:12:45 +0800'
tags: [数据库,SQL]
categories: [数据库,SQL]
---

# SQL Server 创建表语句

参考：

[SQL CREATE TABLE 语句](https://www.w3school.com.cn/sql/sql_create_table.asp)
[SQL Server 创建表语句介绍](https://blog.csdn.net/kepa520/article/details/78793431)

## 一、基本实例

SQL Server 创建表是最常见也是最常用的操作之一，下面就为您介绍 SQL Server 创建表的语句写法，供您参考，希望可以让您对 SQL Server 创建表方面有更深的认识。

```sql
USE suntest
create table 仓库
(
仓库编号 int ,
仓库号 varchar(50) ,
城市 varchar(50) ,
面积 int
)

create table 仓库1
(
仓库编号 int not null ,
仓库号 varchar(50) not null,
城市 varchar(50) not null, --不能为空not null--
面积 int
)

create table 仓库2
(
仓库编号 int primary key , --主键的关键字primary key--
仓库号 varchar(50) unique, --唯一索引关键字unique--
城市 varchar(50) not null, --不能为空not null--
面积 int
)

create table 仓库3
(
仓库编号 int primary key , --主键的关键字primary key--
仓库号 varchar(50) unique, --唯一索引关键字unique--
城市 varchar(50) default '青岛', --不能为空not null--
面积 int check (面积>=300 and 面积<=1800)
)

create table 职工表
(
职工编号 int identity (1,1) primary key,
职工号 varchar(50) unique,
仓库号 varchar(50),
工资 int check(基本工资>=800 and 基本工资<=2100),
)

create table 订单表
(
订单编号 int identity(1,1) primary key,
订单号 varchar(50) unique,
职工号 varchar(50) references 职工表(职工号),--references两张表通过“职工号”关联--
订购日期 datetime,
销售金额 int
)

create table 阳光工资表
(
职工编号 int identity (1,1) primary key,
职工号 varchar(50) unique,
仓库号 varchar(50),
基本工资 int check(基本工资>=800 and 基本工资<=2100),
加班工资 int,
奖金 int,
扣率 int,
应发工资 as (基本工资+加班工资+奖金-扣率) --as为自动计算字段，不能输入值--
)
```

## 二、详细划分

### 1：临时表

在 sql 语句中，临时表有两类，分别是局部(local)和全局(global)临时表，局部临时表只在其会话（事务)中可见，全局临时表可以被会话(事务)中的任何程序或者模块访问

### 2：创建局部临时表

```sql
use db_sqlserver
go
create table #db_local_table
(
  id  int,
  name varchar(50),
  age int,
  area int
)
```

创建的临时表不能与其他会话共享，当会话结束时，行和表的定义都将被删除

### 3：创建全局临时表

```sql
use db_sqlserver
go
create table ##db_local_table
(
  id  int,
  name varchar(50),
  age int,
  area int
)
```

全局临时表对所有用户都是可见的，在每个访问该表的用户都断开服务器连接时，全局临时表才会被删除

### 4：创建主键、外键关联的数据库表

```sql
use db_sqlserver;
go
create table db_table5
(
  职工编号 int primary key,
  职工号  varchar(50) unique,
  仓库号  varchar(50),
  工资   int
)

go
create table db_table6
(
  订单编号 int primary key,
  订单号  varchar(50) unique,
  职工号 varchar(50) references db_table5(职工号),
  订购日期 datetime,
  销售金额 int
)
```

### 5：创建具有 check 约束字段的数据库表

```sql
use db_sqlserver;
go
create table db_table7
(
  仓库编号 int primary key,
  职工号  varchar(50) unique,
  仓库号  varchar(50),
  工资   int,
  面积  int check(面积>=600 and 面积<=1800)
)
```

### 6：创建含有计算字段的数据库表

```sql
use db_sqlserver;
go
create table db_table8
(
  职工编号 int primary key,
  职工号 varchar(50) unique,
  仓库号 varchar(50),
  基本工资 int check(基本工资>=800 and 基本工资<=2100),
  加班工资 int,
  奖金 int,
  扣率 int,
  应发工资 as (基本工资 + 加班工资 + 奖金 - 扣率)
)
```

### 7：创建含有自动编号字段的数据库表

```sql
use db_sqlserver;
go
create table db_table9
(
   仓库编号 int identity(1,1) primary key,
   仓库号 varchar(50) unique,
   城市 varchar(50) default('青岛'),
   面积 int check(面积>=300 and 面积<=1800)
)
```

向表中添加记录：

```sql
insert into [db_sqlserver].[dbo].[db_table9](仓库号, 面积) values('400', 1600);
```

仓库编号会自动增加

### 8：创建含有排序字段的数据表

```sql
create table db_table10
(
   仓库编号 int identity(1, 1) primary key,
   仓库号 varchar(50) collate french_CI_AI not null,
   城市 varchar(50) default '青岛',
   面积 int check(面积>=300 and 面积<=1800)
)
```

仓库号是一个排序字段，其中 CI(case insensitive)表示不区分大小写，AI(accent insensitive)表示不区分重音，即创建的是一个不区分大小写
和不区分重音的排序。如果要区分大小和和区分排序，修改代码为：French_CS_AS

### 9：动态判断数据库表是否存在

```sql
use db_sqlserver;
go
if(Exists(select * from sys.sysobjects where id=OBJECT_ID('db_table9')))
  print '数据库表名已经存在'

else
  print '该数据库表名不存在，可以利用该名创建表'
```

### 10：查看表的各种信息，可以查看指定数据库表的属性、表中字段属性、各种约束等信息

```sql
use db_sqlserver;
go
execute sp_help db_table9;
```

### 11：用 select 语句查看数据库表的属性信息

```sql
use db_sqlserver;
go
select * from sysobjects where type='U'
```

### 12：重命名数据库表

```sql
use db_sqlserver;
go
execute sp_rename "db_table9", "db_renametable"
```

### 13：增加数据库表的新字段

```sql
use db_sqlserver;
go
alter table db_table1 add 电子邮件 varchar(50)
alter table db_table1 add 联系方式 varchar(50) default '0532-88886396'

select name 字段名, xusertype 类型编号, length 长度 from syscolumns where id = object_id('db_table1')
```

### 14：修改数据库表的字段

```sql
use db_sqlserver;
go
alter table db_table1 alter column 电子邮件 varchar(200)


select name 字段名, xusertype 类型编号, length 长度 from syscolumns where id = object_id('db_table1')
```

### 15：删除数据库表字段

```sql
use db_sqlserver;
go
alter table db_table1 drop column 电子邮件

select name 字段名, xusertype 类型编号, length 长度 from syscolumns where id = object_id('db_table1')
```

### 16：删除数据库表

```sql
use db_sqlserver;
go
drop table db_table1
drop table db_table1, db_table2
```

如果删除有依赖关联的数据库表，即主键、外键关键表、则要删除两个表之间的关联约束，然后才能删除表。注意，也可以先删除引用该表的数据库表，然后即可删除该表，
