## 线程池

```mysql
package com.thread;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * 线程池
 * 线程池主要是解决，有些线程工作周期比较短。这些线程在进行很短时间的使用后，就退出撤销。
 * 这种频发的大量的，线程的创建和撤销，会造成性能的极大损耗。
 * 解决办法就是创建一个定义的线程池，并未其初始化一定量的原始线程。
 * 每次使用时传递线程执行函数到线程池中的某一个线程。让线程池中的某一个线程执行完一个传递上来的函数之后，在返回其池中。
 * 这样就不会出现线程的频繁创建和撤销产生的抖动现象
 ***********************************************************************
 * 线程池的基本实现：
 *      1. 定义一个集合|数列|Map等集合类
 *      2. 创建一定数量的线程，并放入这个数列里。
 *      3. 有程序需要一个线程，从线程池里面 POP 一个线程。传递执行函数，让其执行
 *      4. 线程执行完毕，归还该线程给这个集合类 append
 *      5. 如果线程全满，需要位置一个task在线程池上的等待队列 wait
 ***********************************************************************
 * JDK1.5 以后
 * java.util.concurrent.Executors
 * java.util.concurrent.ExecutorServices
 *
 * Executors 类util包下的线程池工厂类
 *      方法：
 *      1. static newFixedThreadPool  用于创建一个线程池。传递一个int参数，指定线程池初始化时线程个数
 * ExecutorsServices 线程池类
 *      对象方法：
 *      1. submit(Runnable task)  执行程序，传递一个实现了Runnable接口的对象实例，用来指明线程需要执行的内容
 *      2. void shutdown()        关闭整个线程池
 */
public class ThreadPool {
    public static void main(String[] args) {
        // 用线程池工厂类的静态方法，创建一个有5个线程的线程池
        ExecutorService es = Executors.newFixedThreadPool(5);
        for(int i = 0; i<10;i++){
            // 发起10次线程池调用
            // 每次将任务体，提交给线程池，线程池就会分配一个线程取执行
            es.submit(new TaskBody());
        }
        es.shutdown();  // 手动关闭线程池。如果不手动关闭，线程池会一直运行（即阻塞等待新的线程来）
    }
}

/**
 * 定义一个执行体类，只要实现 Runnable 接口，就可以传递给线程池用来执行
 */
class TaskBody implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+" 正在执行");
    }
}

/*
    执行结果
pool-1-thread-1 正在执行
pool-1-thread-4 正在执行
pool-1-thread-2 正在执行
pool-1-thread-3 正在执行
pool-1-thread-5 正在执行
pool-1-thread-5 正在执行
pool-1-thread-5 正在执行
pool-1-thread-1 正在执行
pool-1-thread-2 正在执行
pool-1-thread-4 正在执行
 */
```

