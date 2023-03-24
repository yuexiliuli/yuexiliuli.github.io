---
title: SQL Server创建视图
urlname: ddvy7x
date: '2021-05-15 21:27:25 +0800'
tags:
  - 数据库
  - SQL
categories:
  - 数据库
  - SQL
---

# [SQL Server 创建视图](https://www.w3cschool.cn/sqlserver/sqlserver-xaz328n3.html)

从用户角度来看，一个视图是从一个特定的角度来查看数据库中的数据 。

从数据库系统内部来看，一个视图是由 SELECT 语句组成的查询定义的虚拟表（因为一个视图可以拉动多个表，并汇总数据在一起并将其显示，就好像它是一个单一的表）

视图是由一张或多张表中的数据组成的，当你运行视图，会看到它的结果，就像打开一个表时一样。

从数据库系统外部来看，视图就如同**一**张表一样，对表能够进行的一般操作都可以应用于视图，例如查询，插入，修改，删除操作等。

## SQL Server 视图的优点

视图可以执行以下操作：

- 限制访问特定的表中的行
- 限制访问特定的表中的列
- 从多个表中加入列，并呈现出来，好像他们是一个单一的表的一部分
- 呈现汇总的信息(如 COUNT 函数的结果)

## SQL Server 视图语法

通过使用 CREATE VIEW 语句创建一个视图，其次是 SELECT 语句，如下：

```sql
CREATE VIEW ViewName AS
SELECT ...
```

## SQL Server 创建视图

我们以前使用的查询设计器创建两个表中选择数据的查询。

现在让我们将查询保存为一个名为 “ToDoList” 的视图。

我们需要做的就是把 CREATE VIEW ToDoList 的 AS 查询，如下：

```sql
CREATE VIEW ToDoList AS
SELECT	Tasks.TaskName, Tasks.Description
FROM	Status INNER JOIN
			Tasks ON Status.StatusId = Tasks.StatusId
WHERE	(Status.StatusId = 1)
```

## SQL Server 运行视图

创建视图后，就可以简单地查看结果，就像你会选择任何表。

可以简单地键入 select \* from todolist，它会运行完整的查询，而不是输入出大量的 SELECT 语句的 INNER JOIN ：

*注：*也可以在视图上单击鼠标右键，并选择 "Select Top 1000 Rows".

## 数据更新

该视图将返回最新的数据。

如果表中的数据发生变化时，视图的结果会改变过；所以，如果要添加新任务以及状态 "To Do", 下一次运行来看，这将包括在结果集中的新纪录。

## 修改视图

通过使用 ALTER 修改现有的视图，而不是 CREATE。

如果我们想要更改视图就要使用 StatusName 字段，而不是 StatusId，做法如下：

```sql
ALTER VIEW ToDoList AS
SELECT	Tasks.TaskName, Tasks.Description
FROM	Status INNER JOIN
			Tasks ON Status.StatusId = Tasks.StatusId
WHERE	(Status.StatusName = 'To Do')
```

*注：*使用查询设计器也可以右键单击视图，然后选择设计来修改您的视图。

正如你所看到的，视图让您保存查询，以便可以做一个 SELECT，再次运行它也会比较简单。

但它们的确有其局限性：它们允许选择数据，但不允许执行任何业务逻辑，如条件语句等。

在下一节中我们通过学习[存储过程](https://www.w3cschool.cn/sqlserver/sqlserver-hw2328n6.html)来做到这点。
