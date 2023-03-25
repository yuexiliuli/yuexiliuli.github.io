---
title: Windows如何查看某个端口被谁占用
urlname: vz06eb
date: '2021-05-15 21:27:49 +0800'
tags: [windows]
categories: [windows]
---

# Windows 如何查看某个端口被谁占用

1、开始---->运行---->cmd，

或者是 window+R 组合键，调出命令窗口

2、 输入命令：

```
netstat -ano
```

列出所有端口的情况。在列表中我们观察被占用的端口，比如是 49157，首先找到它。

3、查看被占用端口对应的 PID，

输入命令：

```
netstat -aon|findstr "49157"
```

回车，记下最后一位数字，即 PID,这里是 2720。

4、 继续输入

```
tasklist|findstr "2720"
```

回车，查看是哪个进程或者程序占用了 2720 端口，结果是：svchost.exe

5、结束该进程：在任务管理器中选中该进程点击”结束进程“按钮，或者是在 cmd 的命令窗口中输入：

```
taskkill /f /t /im Tencentdl.exe。
```
