---
title: java使double保留两位小数的多方法
urlname: yi85r8
date: '2021-05-15 21:26:58 +0800'
tags: [Java]
categories: [Java]
---

# [java 使 double 保留两位小数的多方法 ](https://www.cnblogs.com/aipan/p/8022312.html)

推荐方式：Double.valueOf(String.format())

```java
    public static void main(String[] args) {
        double test1=10.2234;
        double test2=10.3356;

        Double dtest1 = Double.valueOf(String.format("%.2f", test1 ));

        Double dtest2 = Double.valueOf(String.format("%.2f", test2 ));

        System.out.println(dtest1 );
        System.out.println(dtest2 );
    }
```

结果如下：

```java
10.22
10.34
```

代码如下:

```java
DecimalFormat  df  = new DecimalFormat("######0.00");

double d1 = 3.23456
double d2 = 0.0;
double d3 = 2.0;
df.format(d1);
df.format(d2);
df.format(d3);
```

3 个结果分别为:

```java
3.23
0.00
2.00
```

## java 保留两位小数问题：

### 方式一：四舍五入

```java
double  f  =  111231.5585;
BigDecimal  b  =  new  BigDecimal(f);
double  f1  =  b.setScale(2,  BigDecimal.ROUND_HALF_UP).doubleValue();
```

保留两位小数

### 方式二：DecimalFormat

```java
new java.text.DecimalFormat("#.00").format(3.1415926)
```

```java
DecimalFormat df=new DecimalFormat(".##");
double d=1252.2563;
String st=df.format(d);
System.out.println(st);
```

#.00 表示两位小数 #.0000 四位小数 以此类推...

### 方式三：String.format

```java
double d = 3.1415926;
String result = String.format("%.2f");
```

%.2f %. 表示 小数点前任意位数  2 表示两位小数 格式后的结果为 f 表示浮点型

### 方式四：NumberFormat

```java
NumberFormat ddf1=NumberFormat.getNumberInstance() ;
void setMaximumFractionDigits(int digits)
```

digits 显示的数字位数

为格式化对象设定小数点后的显示的最多位,显示的最后位是舍入的

```java
class TT {
	public static void main(String args[]) {
    double x=23.5455;
    NumberFormat ddf1=NumberFormat.getNumberInstance() ;
	ddf1.setMaximumFractionDigits(2);
	String s= ddf1.format(x) ;
	System.out.print(s);
	}
}
```
