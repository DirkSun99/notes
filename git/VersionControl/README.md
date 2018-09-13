## 简介

### 版本控制

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

### 版本控制系统

* **集中式版本控制系统（Centralized Version Control Systems，简称 CVCS）**
  
	集中式版本控制系统有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连接到这台服务器，取出最新的文件或者提交更新。

	常见工具：CVS、**SVN**、VSS......

	![CVCS](http://petzxw2y2.bkt.clouddn.com/git/CVCS.png)

	缺点：单点故障。

* **分布式版本控制系统（Distributed Version Control System，简称DVCS）**

	分布式版本控制系统客户端不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来。这么一来，任何一处协同工作用的服务器发生故障，都可以用任何一个镜像出来的本地仓库恢复。因为每一次的克隆操作，实际上都是对代码仓库的完整备份。

	常见工具：**Git**、Mercurial、Bazaar、Darcs......

	![DVCS](http://petzxw2y2.bkt.clouddn.com/git/DVCS.jpg)