---
title: Windows下使用DOS命令进入MySQL数据库
tags: "SQL"
---

# [Windows下使用DOS命令进入MySQL数据库](https://www.cnblogs.com/isme-zjh/p/11400722.html)

## 1、进入cmd

桌面左下角windows图标--搜索框内输入**cmd**，点击**cmd.exe**，或者使用快捷键Windows键（在键盘上有个Windows标志的按键）+R输入cmd后回车。

## 2、启动mysql数据库：

在出来的DOS命令窗口中输入 

```shell
net start mysql
```

或者使用快捷键Windows键（在键盘上有个Windows标志的按键）+ R直接输入**net start mysql**后回车。（另附：关闭的命令为**net stop mysql**）

##  3、进入mysql数据库

在DOS命令窗口输入 

```shell
mysql -h localhost -u root -p
```

进入mysql数据库，
其中**-h**表示服务器名，localhost表示本地；
**-u**为数据库用户名，root是mysql默认用户名；
**-p**为密码，如果设置了密码，可直接在**-p**后链接输入，如：-**p123456**，用户没有设置密码，显示Enter password时，直接回车即可。
注意，如果你的mysql没有安装在C盘下，你需要先使用DOS命令进入mysql的安装目录下的bin目录中。以我的电脑为例，方法如下：输入**D:**进入D盘，在输入**cd D:\Tools\M** 

## 4、成功进入

显示welcome 

## 5、操作数据库

输入**show databases；**显示你有的数据库（mysql数据库中的命令必须以分号结尾“；”） 

## 执行sql文件

执行sql文件，/usr/t_user_alpha.sql路径是mysql客户端的路径，mysql命令行中输入路径按Tab键是不会自动补全的，需要自己敲

```shell
MySQL [e_user]> source /usr/t_user_alpha.sql
```

