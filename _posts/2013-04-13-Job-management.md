---
title: Shell 作业管理
tags: shell,linux,jobs,后台
---

## 作业管理：&、jobs、fg、bg、kill介绍

### &： 将命令放在后台执行
用法可以如下：

	tar -zpcf /tmp/tar.gz /etc &

要把当前作业放到后台执行，也可以用Ctrl + z，当我们在编辑文件的时候，突然有事情，那么可以用Ctrl+z，把作业暂停在后台，状态是Stopped，这样回到bash状态。

### jobs： 观察当前后台作业状态
	用法:
		jobs -l

### fg： 把后台作业拿到前台处理
	使用：fg %jobnumber （其中的jobnumber使用 jobs -l查看）

### bg： 让作业在后台运行
	使用：bg %jobnumber


### kill： 终止作业命令
	使用： kill -signal %jobnumber
	-9 强制删除一个作业
	-15 以正常程序方式终止一个作业

	kill -9 %1 等等 kill -9 pid


### ps:  后台挂起运行daemon

	nohup /shellcommand   &


### 杀死连续进程

	kill -9 `seq 2000 2010`
