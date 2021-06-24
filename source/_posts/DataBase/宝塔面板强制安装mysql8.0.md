---
title: 宝塔面板强制安装mysql8.0
tags: "MySQL"
---

# 宝塔面板强制安装mysql8.0

 mysql终于更新到8.0，mysql8.0对比以往的版本有了很大的提升，但是要求的服务器配置也就变得越来越高。对于低配置服务器，在宝塔面板进行安装时，总会出现“至少需要2个CPU核心才能安装”或者“至少需要XXX内存才能安装”。但我们又想要体验mysql8.0新版本，这时候该怎么办呢？只有强制在宝塔面板中安装mysql8.0。 



## 低配置服务器在宝塔面板中安装mysql8.0方法

远程连接到服务器中，进入/www/server/panel/install并执行下面代码。

编译安装mysql8.0，请在远程控制台中输入下面脚本：

```
wget http://download.bt.cn/install/0/mysql.sh；
bash mysql.sh install 5.7
```

 极速安装mysql8.0，请在远程控制台中输入下面脚本 

```
wget http://download.bt.cn/install/1/mysql.sh；
bash mysql.sh install 5.7
```

将5.7替换成你要安装的mysql版本。

请注意：如果你已经安装了数据库，上面的命令会卸载删除当前数据库及数据

这样低内存服务器在宝塔面板安装Mysql8.0就实现了！