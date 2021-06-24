---
title: 已处理证书链,但是在不受信任提供程序信任的根证书中终止 - Windows 7安装.Net Framework 4.6.2时出现此问题
tags: "windows"
---

# 已处理证书链,但是在不受信任提供程序信任的根证书中终止 - Windows 7安装.Net Framework 4.6.2时出现此问题

在全新安装 Windows 7 SP1 后，通过离线包安装 .Net Framework 4.6.2时，遇到错误提示：已处理证书链，但是在不受信任提供程序信任的根证书中终止。 

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200708101316.png)

原因是计算机中没有相应的受信任证书，通过导入微软的证书，成功解决此问题。文末附安装包下载链接。

1.点击链接下载微软证书：http://download.microsoft.com/download/2/4/8/248D8A62-FCCD-475C-85E7-6ED59520FC0F/MicrosoftRootCertificateAuthority2011.cer

2.按 Windows徽标键+R 打开运行，输入MMC

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200708101343.png)

 3.打开控制台，文件→添加/删除管理单元 (Ctrl+M) 

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200708101458.png)

 4.选择证书 → 添加 → 计算机账户（其他的保持默认，一直下一步） 

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200708101352.png)

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200708101356.png)

 5.回到控制台主窗口，依次展开：证书 → 受信任的根证书颁发机构 → 证书，单击更多操作的小箭头，选择所有任务 → 导入；接下来选择步骤1中下载好的cer证书文件，然后一直点击下一步，导入成功即可。 

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200708101544.png)

![](https://gitee.com/yuexiliuli/MyMarkdownImage/raw/master/img/20200708101404.png)

6.此时重新安装 .Net Framework 4.6.2 则不会提示问题了。

[.Net Framework 4.6.2 离线安装包](https://www.lanzous.com/i9aitef)
[.Net Framework 4.6.2 在线安装包](https://www.lanzous.com/i9aitja)

原文：https://blog.csdn.net/inchat/article/details/104294302

