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

共享内存使用注意事项：

***一定要控制好锁机制***，防止发生死锁或不同步的情况。

共享内存命令：

```bash
ipcs # 查看系统中共享内存信息的命令
ipcrm -m shmid # 删除共享内存，-m表示内存形式，shmid表示要删除的内存id
# 共享内存的删除，实现为‘标记删除’，即标记一段共享内存为待删除状态，只有当
# 没有任何挂载进程的时候，一个共享内存才能真正的被删除。
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
   // sizeof() 此处一般为4k的整数倍，因为开辟的内存都是以页为单位，
   // 开辟的少了会有浪费（碎片）
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

3. 通过对shmat返回的地址空间的读写，进程可以共享使用该内存。

#### 共享内存的属性操作

4. 通过共享内存控制函数shmctl来控制共享内存。

   ```c
   #include <sys/ipc.h>
   #include <sys/shm.h>
   
   int shmctl(int shmid, int cmd, struct shmid_ds *buf);
   // shmid参数，为shm的id，指名操作哪个共享内存
   // cmd参数，表示为要操作共享内存的命令参数
   // cmd参数有：
   // IPC_STAT命令参数，表示获取共享内存的状态信息
   // IPC_SET命令参数，表示设置共享内存状态
   // IPC_RMID命令参数，删除一段共享内存
   // buf参数，表示当要设置，或获取共享内存信息时，用户指定的传入传出结构体，
   // 当要删除该共享内存时，该参数可以是NULL
   ```

   - ***如果有进程连接该共享内存，则该共享内存不会被删除。***（标记删除）

5. shmid_ds状态信息结构体

6. ```c
   struct shmid_ds {
   struct ipc_perm shm_perm;    /* Ownership and permissions */
      size_t          shm_segsz;   /* Size of segment (bytes) */
      time_t          shm_atime;   /* Last attach time */
      time_t          shm_dtime;   /* Last detach time */
      time_t          shm_ctime;   /* Last change time */
      pid_t           shm_cpid;    /* PID of creator */
      pid_t           shm_lpid;    /* PID of last shmat(2)/shmdt(2) */
      shmatt_t        shm_nattch;  /* No. of current attaches */
      ...
   };
   
   struct ipc_perm {
          key_t          __key;    /* Key supplied to shmget(2) */
          uid_t          uid;      /* Effective UID of owner */
          gid_t          gid;      /* Effective GID of owner */
          uid_t          cuid;     /* Effective UID of creator */
          gid_t          cgid;     /* Effective GID of creator */
          unsigned short mode;     /* Permissions + SHM_DEST and
                                      SHM_LOCKED flags */
          // 创建共享内存的权限
          unsigned short __seq;    /* Sequence number */
      };
   ```

### IPC信号量

信号量是一种用于不同进程间，或是不同线程间实现**同步**的手段。信号量是一种**原语**，用于保护一段代码执行。在信号量的保护下，一段代码只可以被一个或n个进程/线程执行，即信号量的设计是用来阻止**并发**，并通过一系列设计逻辑，最终实现**同步通信**。

#### 两种信号量标准

- System V 标准信号量。信号量在内核中维护，常用与**进程间同步**
- Posix 有名信号量，基于**内存**控制的信号量，常用与**线程间同步**

#### System V 标准信号量

1. 创建一个信号量

2. ```c
   #include <sys/types.h>
   #include <sys/ipc.h>
   #include <sys/sem.h>
   
   int semget(key_t key, int nsems, int semflg);
   // key：一个创建信号量的key值，
   // nsems：number of semphores，创建的信号量集合中包含几个信号量
   // semflg：创建的信号量集合的权限以及创建属性
   // IPC_PRIVATE 表示私有信号量，用于父子进程间通信
   // IPC_CREAT 和 IPC_EXECL，用于创建信号量集合
   // 返回值：失败返回-1，成功返回信号量集合id
   key_t semid = semget((key_t)1234,1,IPC_CREAT|0600);
   // 创建了一个权限为0600的信号量集合，该集合中有一个信号量。
   
   
   ```

   ```bash
   #删除一个信号量
   ipcrm -s semid
   ```

3. 对信号量初始化

1. ```c
   #include <sys/types.h>
   #include <sys/ipc.h>
   #include <sys/sem.h>
   
   int semctl(int semid, int semnum, int cmd, ...);
   // 对信号量的状态进行设置，也就是对信号量初始化
   // semid：信号量集合的id
   // semnum：一个整数，表示操作该集合中第几个信号量，其实index为0
   // cmd：信号量操作命令如下：
   // IPC_RMID 命令参数：立即删除信号量集合，并唤醒所有阻塞进程
   // GETVAL 命令参数：获取某一信号量值
   // GETALL 命令参数：获取所有信号量值
   // SETVAL 命令参数：设置信号量的值
   // SETALL 命令参数：获取所有信号量的值
   // ... 命令参数：是一个联合体union，它根据cmd的不同，来改变
   // union semun {
   //      int              val;    /* Value for SETVAL */
   //      struct semid_ds *buf;    /* Buffer for IPC_STAT, IPC_SET */
   //      unsigned short  *array;  /* Array for GETALL, SETALL */
   //      struct seminfo  *__buf;  /* Buffer for IPC_INFO
   //                                   (Linux-specific) */
   //    };
   int ret = semctl(semid,0,SETALL,1);
   // 将semid信号量集合中第0个信号量，用SETALL命令，设置成值1.
   
   ```

3. 对信号量进行原子操作

   semop之所以能成为**原子**操作，是因为semop的实现遵循在操作系统级别遵循：

   1. 关中断，关抢占(全核关，或是单核关)
   2. 对信号量的值进行操作，P操作进行-1，V操作进行+1
   3. 开中断，开抢占（在中断关闭的中间，一定不能sleep，sleep就会出现starve）
   4. 开中断后，若经过信号量的操作后信号量小于0，则将本进程置于sleep状态

4. ```c
   #include <sys/types.h>
   #include <sys/ipc.h>
   #include <sys/sem.h>
   
   int semop(int semid, struct sembuf *sops, size_t nsops);
   // semid 表示要操作的信号量集合id
   // sembuf 表示操作的信息结构体
   //           unsigned short sem_num;  /* 操作的第几个信号量 */
   //           short          sem_op;   /* 信号量在一次操作中改变的值 */
   //           short          sem_flg;  /* 通常为SEM_UNDO表示		
   // 					程序结束，信号量变为调用前的值，最简单的
   //					理解为，如果程序在关中断期间，发生了崩溃并退出，系统会自动
   // 					将信号量设置为调用前的值，从而使得进程调度正常进行。
   // nsops 表示之前的结构体指针*sops 有包含几个结构体，
   // 这样semop就可以同时操作多个信号量。
   
   ```
