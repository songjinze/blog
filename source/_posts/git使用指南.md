---
title: git使用指南
date: 2018-07-19 13:27:57
tags: git
categories: 版本控制
---

# Git使用指南

此文章直接介绍git通用的基本命令，对于git的历史发展和原理在其他文章中介绍。

## Git基本命令

话不多说，先通过Linux终端的截图有大致的了解。

{% asset_img git基本命令.png git--help %}  

这里的命令不全，若对于某个命令有疑问可以查询文档，或直接输入<code>git help <命令></code>来查寻

### clone

克隆一个仓库到一个新目录。  
用法为  
<code>git clone [<选项>] [--] <仓库> [<路径>]</code>  
例如我要从“https://github.com/songjinze/hello-world.git”上clone下来（注意网址是以.git结束的），那么命令可以写为
<code>git clone https://github.com/songjinze/hello-world.git</code>  
便会从源端仓库clone下来一个项目。此时本地仓库
的源端仓库已经与clone的网址相匹配，不需要重新配置。

### init

创建一个空的Git仓库或重新初始化一个已存在的仓库。  
此时初始化了一个本地仓库，已经可以使用Git进行本地的版本控制。但此时本地的仓库还没有对应一个远端仓库，需要重新配置。

### add

添加文件内容至索引。  
比如现在要对文件test1、test2加入追踪，命令为  
<code>git add test1 test2</code>  
当热，也可以使用  
<code>git add .</code>  
将目录下所有文件以及子文件都加入追踪。

### mv

移动或重命名一个文件、目录或符号链接。  
需要注意的是，在移动或重命名之前需要先提交已更改的内容，并且源文件要存在并且可修改，而目标文件的目录也要存在。  
在成功完成之后，git对于文件的索引也会自动更改。

**其他的不想写了，想学的看官方文档，乖。**



