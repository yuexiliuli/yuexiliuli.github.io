---
title: Javadoc文档注释及命令生成api文档
urlname: bxfqys
date: '2021-05-15 21:26:53 +0800'
tags: []
categories: []
---

---

## title: Javadoc 文档注释及命令生成 api 文档 tags: "Java"

# Javadoc 文档注释

## 一、Javadoc 中的 Tag 说明

## Tag 的一些惯例

### 一些 Tags 的说明

- **[@author ](/author) **：作者的姓名
- **[@version ](/version) **：版本号
- **[@param ](/param) **：对于参数的描述
- **[@return ](/return) **：对于返回内容的描述
- **[@exception ](/exception) ** (和**[@throws ](/throws) **是同义词)：异常类的名称和描述
- **[@see ](/see) **：表示去查看参考资料
- **[@since ](/since) **：表示这个变更或特性从什么时候或版本号等（由该标签中声明的内容决定）开始存在的
- **[@serial ](/serial) **：用于表示序列化的字段（include | exclude）
- **[@deprecated ](/deprecated) **：表示被弃用
- **[@link ](/link) **：用法{[@link ](/link) \*_ \_package.class#member label_\_}。插入一个带标签的链接，可以指向特定包、类或指定类的成员名称的文档。
- **[@literal ](/literal) **：用法{[@literal ](/literal) \*_ \_text_\_}。用来显示那些不用被 HTML 标记或嵌套 javadoc 标签解析的文本。

### Tags 的顺序

按照下面的顺序写 tags：

- **[@author ](/author) ** (仅用于类和接口，必须的)
- **[@version ](/version) ** (仅用于类和接口，必须的)
- **[@param ](/param) ** (仅用于方法和构造函数)
- **[@return ](/return) ** (仅用于方法)
- **[@exception ](/exception) ** (和**[@throws ](/throws) **是同义词)
- **[@see ](/see) **
- **[@since ](/since) **
- **[@serial ](/serial) ** (或者**[@serialField ](/serialField) **，**[@serialData ](/serialData) **)
- **[@deprecated ](/deprecated) **

### 对多个相同 tags 的排序

按照下面的惯例来使用多个相同 tags。如果需要，可以用空白行（单\*）来隔离这些包含多个相同 tags 的组。

- 多个**[@author ](/author) **标签，应该按照发生时间顺序排列，最早的发在最上面。
- 多个**[@param ](/param) **标签，应该按照参数的声明顺序排列。
- 多个**[@throws ](/throws) **标签（或**[@exception ](/exception) **），应该按照异常名称的字母顺序排列。

### 必须的 Tags

按照惯例，每一个参数都应该有一个**[@param ](/param) **标签，即使描述很明显。每一个返回不是*void*的方法都应该有一个**[@return ](/return) **标签，即使这个标签和方法的描述内容重复。

参考资料

[How to Write Doc Comments for the Javadoc Tool](https://link.jianshu.com/?t=http%3A%2F%2Fwww.oracle.com%2Ftechnetwork%2Fjava%2Fjavase%2Ftech%2Findex-137868.html)

菜鸟：[Java 9 改进 Javadoc](https://www.runoob.com/java/java9-improved-javadocs.html)

菜鸟：[Java 文档注释](https://www.runoob.com/java/java-documentation.html)

# 二、javadoc 命令生成 api 文档

## 百度经验的生成：

```shell
javadoc -d javaapi -header 测试的API -doctitle 这是我的第一个文档注释 -version -author javadoc/Hello.java 进行文档生成。
```

## 我的：对 Main.java 生成 Api 文档：

```shell
javadoc -d javaapi -header test -doctitle test1 -version -author Main.java
```

**-d:文件存储位置;**

**-head:文件头部名称; **

**-version:显示版本;**

**-author:显示作者;**

**-javadoc/Hello.java 处理的文件包以及 java 源文件.\***

## 查看生成的 api 文件

创建成功之后，就会自动创建指定的文件夹下生成 api 文件。打开 index.html 就是 api 文件的入口，可查看自己编写的代码生成的 api 文档。

![20210318231917.png](https://272y4n7101.goho.co/i/2023/03/22/641adb7536c62.png)
