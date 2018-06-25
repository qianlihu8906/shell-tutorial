# shell 

## 历史
由于历史原因unix系统上有多种shell
* sh（Bourne Shell）：由Steve Bourne开发，各种UNIX系统都配有sh。

* csh（C Shell）：由Bill Joy开发，随BSD UNIX发布，它的流程控制语句很像C语言，支持很多Bourne Shell所不支持的功能：作业控制，命令历史，命令行编辑。

* ksh（Korn Shell）：由David Korn开发，向后兼容sh的功能，并且添加了csh引入的新功能，是目前很多UNIX系统标准配置的Shell，在这些系统上/bin/sh往往是指向/bin/ksh的符号链接。

* tcsh（TENEX C Shell）：是csh的增强版本，引入了命令补全等功能，在FreeBSD、Mac OS X等系统上替代了csh。

* bash（Bourne Again Shell）：由GNU开发的Shell，主要目标是与POSIX标准保持一致，同时兼顾对sh的兼容，bash从csh和ksh借鉴了很多功能，是各种Linux发行版标准配置的Shell，在Linux系统上/bin/sh往往是指向/bin/bash的符号链接[38]。虽然如此，bash和sh还是有很多不同的，一方面，bash扩展了一些命令和参数，另一方面，bash并不完全和sh兼容，有些行为并不一致，所以bash需要模拟sh的行为：当我们通过sh这个程序名启动bash时，bash可以假装自己是sh，不认扩展的命令，并且行为与sh保持一致。

*busybox提供ash作为默认shell。ash行为和ubuntu下/bin/sh行为一致*

## 启动shell

* 交互式启动shell
* 非交互启动

*主要注意环境变量的加载方式*

## shell命令的执行

### 命令
* 外部命令
	用户在命令行输入命令后，一般情况下Shell会fork并exec该命令
* 内建命令(man bash-builtins)
	执行内建命令相当于调用Shell进程中的一个函数，并不创建新的进程

*手动实现一个简单shell*
*cd为什么采用内建命令方式实现*

### 执行

* sh xxx.sh
* ./xxx.sh
* . xxx.sh

### 解释器的加载
*parser.sh*
```shell
#!/bin/bash
echo "$0" "$1"
```
*test.sh*
```shell
#!/tmp/parser.sh 
```

## 变量

### 环境变量和本地变量
* 环境变量
	从父进程传给子进程
* 本地变量
	只在当前shell进程使用
* example

*test.sh*
```shell
#!/bin/sh
echo $TOPDIR
```
*test1.sh*
```shell 
#!/bin/sh
TOPDIR=$(cd "$(dirname "$0")";pwd)
./test.sh
echo "export"
./test.sh
```

### 命令代换
由反引号括起来的也是一条命令，Shell先执行该命令，然后将输出结果立刻代换到当前命令行中。例如定义一个变量存放date命令的输出：

	$ DATE=`date`
	$ echo $DATE
命令代换也可以用$()表示：

	$ DATE=$(date)
*建议使用$()的形式，嵌套方便*

### 算术代换
用于算术计算，$(())中的Shell变量取值将转换成整数，例如：

	$ VAR=45
	$ echo $(($VAR+3))
*$(())中只能用四则运算和()运算符，并且只能做整数运算*

### 转义字符
* 单引号
	单引号用于保持引号内所有字符的字面值	
* 双引号	
	除变量名 前缀$ 后引号` 转移符号\外所有字符保持字面值

### 文件名通配符  
	* 
	?
	[adb]

### 位置参数和特殊变量
	$0 
	$@ 
	$? 
	$# 
	$$

## 条件测试
	* 命令 [] [[]] test
	* [ 是一个命令 所以需要空格隔开 ] 是[ 命令的一个参数，所以也需要空格隔开

## 分支控制

### if 
```shell
#! /bin/sh

echo "Is it morning? Please answer yes or no."
read YES_OR_NO
if [ "$YES_OR_NO" = "yes" ]; then
  echo "Good morning!"
elif [ "$YES_OR_NO" = "no" ]; then
  echo "Good afternoon!"
else
  echo "Sorry, $YES_OR_NO not recognized. Enter yes or no."
  exit 1
fi
exit 0
```
### case 

```shell
#! /bin/sh

echo "Is it morning? Please answer yes or no."
read YES_OR_NO
case "$YES_OR_NO" in
yes|y|Yes|YES)
  echo "Good Morning!";;
[nN]*)
  echo "Good Afternoon!";;
*)
  echo "Sorry, $YES_OR_NO not recognized. Enter yes or no."
  exit 1;;
esac
exit 0	
```

### 逻辑短路
	test "$(whoami)" != 'root' && (echo you are using a non-privileged account; exit 1)

## 循环

*test.sh*
```shell
#!/bin/bash

while [[ $tmp < 5 ]]
do
	echo $tmp
	let ++tmp
done

for i in {1..5}
do
	echo $i
done
for (( i=0;i<5;i++))  do
	echo $i
done 
```

## 调试方法

* set -x  set +x
* shellcheck

## main.sh
## ipmitool.sh

## 正则表达式
	man grep
