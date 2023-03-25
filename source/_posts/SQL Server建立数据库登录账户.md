---
title: SQL Server建立数据库登录账户
urlname: fr374g
date: '2021-05-15 21:27:29 +0800'
tags: [数据库,SQL]
categories: [数据库,SQL]
---

# 参考：[SQL Server 创建登录用户，授权](https://blog.csdn.net/u012997311/article/details/72630321)

```sql
要成功访问数据库数据，需要两个方面的权限，
（1）连接数据库服务器的权限
（2）需要获得访问某个特定的数据库数据的权限

--创建登录账户
create login u001  with password='u001',default_database=filmDB--名称和默认数据库不加引号
   --用存储过程创建
 exec sp_addlogin 'u002','u002'
--登录账户需要关联到数据库用户（多个），默认是一样的名字，
--登录账户只是用来连接服务器，数据库的访问需要数据库用户。
--创建数据库用户
  /***************************************
  *在数据库 filmDB中创建一个数库用户，名字也叫u001
  *并且将登录名u001和数据库用户u001映射，然后再给数据库用户赋予数据操作权限
  *作者：
  *时间：2017年5月15日15:48:54
  ******************************************/
  use filmDB
  go
  CREATE USER u001 for login u001  with default_schema=dbo --default-schema可以不加
  go
	 --用存储过程创建
	 EXEC sp_grantdbaccess 'u001','u001'--前面是登录名后面是数据库用户名
  --为数据库用户设置权限
    EXEC sp_addrolemember 'db_datareader','u001' --给u001这个用户一个db_datareader角色


	GRANT SELECT,INSERT,UPDATE ON filmInfo to u001  --给filmInfo u001设置权限

--禁用u001登录账户
alter login u001  disable
--启用u001
alter login u001  enable
--登录账号的密码修改
alter login u001 with password=''

--删除数据库用户
drop  user u001
--删除登录名
drop login u001
```

## 参考： 建立数据库登录账户

1. 首先在 SQL Server 服务器级 bai 别，创建登 du 陆帐户（create login）

```sql
create login dba with password='sqlstudy', default_database=mydb
```

登陆帐户名为：zhi“dba”，登陆密码：“sqlstudy”，默认连接到的数据库 dao：“mydb”。这时候，dba 帐户就可以连接到 SQL Server 服务器上了。但是此时还不能访问数据库中的对象（严格的说，此时 dba 帐户默认是 guest 数据库用户身份，可以访问 guest 能够访问的数据库对象）。
要使 dba 帐户能够在 mydb 数据库中访问自己需要的对象，需要在数据库 mydb 中建立一个“数据库用户”，赋予这个“数据库用户” 某些访问权限，并且把登陆帐户“dba” 和这个“数据库用户” 映射起来。习惯上，“数据库用户” 的名字和 “登陆帐户”的名字相同，即：“dba”。创建“数据库用户”和建立映射关系只需要一步即可完成：

2. 创建数据库用户（create user）：

```sql
create user dba for login dba with default_schema=dbo
```

并指定数据库用户“dba” 的默认 schema 是“dbo”。这意味着用户“dba” 在执行“select _ from t”，实际上执行的是 “select _ from dbo.t”。

3. 通过加入数据库角色，赋予数据库用户“dba”权限：

```sql
exec sp_addrolemember 'db_owner', 'dba'
```

此时，dba 就可以全权管理数据库 mydb 中的对象了。
如果想让 SQL Server 登陆帐户“dba”访问多个数据库，比如 mydb2。可以让 sa 执行下面的语句：

```sql
use mydb2
go
create user dba for login dba with default_schema=dbo
go
exec sp_addrolemember 'db_owner', 'dba'
go此时，dba 就可以有两个数据库 mydb, mydb2 的管理权限了！
```

4. 禁用、启用登陆帐户：

```sql
alter login dba disable
alter login dba enable
```

5. 登陆帐户改名：

```sql
alter login dba with name=dba_tom
```

提示：在 SQL Server 2005 中也可以给 sa 改名。 《SQL Server 2005 安全性增强：给超级用户 sa 改名》

6. 登陆帐户改密码：

```sql
alter login dba with password='sqlstudy.com'
```

7. 数据库用户改名：

```mssql
alter user dba with name=dba_tom
```

8. 更改数据库用户 defult_schema：

```sqlite
alter user dba with default_schema=sales
```

9. 删除数据库用户：

```sql
drop user dba
```

10. 删除 SQL Server 登陆帐户：

```sql
drop login dba
```
