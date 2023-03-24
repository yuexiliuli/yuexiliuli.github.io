---
title: 'Sql Server使用TOP实现Limit m,n的功能'
urlname: rsixmg
date: '2021-05-15 21:27:34 +0800'
tags:
  - 数据库
  - SQL
categories:
  - 数据库
  - SQL
---

在 MySQL 中，可以用 Limit 来查询第 m 列到第 n 列的记录，例如：

```sql
select * from [tablename] limit [m], [n]
```

但是，在 SQL Server 中，不支持 Limit 语句。怎么办呢？
解决方案：
虽然 SQL Server 不支持 Limit ，但是它支持 TOP。

例如，如果要查询上述结果中前 6 条记录，则相应的 SQL 语句是：

```sql
select top 6 [columnNames..] from [tablename]
```

如果要查询上述结果中第 7 条到第 9 条记录，则相应的 SQL 语句是：

```sql
select top 3 [columnNames..] from [tablename]
where [columnNames..] not in (
    select top 6 [columnNames..] from [tablename]
    )
```

取第 m 条到第 n 条记录：

```sql
select top (n-m+1) [columnNames..] from [tablename]
where [columnNames..] not in (
    select top (m-1) [columnNames..] from [tablename]
    )
```

或者：

```sql
select top @pageSize [columnNames..] from [tablename]
where [columnNames..] not in (
    select top @offset [columnNames..] from [tablename]
    )
```
