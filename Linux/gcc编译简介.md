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

   - **预编译替换**```define```，不替换```typedef```。
   - \#ifdef，#ifndef，#endif

2. 编译

   ```bash
   
   ```

3. 链接

