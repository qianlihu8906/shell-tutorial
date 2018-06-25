# 基本语法
	shell分多种，bash zsh.. 我们一般采用bash
	一条指令以换行或分号结束
	指令执行过程 一般都是 shell解析环境变量，exec 调用执行

	tmp="hello world" echo $tmp    #执行结果是什么
	tmp="hello world"; echo $tmp   #执行结果是什么

	shell变量
		变量访问要加$符号
		变量赋值=符号两边不能有空格

		tmp="hello world"   #变量赋值，注意没有$符号，=两边没有空格
		echo $tmp           #访问tmp变量，注意有$符号

		tmp="hello,"
		echo $tmpworld   
		echo "$tmp"world 
		echo ${tmp}world 
		echo '$tmp'world
		以上四句 打印结果是什么？shell做了什么，echo 命令做了什么
		trap 单引号
	分支逻辑，循坏逻辑
		tmp="hello"
	        if [ "$tmp"x  = "x" ];then
			echo ""
		fi

		tmp=1
		if [ $tmp -ne 0 ];then
			echo 
		fi

	        支持 if switch-case 分支逻辑
		支持 while  for do 循环逻辑
#小括号，双小括号，中括号，双中括号，大括号
小括号:
	命令组，subshell
大括号：
	当前shell执行，一组

双小括号
	数学运算环境
双中括号
	单中括号语法糖


传参
function foo()
{
	echo $1 $2 $3
	return 0  返回值只能是 0-255 255=-1
}

foo hello world   # $3 为空

if [ $? -ne 0 ];then
	echo ""
fi

or

arg1=
arg2=
arg3=

function foo()
{
	echo $arg1 $arg2 $arg3
	return 0
}

arg1=hello
arg2=world
#arg3=
foo 

if [ $? -ne 0 ];then
	echo ""
fi

错误处理，分支处理

例子，更新psu

	先判断主板厂商，如果一致可更新，否则退出，再判断主板型号，如果一致可更新，否则退出。
	再判断设备厂商，如果一致可更新，否则退出, 再判断设备型号，如果一致可更新，否则退出。

	if [ $board_vendor = "Inspur" ];then
		if [ $board_model = "SA5212H5" ];then
			if [ $device_vendor = "LITEON" ];then
				if [ $device_model = "PS-2801-12L" ];then
					echo "update ..."
					return 0
				fi
			fi
		fi
	fi

	return 1


	if [ $board_vendor != "Inspur" ];then
		return 1
	fi

	if [ $board_model != "SA5212H5" ];then
		return 1
	fi

	if [ $device_vendor != "LITEON" ];then
		return 1
	fi

	if [ $device_model != "PS-2801-12L" ];then
		return 1
	fi

	echo "update..."
	return 0
	

错误处理
	出错返回错误码
		需要接收，解析返回值，比较麻烦，但错误信息会归一到某模块下面，模块比较清晰
	出错直接退出
		实现简单，但需要时常记得处理日志信息




shell 数组

array1=(1 2 3 4 5)
tmp="1 2 3 4 5"
array2=($tmp)


echo "${array1[1]}"
echo "${array2[1]}"

echo $((array2[1]))


间接引用

tmp=1
echo $tmp
echo ${!tmp}


tmp=abc
abc=hello

echo $tmp
echo $abc
echo ${!tmp}

${!tmp}=world
echo $abc


#awk 用法


#shebang 

#!/bin/sh   过程

exec过程

环境变量和本地变量
set printenv
$(())中只能用+-*/和()运算符，并且只能做整数运算。

开关机　登录环境变量

正则表达式

#awk
