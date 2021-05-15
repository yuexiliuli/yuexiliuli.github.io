---
title: Javadoc文档注释及命令生成api文档
tags: "Java"
---

# Javadoc文档注释

## 一、Javadoc中的Tag说明

## Tag的一些惯例

### 一些Tags的说明

- **@author**：作者的姓名
- **@version**：版本号
- **@param**：对于参数的描述
- **@return**：对于返回内容的描述
- **@exception** (和**@throws**是同义词)：异常类的名称和描述
- **@see**：表示去查看参考资料
- **@since**：表示这个变更或特性从什么时候或版本号等（由该标签中声明的内容决定）开始存在的
- **@serial**：用于表示序列化的字段（include | exclude）
- **@deprecated**：表示被弃用
- **@link**：用法{@link ***package.class#member label\***}。插入一个带标签的链接，可以指向特定包、类或指定类的成员名称的文档。
- **@literal**：用法{@literal ***text\***}。用来显示那些不用被HTML标记或嵌套javadoc标签解析的文本。

### Tags的顺序

按照下面的顺序写tags：

- **@author** (仅用于类和接口，必须的)
- **@version** (仅用于类和接口，必须的)
- **@param** (仅用于方法和构造函数)
- **@return** (仅用于方法)
- **@exception** (和**@throws**是同义词)
- **@see**
- **@since**
- **@serial** (或者**@serialField**，**@serialData**)
- **@deprecated**

### 对多个相同tags的排序

按照下面的惯例来使用多个相同tags。如果需要，可以用空白行（单*）来隔离这些包含多个相同tags的组。

- 多个**@author**标签，应该按照发生时间顺序排列，最早的发在最上面。
- 多个**@param**标签，应该按照参数的声明顺序排列。
- 多个**@throws**标签（或**@exception**），应该按照异常名称的字母顺序排列。

### 必须的Tags

按照惯例，每一个参数都应该有一个**@param**标签，即使描述很明显。每一个返回不是*void*的方法都应该有一个**@return**标签，即使这个标签和方法的描述内容重复。

参考资料

[How to Write Doc Comments for the Javadoc Tool](https://link.jianshu.com/?t=http%3A%2F%2Fwww.oracle.com%2Ftechnetwork%2Fjava%2Fjavase%2Ftech%2Findex-137868.html)

菜鸟：[Java 9 改进 Javadoc](https://www.runoob.com/java/java9-improved-javadocs.html)

菜鸟：[Java 文档注释](https://www.runoob.com/java/java-documentation.html)

# 二、javadoc命令生成api文档

## 百度经验的生成：

```shell
javadoc -d javaapi -header 测试的API -doctitle 这是我的第一个文档注释 -version -author javadoc/Hello.java 进行文档生成。
```

## 我的：对Main.java生成Api文档：

```shell
javadoc -d javaapi -header test -doctitle test1 -version -author Main.java
```

 **-d:文件存储位置;**  

 **-head:文件头部名称; **

**-version:显示版本;**

**-author:显示作者;** 

**-javadoc/Hello.java 处理的文件包以及java源文件.***



## 查看生成的api文件

创建成功之后，就会自动创建指定的文件夹下生成api文件。打开index.html就是api文件的入口，可查看自己编写的代码生成的api文档。 

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20210318231917.png)

