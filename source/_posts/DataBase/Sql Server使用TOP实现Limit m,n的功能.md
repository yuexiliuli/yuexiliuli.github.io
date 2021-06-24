---
title: Sql Server使用TOP实现Limit m,n的功能
tags: "SQL"
---

# Sql Server使用TOP实现Limit m,n的功能

在MySQL中，可以用 Limit 来查询第 m 列到第 n 列的记录，例如：

```sql
select * from [tablename] limit [m], [n]
```

但是，在SQL Server中，不支持 Limit 语句。怎么办呢？
解决方案：
虽然SQL Server不支持 Limit ，但是它支持 TOP。

例如，如果要查询上述结果中前6条记录，则相应的SQL语句是：

```sql
select top 6 [columnNames..] from [tablename]
```


如果要查询上述结果中第 7 条到第 9 条记录，则相应的SQL语句是：

```sql
select top 3 [columnNames..] from [tablename]
where [columnNames..] not in (
    select top 6 [columnNames..] from [tablename]
    )
```


取第m条到第n条记录：

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


