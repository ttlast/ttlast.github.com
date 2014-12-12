---
title: Shell 命令
tags: 后台,shell,linux,命令
---

==========

<br \>

## 比较常用的命令
	ls 	(ls -hlrt) 列出文件
	ch dir 切换目录
	pwd 显示当前目录

	mkdir dir   创建目录 dir
	rm  file   (-r dir) (-f file ) (-rf dir)  删除 文件  （递归）目录    -f 强制删除
	cp file1 file2       cp -r dir1 dir2   复制***1 到 *** 2 
	mv file1 file2   移动文件
	ln -s file link    创建file的符号链接link
	chmod 755 ***  更改文件属性
	date   显示当前日期时间  cal 显示当月日历
	uname -a  显示内核信息  

## 安装软件
	1  ./configure      2. make    3. make install
	deb:   dpkg -i pkg.deb
	rpm：  rpm -Uvh pkg.rpm


## 进程管理
	ps -elf  | grep pattern   检索符合条件进程
	kill -9 pid 杀死pid进程
	调试
	strace -p pid -s100000 -tt -T -f -o filename 调试pid系统调用输出filename



## 搜索
	搜索文本内容中含有 pattern grep  
	grep pattern files
	grep -r pattern dir  递归搜索

	检索文件名符合pattern  find
	find . -name pattern
	

	
