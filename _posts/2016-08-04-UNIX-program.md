---
layout:     post
title:      "UNIX学习笔记"
subtitle:   "程序控制流命令"
date:       2016-08-04 19:33:13
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- UNIX
---
## 读写shell变量
```
variable1 = value1 [variable2 = value2 ... variableN = valueN]
```

## 命令替换
将其输出替换为\`command\`。

```
`command`
```

## 导出环境
导出 ’name-list‘ 里的名称和当前值的拷贝到所有后继命令

```
export [name-list]
```

## 变量重置
重置或删除’name-list‘里指定的变量或函数，’name-list‘为由空格分隔的名称列表

```
unset [name-list]
```

## 创建用户自定义的制度变量
防止对'name-list'里指定的变量重新赋值

```
readonly [name-list]
```

## 从标准输入读
从标准输入读取一行并把此行中的单词依次赋给 ‘variable-list’中的变量

```
read [variable-list]
```

## 向shell脚本传递参数

把个位置参数的值一次设为‘argument-list’里指定测参数
可以传送9个参数的值给shell脚本，前9个参数的值分别被存储在变量**$1**到**$9**中。
**$#**的值为执行脚本时传递给脚本的参数个数，
**$**和**$@**都包含所有参数的值，但使用**$@**时，表示每个参数各自由引号包围起来，
**$0**的值为脚本文件名（命令名称）。

```
set [option] [argument-list]
```


## if-then-elif-else-fi语句

### 实现两路或多路分支

```
if expression
		then
			[elif expression
			then
				then-command-list]
			...
			[else
				else-command-list]
fi
```

### 评估表达式 ‘expression’并返回真或假的状态
两种写法：

```
test [expression]
	if test $# -eq 0
		then ...
[[expression]]
	if [ $# -eq 0 ]
		then ...
```

### 用于test命令的运算符：

1. 文件测试
	- -d file : 'file' 为目录时为真
	- -f file : 'file' 为普通文件时为真
	- -r file : 'file' 可读时为真
	- -s file : 'file' 长度大于0时为真
	- -t [filedes] : 文件描述符'filedes'与终端相连时为真
	- -w file : 'file'可写时为真
	- -x file : 'file' 可执行时为真
2. 整数测试
	- int1 -eq int2 : 'int1' 等于 'int2' 时为真
	- int1 -ge int2 : 'int1' 大于或等于 'int2' 时为真
	- int1 -gt int2 : 'int1' 大于 'int2' 时为真
	- int1 -le int2 : 'int1' 小于或等于 'int2' 时为真
	- int1 -lt int2 : 'int1' 小于 'int2' 时为真
	- int1 -ne int2 : 'int1' 不等于 'int2' 时为真
3. 字符串测试
	- str : 'str' 为非空字符串时为真
	- str1 = str2 : 'str1'和'str2'相同时为真
	- str1 != str2 : 'str1'和'str2'不相同时为真
	- -n str : 'str' 长度大于0时为真
	- -z str : 'str' 长度等于0时为真
	
### 构成复杂表达式的运算符
1. ! 逻辑非
2. -a 逻辑与
3. ('expression') 圆括号用于把表达式分组；每个括号前后至少应有一个空格
4. -o 逻辑或
	
## for语句
重复执行‘command-list’里的命令多次，执行次数与‘argument-list’里的单词个数一样多。方括号内为可选部分，如果不带看我宣布分，参数可在命令行里提供。

```
for variable [in argument-list]
do
	command-list
done
```
## while语句

```
while expression
do
	command-list
done
```

## until语句

**只要’expression‘ 为假，就执行’command-list‘**

```
until expression
do
	command-list
done
```

## case语句
```
case test-string in 
	pattern1) command-list1
	        ;;
	pattern2) command-list2
	        ;;
	...
	patternN) command-listN
	        ;;
esac
```
