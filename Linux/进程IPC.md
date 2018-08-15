# 进程间通信

进程间通讯进程IPC可以有多种方式

最简单的方式为：

#### 管道通讯

管道通讯指，两个进程间的通讯通过共同访问管道文件（虚拟内存文件）来进行通讯。通讯以流的方式进行。工作状态为半双工。

1. 方式可以通过双方直接打开一个**管道文件**来实现
2. 可以通过**popen**标准库函数直接建立进程管道通讯，该方式省略了建立管道文件这一步骤。

```c
#include <stdio.h>
FILE *popen(const char *command, const char *type);
// command参数：表示打开另一个程序（进程）的命令
// type参数：表示打开的进程只可以是r或w；若是r，则该进程将要打开
// 另外一个进程用于提供数据给该进程，即从另外进程那面度数据。
// w则相反。
int pclose(FILE *stream);
```

popen 新创立的用于通讯的新进程，更改了STDOUT，即更改了标准输出，他将标准输出重定向到管道



#### 无名管道

无名管道只能在**亲属进程**中通讯

主要手段就是利用函数 ```pipe```创建一个管道的输入输出后，通过父子进程分别继承相应的输入或输出端口使用。

***通过pipe创建的无名管道，不会进行写时复制，即无名管道不会被复制两份***

```c
#include <stdio.h> 
#include <string.h> 
#include <unistd.h> 
int main() {

    int fds[2] = {0}; 
    pipe(fds); 
    char szBuf[32] = {'\0'}; 
    if(fork() == 0){ //表示子进程
        close(fds[1]); //子进程关闭写
        sleep(2); //确保父进程有时间关闭读，并且往管道中写内容
        if(read(fds[0], szBuf, sizeof(szBuf)) > 0)
            puts(buf);
        close(fds[0]); //子关闭读 
        exit(0); 
    }else{ //表示父进程
        close(fds[0]); //父关闭读 
        write(fds[1], "hello", 6); 
        waitpid(-1, NULL, 0); 
        //write(fds[1], "world",6);
        close(fds[1]); 
        //父关闭写 exit(0);
    } 
    return 0;
    //等子关闭读 //此时将会出现“断开的管道”因为子的读已经关闭了
}
```

进程读写管道的关闭是有先后顺序的。若写端先关闭，读端就会读到0。若读端关闭，写端再写，写端就会收到**SIGPIPE**信号，若程序不对该改信号进行处理，则程序会崩溃，如果写端处理了该信号，则写端write会返回-1。

#### 有名管道

有名管道见文件章节。有名管道主要是在系统中存在一个**管道文件**，进程双方通过对有名管道或读，或写的方式打开，来进行通讯。

**管道文件**在标准C中的创建和删除如下：

```c
#include <sys/types.h>
#include <sys/stat.h>

int mkfifo(const char *pathname, mode_t mode); //创建管道文件，mode一般为666

#include <unistd.h>
int unlink(const char *pathname);	// 删除一个文件
// 不仅是管道文件，所有非目录文件都可以
```



#### 共享内存通讯IPC

先在流行的共享内存的方式遵从```System V```标准。```IPC```是inter-process communication的简写。

不同于```mmap（将文件映射到内存，来加速文件的读写）```，共享内存是多个进程间开辟一段公共内存空间，用于多个进程同时对该内存进行读写，从而实现进程间通信。

基本原理：

系统开辟一段物理内存，系统将此共享内存**映射到每个进程的进程地址空间去**（一般该区域为堆区），因而不同进程操其地址空间中的该内存段时，其操作就会映射为对该共享空间的操作。

共享内存命令：

```bash
ipcs # 查看系统中共享内存信息的命令
ipcrm -m shmid # 删除共享内存，-m表示内存形式，shmid表示要删除的内存id
```

共享内存编程：

1. 首先先建立一段共享内存，每一段共享内存都有其id，且该段内存不依赖某一个进程而存在，shm表示shared memory

   ```c
   #include <sys/ipc.h>
    #include <sys/shm.h>
   
   int shmget(key_t key, size_t size, int shmflg);	
   // key为系统中唯一存在的共享内存的编号
   // size为共享内存大小
   // shmflg为共享内存的一些属性标志，以掩码形式存在
   // 如果是创建共享内存掩码为 IPC_CREAT，不存在则创建，存在则直接拿取共享内存
   // 如果在创建共享内存时，添加上IPC_EXCL，若共享内存存在，则返回创建失败
   // 写法为：
   int ret = shmget((key_t)1234,sizeof(char)*20,IPC_CREAT|0600);
   // 后面的|0600表示该共享内存的权限为该用户可读可写
   
   if(ret==-1){
       return -1; // 共享内存开辟失败返回-1
   }
   key_t shmid = ret; // 开辟成功，则返回共享内存的id，即shmid
   ```

   此处若**key**参数填写的是IPC_PRIVATE，也就是0（注意是key），则创建出来的共享内存为私有内存，其返回的的key为0x000000。从而其他进程无法介入该共享内存。该种共享内存只能用于父子进程通讯，即通过fork，子进程继承了该共享内存的id。

2. 链接共享内存和虚拟地址空间（堆空间）

   ```c
   #include <sys/types.h>
   #include <sys/shm.h>
   
   void *shmat(int shmid, const void *shmaddr, int shmflg); //attach
   // 链接共享内存和虚拟地址空间
   // shmid 为创建好的共享内存id，该共享内存不能为PRIVATE
   // shmaddr 为虚拟内存空间的首地址，一般填写NULL，表示由系统自动分配
   // shmflg 仍是权限，若开辟共享内存时已经指定权限，该参数无用，即填写0即可
   // 成功返回首地址，失败返回void * 形式的-1
   
    int shmdt(const void *shmaddr);
   // 解除链接，输入虚拟地址空间的首地址，解除进程与共享内存的链接
   ```

   