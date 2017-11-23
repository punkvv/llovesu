---
title: 在Windows2012搭建php7
date: 2017-10-25 10:33:42
categories: PHP
tags:
  - PHP
  - Windows
---

在 Windows2012 搭建 php7 环境时遇到的一些问题。

<!-- more -->

# 安装 [VC14][vc14]  


在安装 [VC14][vc14] 时：
  
<img src="/images/w-p-a.png" />

解决办法：
下载  [Windows Server 2012 R2 Update (KB2919355)][ws] 所有补丁。
首先运行下载文件中的：
```
clearcompressionflag.exe　
```
然后按以下顺序安装：　　
```
Windows8.1-KB2919355-x64.msu
Windows8.1-KB2932046-x64.msu
Windows8.1-KB2934018-x64.msu
Windows8.1-KB2937592-x64.msu
Windows8.1-KB2938439-x64.msu
Windows8.1-KB2959977-x64.msu
```
最后重启系统，重新安装 [VC14][vc14]。

# 启动 Nginx

  
启动 Nginx 时，提示80端口被 System 进程占用。

解决办法：
关闭 SqlServer 的 Reporting Service 服务


[vc14]: https://www.microsoft.com/en-us/download/details.aspx?id=48145
[ws]: https://www.microsoft.com/en-us/download/details.aspx?id=42334