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
//								浮点指针异常（例如，除以零。浮点计数器发现除以零
//								除不尽，浮点计数器指针就会越界，则就有硬件发送硬
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

