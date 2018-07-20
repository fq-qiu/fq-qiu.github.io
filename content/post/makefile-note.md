---
title: "Make笔记"
date: 2018-07-20T13:00:46+08:00
tags: ["makefile"]
draft: false
categories: ["linux"]


---
##命名##
文件保存以`Makefile`或`makefile`为名.当在当前目录下直接输入`make`即可执行

如果不是默认的文件名, 可以使用`-f`参数
```
make -f make.some
```

##变量##
###变量基本用法

- 变量以`=`赋值
左侧是变量, 右侧是变量的值
```
objects = main.o kbd.o command.o display.o \
```
也可以使用变量来构造变量
```
A = $(B)
B = $(A)
```
如上所示, 会让make陷入无限的变量展开过程中, 报错. 故可使用下面的`:=`

- 变量以`$(objects)`取值
变量使用时要在前面加上`$`, 最好用`()`或者`{}`把变量括起来. 如果要使用真实的的`$`, 用`$$`

- 变量以`:=`定义
`=`有可能导致递归定义, 故使用该表达式, 前面的变量不使用后面的变量, 只使用已经定义好的变量

- 变量以`?=`定义
```
foo ?= bar
```
如果foo没有被定义过, 那么变量foo的值是bar, 否则, 这条语句什么也不做.

- 追加变量`+=`
如果变量之前没有定义过, `+=`自动变为`=`, 反之则继承于前次的操作的赋值符. 前次是`:=`, 那么此次为`:=`; 前次为`=`, 此次为`=`

- `override`指示符变量
用`override`指示符的变量, 在make命令行参数中可以设置, 而Makefile中对这个变量的赋值会被忽略
```
override <varriable> = <value>
override <varriable> := <value>
```
追加,
```
override <variable> += <more value>
```

- 多行变量
`define`指示符后面跟的是变量名称, 而重起一行定义变量的值, 变量可以换行, 定义以`endef`关键字结束. 变量的值可以包含函数,命令,文字或者其他变量

- 环境变量
make 运行时的系统环境变量可以在 make 开始运行时被载入到 Makefile 文件中,但是
如果 Makefile 中已定义了这个变量,或是这个变量由 make 命令行带入,那么系统的环
境变量的值将被覆盖。如果 make 指定了`-e`参数,那么,系统环境变量将覆盖
Makefile 中定义的变量


###变量高级用法###
- 变量值的替换
替换变量中的共有部分, 格式为`$(var:a=b)`或者`${var:a=b}`, 把var中所有以结尾的字串替换成以b结尾
如下
```
foo := a.o b.o c.o
bar := $(foo:.o=.c)
```
定义了\${foo}变量,把\${foo}中所有以.o结尾的子串全部替换为以.c结尾,$(bar)的值是a.c b.c c.c

- 把变量值再再当成变量
```
x = y
y = z
a := $($(x))
```
如上, \$(x)的值是y, \$(\$(x))就是\$(y), 那\$(a)就是z

##依赖规则##
###规则语法###
```
targets : prerequisites
    command
```
targes是文件名, 以空格分开, 可以使用通配符. 目标可以是一个文件, 也可能是多个文件. 

command是命令行, 如果不与`targets : prerequisites`一行, 必须以[Tagb键]开头, 如果在一行, 可用`;`分割, 如下
```
targets : prerequisites ; command
```

prerequisites是目标所依赖的文件(或者依赖目标), 如果有一个以上的文件比 target 文件要新的话,command 所定义的命令就会被执行

###伪目标###
伪目标的规则类似上面的规则, 但是伪目标不是文件, 只是一个标签, 所以make无法生成它的依赖关系和决定它是否执行, 只有通过显示指定这个目标才能让其生效.
比如, `make clean`科学可以执行下面的命令.
```
clean :
    rm *.o temp
```

为了避免和文件重命的情况, 使用特殊的额标记`.PHONY`来显示的指明一个伪目标, 不管是否有同名文件
```
.PHONY : clean
clean : 
    rm *.o temp
```

伪目标没有依赖文件 ,但是可以为伪目标指定所依赖的文件. **伪目标**可以作为**默认目标**
```
.PHONY : all cleanall cleanobj
all : prog1 prog2

prog1 : prog1.o utils.o
    cc -o prog1 prog1.o util.o

prog2 : proj2.o 
    cc -o prog2 prog2.o

cleanall : cleanobj cleandiff
    rm program

cleanobj :
    rm *.o

dleandiff : 
    rm *.diff
```
Makefile中的第一目标会被当做默认目标. `all`伪目标依赖于其他的三个目标

目标和伪目标都可以成为依赖.

如上, `make cleanall`, `make cleanobj`都可以显示伪目标执行命令

###多目标###
Makefile的规则中的目标可以不止一个,  其支持多目标. 当让, 有自动化变量`$@`吧表示目标的集合, 依次取出目标, 并执行命令.如下
```
bigoutput littleoutput : text.g
generate text.g $@ > $@
```

###静态模式定义的多目标###
语法如下
```
<targets...> : <target-pattern> : <prereq-pattern>
<commmands>
```
- targets 定义了一系列目标, 可以有通配符, 是目标的一个集合
- targets-pattern 指明了目标集模式, 从目标集中选出一部分目标
- prereq-pattern 是目标的依赖模式, 对target-pattern形成的模式再次进行依赖目标的定义, 对选出的目标进行再定义

