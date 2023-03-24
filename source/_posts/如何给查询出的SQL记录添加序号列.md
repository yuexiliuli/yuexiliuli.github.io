---
title: 如何给查询出的SQL记录添加序号列
urlname: hepgd0
date: '2021-05-15 21:28:34 +0800'
tags: []
categories: []
---

---

## title: 如何给查询出的 SQL 记录添加序号列 tags: "SQL"

# 如何给查询出的 SQL 记录添加序号列

给查询出的 SQL 记录添加序号列，解决方法有以下两种

第一：

```sql
  select ROW_NUMBER() OVER (ORDER BY a.字段 ASC) AS XUHAO,a.* from table a
```

(table 为表名，字段为表 a 中的字段名)

第二：

```sql
 select RANK()  OVER (ORDER BY a.字段 ASC) AS XUHAO,a.* from table a
```

(table 为表名，字段为表 a 中的字段名)
