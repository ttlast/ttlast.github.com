---
title: linux 计算程序运行时间
tags:  后台,linux,clock,效率,gettimeofday
---


程序的运行效率很重要，其往往是评价程序优劣性的直接标准。程序运行效率的最简单方法就是计算程序的运行时间。

计算时间主要可以使用的函数有：clock()、time()、gettimeofday()。

```gettimeofday() 函数的精度最高，为微秒级别；clock() 函数的精度次之，为 10ms 级别；time() 函数的精度最低，为 1s 级别。```

## clock() 函数
clock() 函数是 ANSI C 的标准库函数，是 C/C++ 十分常用的计时函数，其声明定义在 time.h 头文件中,精度```10ms```：

模型：

	clock_t clock( void ); 

几点注意：

	它返回的是 CPU 耗费在本程序上的时间。也就是说，途中 sleep 的话，
	由于 CPU 资源被释放，那段时间将不被计算在内。因此，若函数中存在 sleep() 函数，
	则 sleep() 函数消耗的时间将不包含在内；

	得到的返回值其实就是耗费在本程序上的 CPU 时间片的数量，也就是 Clock Tick 的值。
	该值必须除以 CLOCKS_PER_SEC 这个宏值，才能最后得到以秒为单位的运行时间。
	在 POSIX 兼容系统中，CLOCKS_PER_SEC 的值为 1,000,000 的，也就是 1MHz。

	这个函数的精度不适很高，大约为 10ms，低于精度的程序全部输出 0ms。
	此函数适合用于计算一些大型程序，或循环程序。

## time() 函数
time() 函数来获得当前日历时间（Calendar Time）。所谓的日历时间就是用“从一个标准时间点（一般是1970年1月1日0时0分0秒）到此时的时间经过的秒数”来表示的时间。其原型为：

	time_t time(time_t * timer); 

```精度只有 1s```。

## gettimeofday() 
精度较高的函数，到```微秒``，原型为：

	#include<sys/time.h>
	int gettimeofday(struct timeval*tv, struct timezone *tz )

gettimeofday() 会把目前的时间用 tv 结构体返回，当地时区的信息则放到 tz 所指的结构中。其结构体定义为：

	
	struct  timeval {
   		long  tv_sec;/*秒*/
		long  tv_usec;/*微妙*/
	}；

	struct  timezone{
    	int tz_minuteswest;/*和greenwich 时间差了多少分钟*/
    	int tz_dsttime;/*type of DST correction*/
	};

使用例如：

	#include <stdio.h>
	#include <sys/time.h>
	int main()
	{
		struct  timeval  start;
   		struct  timeval  end;
   		unsigned long timer;

   		gettimeofday(&start,NULL);
   		printf("hello world!\n");
   		gettimeofday(&end,NULL);
   		timer = 1000000 * (end.tv_sec-start.tv_sec)+ end.tv_usec-start.tv_usec;
   		printf("timer = %ld us\n",timer);
   		return 0;
	}