For example.
```
objects = foo.o bar.o
all : $(objects)
$(objects) : %.o : %.c
$(CC) -c $< -o $@
```
从目标集`foo.o bar.o`中通过`%.o`的目标集模式选出之后, 通过`%.c`进行二次定义. 
- `%<`自动化变量, 表示所有的依赖目标, 用`%c`表示
- `%@`表示目标集, 即`%.o`



###规则中使用通配符###
同bash一样, 支持三种通配符
|符号|意义|
|:---:|:---|
|*|0到无穷多个任意字符|
|?|一个任意字符|
|[]|一个[]内的字符(非任意字符). 例如, [abcd], a,b,c,d中的任意一个字符|
|[-]|编码顺序内的字符. 例如, [0-9]代表0到9之间的所有数字|
|[^]|反向选择. 例如, [^abc]非a,b,c外的其他的任意一个字符|


##文件搜索##
###VPATH(大写)变量指定搜索目录###
make默认在当前目录搜索文件, 然后再到`VPATH`指定的目录去搜索.
```
VPATH = src:../headers
```
上面的定义指定两个目录,, 用`:`分割,  "src"和"../headers", make按照这个顺序进行搜索, 当然, 当前目录是最高优先级.

###vpath(小写)关键字设定搜索规则和路径###
三种使用方法
- vpath &lt;pattern&gt; &lt;directories&gt;
为符合模式&lt;patern&gt;的文件指定搜索目录&lt;directories&gt;

- vpath <pattern>
清楚设置的符合模式&lt;pattern&gt;的文件的搜索目录

- vpath
清楚所有已被设置好的文件的搜索目录

**`vpath`使用的&lt;pattern&gt;中需要包含`%`字符, 表示匹配零或者若干字符**
```
vpath %c foo:bar
vpath %c usr
vpath % blish
```
上面的语句表示`.c`结尾的文件, 依次搜索"foo", "bar", "user", "blish"目录

##条件判断##
`ifeq` 条件语句开始, `else`条件表达式为假, `endif`表示一个条件语句的结束.

- `ifeq`, 比较参数arg1和arg2是否相同,相同则为真
```
ifeq '<arg1>' '<arg2>'
ifeq "<arg1>" "<arg2>"
ifeq '<arg1>' "<arg2>"
ifeq "<arg1>" '<arg2>'
```

- 'ifneq`是否相同, 不同则为真
- `ifdef <variable-name>`判断变量的值非空, 则为真.只是测量变量是否有值, 并不会把变量扩展到当前位置

##函数##
函数调用以`$`开头, 以圆括号或花括号把函数名和参数括起.参数以`,`分隔, 函数名和参数之间以空格分隔;
```
$(<function> <arguments1,arguments2>) #小括号可换为大括号
```
####常用的字符串处理函数####
- `$(subst <from>,<to>,<text>)`
把子串&lt;text&gt;中的&lt;from&gt;字符串替换为&lt;to&gt;, 返回替换后的子串

- `(strip <string>)`
去掉&lt;string&gt;子串中开头和结尾的空字符,返回被去掉空格的字符串

- `(findstring <find>,<in>)`
查找字符串函数, 在子串&lt;in&gt;中查找&lt;find&gt;子串, 如果找到, 返回&lt;find&gt;, 否则返回空字符串

- `$(word <n>,<text>)`
取单词函数, 取字符串&lt;text&gt;中的第&lt;n&gt;的单词, 从1开始.返回子串&lt;text&gt;中的第&lt;n&gt;个单词. 如果&lt;n&gt;比&lt;text&gt;中的单词要大, 返回空字符串

- `$(wordlist <s>,<e>,<text>)`
取字符串函数
从字符串&lt;text&gt;中取&lt;s&gt;开始到&lt;e&gt;的单词串, &lt;s&gt;和&lt;e&gt;是一个数字



##特定字符##

`-`放在命令前, 无论出现什么错误, 继续执行


注释用`#`字符

命令以[Tab]键开始

`\`换行符, 类似c语言, 表示连接上一行


##Makefile与vim##
vim内置了`autocmd`, 所以可以使用`:make`, make当前目录`:pwd`下的Makefile
```
:make           #执行当前目录下的Makefile
```
Use the following commands to examine the compile errors with vim
```
:copen          #open a mini-window with list of errors, hit enter on an error to jump to line
:cclose         #close the mini-window
:cn[ext]          #jump to the next errror
:cp[revious]
```
具体用法, 可以在vim下`:h copen`和`:h cnext`

##Makefile经验总结##
- 整个Makefile文件都不要使用空格键最为安全，你看到的所有空格其实是有Tab键产生的，一旦使用了空格键，文件是默认空格是编译语言的其中一部分，从而会导致错误的出现。包括在编写注释的时候，也不要用空格
- 使用`-l`连接库模块时，不要直接写文件名，要把文件名中开头的lib去掉。例如，源文件是libopencv_core.so,在连接时使用-lopencv_core.
- 多个连接模块，使用空格分开。


**参考**: [陈皓的Makefile教程](http://download.csdn.net/detail/tainzhi/9131861)

