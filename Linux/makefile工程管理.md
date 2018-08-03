# makefile工程管理工具

makefile 是自动化编译工具链常用的工具，主要用于自动化编译c，c++程序。

makefile 自动化工具主要用处是，实现**增量编译**。

增量编译的原理：**根据修改时间**来确定文件是不是最新版本。

**makefile，主要是对比.c文件和对应的.o文件的修改时间，若工具发现.c文件的更新时间比.o晚，即发现.c文件最近被修改了，则工具就会自动化编译对应的.o。其他没有被修改的.c文件不进行编译。最后增量编译之后再进行链接**

### makefile 文件格式

基本格式：

```makefile
# 该文件名为：Makefile
目标项：依赖项
<Tab>匹配后执行的命令  
```

例如：

```makefile
main:main.c  # 目标文件:依赖文件
	gcc main.c -o main	# 执行操作
```

```makefile
main: main.o func.o func1.o   # 依次依赖
	gcc func.o func1.o -o main
func.o:func.c
	gcc func.c -c 
func1.o:func1.c
	gcc func1.c -c
```

### makefile 变量

变量**定义**如下:

​	变量名:=变量值

变量**取用**如下：

​	$(变量名)

```makefile
ELF:=main	# 定义变量
OBJS:=main.o func.o
$(ELF):$(OBJS)	# 定义makefile目标依赖
	gcc -c $(OBJS) -o $(ELF)
```

#### makefile 隐式规则

makefile隐式规则指，makefile内部定义好的默认规则，即

```makefile
main:=main.o
	gcc main.o -o main
# 该规则省略了如下
# main.o:main.c
#	gcc -c main.c
# 即该规则缺省了上面的语句，但是makefile有一个自带的默认规则
# 例如上面编译.o，就默认缺省了:
# cc -c -o main.o main.c 
# 因而只用写上面最开始的一句就可以
# 该处的cc 就是makefile 预定义的变量，cc实际指默认系统自带的编译器
```

#### makefile 模式规则

模式规则，通过匹配模式来匹配字符串

- %匹配1-n个字符，%.c即匹配所有.c文件

#### makefile 函数

- wildcard函数，搜索所有当前目录下匹配的文件名

- patsubst函数，替换文件名，主要用于筛选出文件名的首部如main.c筛选出main。格式为```patsubst 原格式 新格式 替换字串来源```，该处是makefile内部函数参数，因而用%代表所有

- ```makefile
  SOURCE:=$(wildcard *.cpp)  #注意，此处*匹配所有字符串，因为该处是系统匹配，所以用*代表所有。该处匹配所有cpp文件
  # 该语句的意思就是把当前文件夹下所有.cpp文件名复制给变量SOURCE
  OBJS:=$(patsubst %.cpp %.o $(SOURCE)) #这样就把所有文件名首部提取出
  # 并添加上.o后缀来
  ```

#### makefile 预定义变量

- AR：库文件打包默认为ar，改变预定义标志为ARFLAGS
- AS：汇编程序默认为as，改变预定义汇编标志为ASFLAGS
- CC：c编译器默认为cc，改变预定义汇编标志为CFLAGS
- CPP：c预编译器默认为$(CC) -E，改变预定义c编译器CPPFLAGS
- CXX：c++编译器默认为g++，改变c++预定义编译器CXXFLAGS
- RM：删除默认为rm -f

```makefile
# 如上面所示，默认缺省会添加，预编译cc，以及相应的编译命令：
# cc -c -o main.o main.c 
# 但是如果修改预编译标志CFLAGS
CFLAGS:=-Wall
# 则编译结果就为：
# cc -Wall -c -o main.o main.c
# 即预定义变量就是，向隐藏规则中添加指定的编译参数
```

例如：

```makefile
CC:=gcc			# 改变隐式的编译器
CFLAGS:=-g -Wall # 改变预定义变量
main:main.o		# 主命令
	gcc -o main main.o
# 这样隐式编译就为
# gcc -g -Wall -o main.o main.c
```

#### makefile 自动变量

自动变量是对于每一个匹配规则（包括，目标项，依赖想，对应执行操作代码），可以自动转义自动变量为相应的变量

- $@：替换目标项目

- ```makefile
  ELF:=main
  OBJS:=main.o
  $(ELF):$(OBJS)	#一条匹配规则开始，ELF为目标项
  	gcc $(OBJS) -o $@	# $@ 自动替换成ELF目标项
  ```

- $^：替换所有依赖文件，用法同上

#### makefile伪命令：

```makefile
.PHONY:clean # 定一个伪命令 clean
clean:		# 定义clean 的具体内容
	rm -rf main.o func1.o
# 伪命令的作用，即没有直接的依赖项的命令
# 伪命令可以通过make的参数调用执行
```

```bash
# 执行时就调用
make clean  # 该命令直接会删除main.o和func1.o
```

### 级联实现makefile多级编译

对于一个大型工程，在文件目录的每一个层级都添加一个Makefile文件，即多层级目录结构。通过**级联传递make命令的方式，实现整个工程的编译**。