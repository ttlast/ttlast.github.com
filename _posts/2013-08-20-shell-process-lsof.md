---
title: Shell 进程和文件
tags: 后台,shell,linux,lsof,pid
---


参考： [IBM lsof](http://www.ibm.com/developerworks/cn/aix/library/au-lsof.html)

在Unix环境中，文件无处不在。lsof 可以帮助查看进程使用的文件等

## lsof简介
```因为 lsof 需要访问核心内存和各种文件，所以必须以 root 用户的身份运行它才能够充分地发挥其功能。```
### lsof 输出清单
	bash-3.00# lsof 
	COMMAND    PID   USER   FD   TYPE        DEVICE SIZE/OFF      NODE NAME
	sched        0   root  cwd   VDIR         136,8     1024         2 /
	init         1   root  cwd   VDIR         136,8     1024         2 /
	init         1   root  txt   VREG         136,8    49016      1655 /sbin/init
	init         1   root  txt   VREG         136,8    51084      3185 /lib/libuutil.so.1
	vi        2013   root    3u  VREG         136,8        0      8501 /var/tmp/ExXDaO7d
	...

### lsof 查看进程打开文件
要指定单个进程，可以使用 -p 参数，后面加上该进程的 PID。因为这样做不仅会返回该应用程序所打开的文件，还会返回共享库和代码，所以通常需要对输出进行筛选。要完成此任务，可以使用 -d 标志根据 FD 列进行筛选，使用 -a 标志表示两个参数都必须满足 (AND)。如果没有 -a 标志，缺省的情况是显示匹配任何一个参数 (OR) 的文件。清单 2 显示了 sendmail 进程打开的文件，并使用 txt 对这些文件进行筛选。

事例：
#### 带有 PID 筛选器并进行 txt 文件描述符筛选的 lsof 输出

	sh-3.00# lsof -a -p 605 -d ^txt
	COMMAND  PID USER   FD   TYPE  DEVICE SIZE/OFF     NODE NAME
	sendmail 605 root  cwd   VDIR  136,8     1024    23554 /var/spool/mqueue
	sendmail 605 root    0r  VCHR  13,2            6815752 /devices/pseudo/mm@0:null
	sendmail 605 root    1w  VCHR  13,2            6815752 /devices/pseudo/mm@0:null
	sendmail 605 root    2w  VCHR  13,2            6815752 /devices/pseudo/mm@0:null
	sendmail 605 root    3r  DOOR             0t0       58
		/var/run/name_service_door(door to nscd[81]) (FA:->0x30002b156c0)
	sendmail 605 root    4w  VCHR  21,0           11010052 
						/devices/pseudo/log@0:conslog->LOG
	sendmail 605 root    5u  IPv4 0x300010ea640      0t0      TCP *:smtp (LISTEN)
	sendmail 605 root    6u  IPv6 0x3000431c180      0t0      TCP *:smtp (LISTEN)
	sendmail 605 root    7u  IPv4 0x300046d39c0      0t0      TCP *:submission (LISTEN)
	sendmail 605 root    8wW VREG         281,3       32  8778600 /var/run/sendmail.pid

### 查找打开某端口进程
	# lsof -i :25
	COMMAND  PID USER   FD   TYPE        DEVICE SIZE/OFF NODE NAME
	sendmail 605 root    5u  IPv4 0x300010ea640      0t0  TCP *:smtp (LISTEN)
	sendmail 605 root    6u  IPv6 0x3000431c180      0t0  TCP *:smtp (LISTEN)


## 不常用用法：

### 检索活动连接
	# lsof -i @192.168.1.10
	COMMAND  PID USER   FD   TYPE        DEVICE  SIZE/OFF NODE NAME
	sshd    1934 root    6u  IPv6 0x300046d21c0 0t1303608  TCP sun:ssh->linux:40379
							 (ESTABLISHED)
	sshd    1937 root    4u  IPv6 0x300046d21c0 0t1303608  TCP sun:ssh->linux:40379
							 (ESTABLISHED)

### 回复删除文件
当进程打开了某个文件时，只要该进程保持打开该文件，即使将其删除，它依然存在于磁盘中。这意味着，进程并不知道文件已经被删除，它仍然可以向打开该文件时提供给它的文件描述符进行读取和写入。除了该进程之外，这个文件是不可见的，因为已经删除了其相应的目录条目。

比如，查找某被删除的日志文件： error_log

	# lsof | grep error_log
	httpd      2452     root    2w      REG       33,2      499    3090660
					/var/log/httpd/error_log (deleted)
	httpd      2452     root    7w      REG       33,2      499    3090660
					/var/log/httpd/error_log (deleted)
	... more httpd processes ...

获取到打开文件的进程id等，在/proc中查找，就可以获取文件

	# cat /proc/2452/fd/7
	[Sun Apr 30 04:02:48 2006] [notice] Digest: generating secret for digest authentication
	[Sun Apr 30 04:02:48 2006] [notice] Digest: done
	[Sun Apr 30 04:02:48 2006] [notice] LDAP: Built with OpenLDAP LDAP SDK



