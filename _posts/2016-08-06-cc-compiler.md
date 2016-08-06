---
layout:     post
title:      "UNIX学习笔记"
subtitle:   "编译c程序"
date:       2016-08-06 12:01:45 
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- UNIX
- Compile
- cc Compiler
---

 **源代码->编译程序->汇编代码->汇编程序->目标代码->连接程序->可执行代码**

```
cc [options] filr-list
```

|               Command        |Description|
|-----------------------|---------------------|
|     -c |  不进行连接阶段，保留目标文件（扩展名为.o）     |        
| -fast |  选择最优的编译选项组合以加快速度     |
|   -g|  创建符号表、开编文件和调试信息     |  
|  -o  output-file  |在'output-file'中建立可执行代码，代替默认的a.out     |  
| -S | 不对.c文件进行汇编或连接，只将汇编版本保存在形影的以.s为扩展名的文件中   |  
| -O[level] |对执行时间进行优化，'level'参数可以是1、2、3或4。通常情况下，数字越大，优化级越高，默认值是2|      

编译一个小文件 1.c:

```c
#include <stdio.h>
main()
{
	printf("UNIX Rules thr Networking World!\n");
}
```
```
cc l.c
```
生成a.out文件,执行文件：
```
./a.out
```

**注：此处ubuntu上实测以./a.out的方式执行，据说UNIX上是直接输入a.out即可，未验证。**

执行
```
cc -o slogan 1.c
```
生成名为slogan的可执行文件,
```
./slogan
```
运行即可。

1. 处理多个源文件

	```
	cc draver.c stack.c misc.c -o polish
	```
	
	如果修改了其中一个源文件，就要重新进入整个命令行，但是这会导致两个问题：
	1. 虽然只有一个文件需要重新编译，但是三个文件都会编译到他们的目标模块中，使得编译时间变长，特别是在文件很大的时候；
	2. 重新键入命令行在只有三个文件时不是一个大问题，但如果要处理很多文件就是个大麻烦
	
    因此，应为每个源文件都创建目标模块，然后编译那些更改过的模块，所有的模块再连接在一起生成一个可执行文件。
    要为c源文件创建目标模块可以使用带-c选项的cc命令，在使用-c选项编译时，编译器会在当前目录生成一个目标文件，这个文件与源文件相同，并以.o为扩展名，于是可以再使用cc命令将多个目标文件连接在一起。例如：
    
    ``` 
    $ cc -c driver.c
    $ cc -c stack.c
    $ cc -c misc.c
    $ cc misc.o stack.o driver.o -o polish
    $ polish  
    [output of the program]
    $ 	
	```
	
	还可以使用-c选项来编译多个文件。在下面的第一个命令行中只用一个命令就编译了所有三个源文件并生成了目标文件，编译器在编译时会将每个文件的名字显示出来。在命令行中文件列出的次序并不重要，第二个命令行讲三个目标文件连接起来生成一个可执行文件。
	
	```
	$ cc -c driver.c stack.c misc
	driver.c:
	stack.c:
	misc.c:
	$ cc misc.o stack.o driver.o -o polish
	$
	```
	
	如果更改了源文件中的一个，就只需要使用cc -c 命令来生成这个源文件的目标文件，然后将所有的目标文件连接起来生成可执行文件（使用第二个cc命令）。
2. 连接库
	绝大多数UNIX系统上的C编译器会在编译程序的时候将正确的库连接到程序上，然而有些时候必须显式地告诉编译器必须连接什么库。要做到这一点可以使用带**-l**选项的cc命令，并在后面紧跟着库名字中在字符串lib之后，扩展名之前的字母。大多数的库在目录/lib中，对每一个需要连接的库需要使用一个单独的-l选项。在下面的会话中，将要吧数学库（/lib/libm.a）连接到power.c文件中程序的目标代码上，第一个cc命令表明如果没有连接数学库编译器就会显示一个错误信息，消息说符号pow没有在文件power.o中找到。数学库的名字是libm.a，所以在-l选项后紧跟字母（在字符串lib之后，在扩展名之前）。
	
	```c
	$ cat power.c
	
	#inlcude <math.h>
	#include <stdio.h>
	
	main()
	{
		float x,y;
		
		printf("The program takes x and y from stdin and displays x^y.\n");
		printf("Enter integer x: ");
		scanf("%f", &x);
		printf("Enter integer y: ");
		scanf("%f", &y);
		printf("x^y is: %6.3f\n", pow((double)x,(double)y));
	}
	
	$ cc power.c -o power
	
	Undefined           first referenced
	 symbol                in file
	 pow                    power.o
	 ld: fatal:Symbol referencing errors. No output written to power
	 
	 $ cc power.c -lm -o power
	 $ power
	 
	 The program takes x and y from stdin and displays x^y. 
	 Enter integer x: 9.82
	 Enter integer y: 2.3
	 x^y is: 191.362
	 
	 $
 	```
 	
 	下面的命令行将/lib/libsoket.a和/lib/libnsl.a两个库连接大client.c文件中程序的目标代码上，生成的可执行代码在文件client中。
 	
 	```
 	cc client.c -o client -lsocket -lnsl
 	```
 	
 	cc 命令行中选项的次序在大多数系统中都没什么关系，这时
 	```
 	cc  -o client client.c -lnsl -lsocket 
   ```
   与上面的命令行等价，但有些C编译器要求连接选项必须在命令行的最后。