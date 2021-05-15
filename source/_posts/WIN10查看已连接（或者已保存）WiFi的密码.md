---
title: WIN10查看已连接（或者已保存）WiFi的密码
tags: "windows"
---

# WIN10查看已连接（或者已保存）WiFi的密码

## 一、普通的查看已连接WiFi方法

1.电脑的右下角的WiFi的图标，右击它，选择“打开网络和共享中心”

2.点击“更改适配器设置”

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200712091422.png)

 3.双击“WLAN” 

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200712091441.png)

 4.点击“无线属性”

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200712091457.png)

 5.点击“安全” 

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200712091554.png)

 6.勾选“显示字符” 

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200712091515.png)

## 二、另一种查看所有保存的WiFi密码方法：

1.打开cmd
按键盘windows+R，输入cmd，然后enter
2.输入命令：
netsh wlan show profile

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200712091622.png)

3.输入命令：

netsh wlan export profile name=“所选wifi名称” folder=. key=clear

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200712091629.png)

 4.输入命令：
.\WLAN-所选wifi名称.xml 

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200712091636.png)

 5.系统会以记事本打开保存的文件
``内的字段就是密码 

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200712091642.png)

## 第三种：直接在cmd显示密码

```shell
netsh wlan show profiles +wifi名字 +key=clear
```