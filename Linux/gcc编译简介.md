# gcc简单使用

### C或C++程序的编译执行过程

<div align="middle">

<img src="./images/bianyi.png" width=400>

</div>

**编译工具链** = 预处理器+编译器+汇编工具

#### 正常编译流程

1. 预处理

   ```bash
   gcc -E main.c -o test.i
   # gcc -E 参数，即执行预处理操作
   # 结果一般保存为.i后缀的文件
   # -o 参数，为对输出命名
   # 执行结果 main.c 中的 define，include 等预操作都被替换
   ```

   ```c
   #define debug  //调试开关
   int main(){
   #ifdef
       printf("lalala");
   #endif
       return 0;
   }
   // 此处执行了预编译后，若ifdef成立，则printf()代码会出现在.i文件中
   ```

   - 预编译替换```define```，不替换```typedef```。
   - \#ifdef，#ifndef，#endif

2. 编译

   - 编译成汇编源文件

     ```bash
     gcc -S main.i -o main.s
     # gcc 通过编译预处理后的文件生成.s文件
     # .s文件是汇编源码文件，是文本文件，里面包含许多汇编指令。
     ```

     

   - 汇编成目标文件

     ```bash
     as main.s -o main.o
     # 用as命令将汇编文件生成二进制目标文件，
     # 目标文件中留有链接库的地址
     # 目标文件无法用vim等文本解析器查看
     # 查看目标文件需要用nm命令
     nm main.o
     # 该函数可以查看目标函数的链接
     ```

3. 链接

   ```bash
   gcc main.o -o main
   # gcc 链接main.o和库从而生成main 可执行程序
   ```

   

