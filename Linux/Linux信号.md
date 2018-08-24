# Linux 信号

信号是一种信息，可以有**硬件**或**软件**产生。

信号通常会产生**中断行为**，即某些信号会中断CPU的执行，有些中断会促使系统进程调度，等

最常见的，也是最基本的调度为，**时间片**中断。

信号通常都有特定的宏定义的整型数，来表示不同类型的中断。

```bash
sig -l 
# kill 命令 列出所有中断
man 7 signal
# 手册查询signal的详细信息
```

***信号所代表的通信，是异步通讯，即随时没有任何事先协调的通信行为***

信号的使用场景主要在```程序的有序撤退```，即做服务器清理工作。

#### 信号种类和行为

信号处理的行为可以分为5类：

```c
// Term   Default action is to terminate the process. 终止当前进程
// Ign    Default action is to ignore the signal. 忽略信号
// Core   Default action is to terminate the process and  dump  core  
// 终止进程并产生core文件
// Stop   Default action is to stop the process.
// 暂停进程，使进程进入阻塞状态，类似于contrl+z，即进程的T状态
// Cont   Default  action  is  to  continue the process if it is
// currently stopped. 该信号是continue信号，与stop是相反的信号
```

常用信号：

```c
// SIGHUP        1       Term    Hangup detected on controlling terminal
//					or death of controlling process 
//								终止进程或死锁进程
// SIGINT        2       Term    Interrupt from keyboard
//								control+c 信号，键盘中断进程
// SIGQUIT       3       Core    Quit from keyboard 
//								control+\ 会产生core文件
// SIGILL        4       Core    Illegal Instruction 
//								指令非法信号（汇编常用）
// SIGABRT       6       Core    Abort signal from abort(3) 中间层常用
// 								在中间层软件内终止程序，一般用此信号
// SIGFPE        8       Core    Floating-point exception
//								浮点指针异常（例如，除以零。浮点寄存器发现除以零
//								除不尽，浮点寄存器指针就会越界，则就有硬件发送硬
//								中断，该硬中断引起SIGFPE软中断）
// SIGKILL       9       Term    Kill signal
//								强制终止进程，关机，就是kill -9 1，即终止init
// 								进程，该信号无法被捕捉。
// SIGSEGV      11       Core    Invalid memory reference
//								无效内存访问
// SIGPIPE      13       Term    Broken pipe: write to pipe with no
//								readers; see pipe(7)
//								管道读关闭，写端继续写管道
// SIGALRM      14       Term    Timer signal from alarm(2)
// 								睡眠信号，即通知进程进入S状态
// SIGTERM      15       Term    Termination signal
// SIGUSR1   30,10,16    Term    User-defined signal 1
// SIGUSR2   31,12,17    Term    User-defined signal 2
// SIGCHLD   20,17,18    Ign     Child stopped or terminated
//								子进程结束，它向父进程发送的信号
// SIGCONT   19,18,25    Cont    Continue if stopped
//								用于调试
// SIGSTOP   17,19,23    Stop    Stop process
//								用于调试
// SIGTSTP   18,20,24    Stop    Stop typed at terminal
//								用于调试
// SIGTTIN   21,21,26    Stop    Terminal input for background process
// 								用于调试
// SIGTTOU   22,22,27    Stop    Terminal output for background process
//								用于调试
```

### 信号源

进程的信号一般有三个来源

- 用户：用户执行contrl+C，contrl+\，这些信号会向系统内核发送信号；或者用户自定义的驱动程序定义的组合健位，向系统发送某信号

- 内核：内核产生的信号一般是由于时间片产生，也可以由于一些异常（例如，除以零，地址越界）产生。

- 进程：进程可以调用```kill```函数来向其他进程发送指定的信号

- ```c
  #include <sys/types.h>
  #include <signal.h>
  
  int kill(pid_t pid, int sig); // 向指定进程发送相应的signal
  ```

### 信号流

进程对信号的处理也有三种

- 默认处理，如果进程不对信号进行处理，信号默认会自动消亡，等价于执行```signal(SIGINT,SIG_DFL)```
- 忽略信号，进程可以用代码，显示的忽略某信号，等价于执行```signal(SIGINT,SIG_IGN)```
- 捕捉信号，进程可以事先注册信号处理函数，当进程接收到信号时，会调用相应的处理函数来处理信号。有两个信号不能被捕捉，即**SIGKILL**和**SIGSTOP**

### 信号处理函数

信号处理函数主要利用```signal 函数```来注册一个信号捕捉函数。

#### 信号注册函数signal

```c
#include <signal.h>

typedef void (*sighandler_t)(int); // 定义了一个函数指针类型，
sighandler_t signal(int signum, sighandler_t handler); // 设置信号捕捉函数，并传递一个函数指针，用来定义相应的处理行为。
```

#### 信号处理的例子：

```c
#include <stdio.h>
#include <signal.h>
    
void sig(int signum){	// 定义了一个捕捉函数
    printf("The %dth signal\n",signum);
}
int main(){

    if(signal(SIGINT,sig)==SIG_ERR){//捕捉函数启动，如果启动失败返回SIG_ERR
        perror("signal");
        return -1;
    }	// 至此，函数捕捉行为已经设定好了，
    //	因为信号是异步行为，所以设置好了之后就可以执行其他操作了
    while(1);	// 此处循环运行程序，等待捕捉信号
}
```

- **注意事项**：

  - 对与相同信号，相同信号不会打断自身的行为

  - ```c
    // 例如，在信号处理流程中，又来了新的相同的信号，则该信号不会被打断
    void sig(int signum){	// 定义了一个捕捉函数
        printf("BEFORE:The %dth signal\n",signum);
        sleep(3);  //处理流程中睡3秒，以等待新信号，看看sig函数是否能被打断
        printf("AFTER:The %dth signal\n",signum);
    }
    ```

    ```bash
    # 执行结果如下
    ^CBEFORE:The 2th signal
    ^C^C^C^C^C^C^CAFTER:The 2th signal
    BEFORE:The 2th signal
    AFTER:The 2th signal
    # 原理是，相同信号不能抢占本中断处理函数
    # 但是，相同的中断会重新置SIGINT中断位为激活状态，
    # 因而当该中断执行结束后，会再次执行一次该中断
    ```

  - 不同信号，可以打断信号执行流程（可以打断sleep行为，因为sleep也是由信号SIGALRM来实现的）

  - 综上所述，相同信号的信号处理流程（处理函数），不能重入，即不能调用自身

#### 信号忽略和恢复的例子：

信号忽略函数，只需要在signal函数里传入宏```SIG_IGN```，来启动信号捕捉函数

```c
signal(SIGINT,SIG_IGN);
// 如果进程执行到某一状态，需要恢复接收信号的状态，则可以用
signal(SIGINT,SIG_DFL); // 恢复正常SIGIN信号的处理
```

#### 高级信号注册函数sigaction

