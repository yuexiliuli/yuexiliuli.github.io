---
title: 使用命令行编译java
tags: "Java"
---

# 使用命令行编译java

## 编译单个文件

### 在当前目录下的命令行输入：

```cmake
javac xxx.java
java xxx
```

编译成功，输出结果。

如果 **出现“编码GBK的不可映射字符”的错误**，原因是，系统默认的编码格式与源文件编码格式不同，编译时没有显式指定源文件编码格式的话，JDK会把源文件从系统默认编码格式转换为Java默认编码格式，这样就出现乱码了。 

### 解决命令：

```
javac -encoding utf-8 xxx.java
```

## [编译运行含package语句的类]( http://www.cnblogs.com/chanchan/p/7613261.html  )

### 文件如下：

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/1590397912104.png)

### 内容：

#### Point.java

```java
package com.company;

public class Point {
    private int X;
    private int Y;
......省略
```

#### TestPoint.java

```java
package com.company;
import com.company.Point;

public class TestPoint {

    public static void main(String[] args) {
......省略
```

### 使用命令：

```
javac -encoding utf-8  -d . Point.java
javac -encoding utf-8  -d . TestPoint.java
```

**参数"-d ."，表示在当前目录下生成包文件夹，并把类文件放到该文件夹下**；不加-d参数的话，在当前目录下能生成类文件，但运行时会提示找不到或无法加载类文件，原因在于，JVM要到包文件夹下寻找类文件，而此时只在当前目录下有类文件，这样就会出错。 

### 注意：[“Error:(1, 1) java: 非法字符: '\ufeff'”错误解决办法](https://www.cnblogs.com/ShaYeBlog/p/9755107.html)

#### 原因：

用Windows记事本打开并修改.java文件保存后重新编译运行项目出现“Error:(1, 1) java: 非法字符: '\ufeff'”错误 

原来这是因为Windows记事本在修改UTF-8文件时自作聪明地在文件开头添加BOM导致的，所以才会导致IDEA不能正确读取.java文件从而程序出错。 

#### 解决办法

在编辑器IDEA中将文件编码更改为UTF-16，再改回UTF-8即可，其实就相当于刷新了一下文件编码。

成功效果：

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/1590398816638.png)

### 然后运行TestPoint，在命令行输入命令：

```java
java com.company.TestPoint
```

成功效果：

```java
point2点的当前信息是：X坐标是：0
Y坐标是:0
point3点的当前信息是：X坐标是：1
。。。。。。省略
```

