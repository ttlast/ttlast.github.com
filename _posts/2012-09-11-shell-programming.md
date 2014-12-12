---
title: Shell 编程
tags: 后台,shell,linux,programming
---


## 变量（命名规则，和c/c++类似）：

num=2

注意点： 不要写成num = 2  这样无法识别变量。。～～

使用的时候，可以是： $num

在字符串中当变量后面连着字母什么的，可以用 echo "num is ${num}nd" 括号括起来。


shell中变量默认是`字符串赋值`	

	var=1

	var=$var+1

	echo $var   得到的结果是1+1，不是2

要达到目的可以这样：

	let "var+=1"

	var=$[$var+1]

	var=$(($var+1))

	var='expr $var+1'


## 流程控制

	if 语句

	if .........; then

		....

	elif .........; then

   		...

	else

  		....

	fi

一般都用括号[]括起来。`[] 前后要留空`

	[ -f "somefile" ] ：判断是否是一个文件

	[ -x "/bin/ls" ] ：判断/bin/ls是否存在并有可执行权限

	[ -n "$var" ] ：判断$var变量是否有值

	[ "$a" = "$b" ] ：判断$a和$b是否相等

## rename 命令使用

	可以用man rename查看用法,用法可以这样： 
	rename 's/***1/**2/' * 这样用正则表达式，替换所有符合***1的文件名为**2
	大小写替换可以用 rename 'y/A-Z/a-z/' *
