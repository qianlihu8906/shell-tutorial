# 这是一个shell基础教程

##基本语法

一条shell指令以换行符或分号结束。执行过程为：
*shell解析整条指令（处理基本逻辑，展开变量）,调用exec函数执行对应指令*

##变量
变量的赋值,与访问
```shell
tmp="hello world" #变量赋值时，变量前无$符号，=两边没有空格
echo "$tmp"       #访问变量时，需要在变量前添加$符号
```



#分支

#循坏