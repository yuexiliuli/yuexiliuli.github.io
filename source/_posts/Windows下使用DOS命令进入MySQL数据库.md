---
title: Windows下使用DOS命令进入MySQL数据库
urlname: ymysue
date: '2021-05-15 21:27:54 +0800'
tags: []
categories: []
---

---

## title: Windows 下使用 DOS 命令进入 MySQL 数据库 tags: "SQL"

# [Windows 下使用 DOS 命令进入 MySQL 数据库](https://www.cnblogs.com/isme-zjh/p/11400722.html)

## 1、进入 cmd

桌面左下角 windows 图标--搜索框内输入**cmd**，点击**cmd.exe**，或者使用快捷键 Windows 键（在键盘上有个 Windows 标志的按键）+R 输入 cmd 后回车。

## 2、启动 mysql 数据库：

在出来的 DOS 命令窗口中输入

```shell
net start mysql
```

或者使用快捷键 Windows 键（在键盘上有个 Windows 标志的按键）+ R 直接输入**net start mysql**后回车。（另附：关闭的命令为**net stop mysql**）

## 3、进入 mysql 数据库

在 DOS 命令窗口输入

```shell
mysql -h localhost -u root -p
```

进入 mysql 数据库，

其中**-h**表示服务器名，localhost 表示本地；

**-u**为数据库用户名，root 是 mysql 默认用户名；

**-p**为密码，如果设置了密码，可直接在**-p**后链接输入，如：-**p123456**，用户没有设置密码，显示 Enter password 时，直接回车即可。

注意，如果你的 mysql 没有安装在 C 盘下，你需要先使用 DOS 命令进入 mysql 的安装目录下的 bin 目录中。以我的电脑为例，方法如下：输入**D:**进入 D 盘，在输入**cd D:\Tools\M**

## 4、成功进入

显示 welcome

## 5、操作数据库

输入**show databases；**显示你有的数据库（mysql 数据库中的命令必须以分号结尾“；”）

## 执行 sql 文件

执行 sql 文件，/usr/t_user_alpha.sql 路径是 mysql 客户端的路径，mysql 命令行中输入路径按 Tab 键是不会自动补全的，需要自己敲

```shell
MySQL [e_user]> source /usr/t_user_alpha.sql
```
