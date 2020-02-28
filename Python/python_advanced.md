# Python 高阶


### Python网络

#### UDP
- 发送

    ```python
    # *-* coding:utf-8 *-*
    import socket
    
    def main():
        # 创建udp套接字
      	udp_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
      
      	# 准备发送的目的地址，ip地址 和 端口号 的元组
        dest_addr = ("192.68.1.123",8080)
        
        # 准备发送数据的内容
        send_data = "这是我udp发送的数据"
        
        # 执行发送命令
        udp_socket.sendto(send_data.encode("utf-8"),dest_addr)
        
        # 关闭socket套接字
        udp_socket.close()
      
    if __name__ == "__main__":
      	main()
    ```

- 接受

    ```python
    #coding=utf-8
    import socket
    
    def main():
        # 创建udp套接字
      	udp_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
      
      	# 准备接受地址，ip空表示本地地址localhost
        recev_addr = ("",8080)
        
        # 绑定socket地址
        udp_socket.bind(recev_addr)
        
        # 执行接受命令，会阻塞
        recev_data = udp_socket.recvfrom(1024)  #1024表示一次接受可接受的，最大字节数
        
        # 显示接受到的数据
        print(recev_data[0].decode("utf-8") #之所以写[0]，是因为接受到的信息，还包含对方套接字的信息 
        
        # 关闭socket套接字
        udp_socket.close()
      
    if __name__ == "__main__":
      	main()
    ```



#### TCP

- 客户端

    ```python
    import socket
    
    # 创建socket
    tcp_client_socket = socket.socket(AF_INET,socket.SOCKET_STREAM)
    
    # 获取服务器的socket信息
    server_ip = "192.168.1.42"
    server_port = 8080
    
    # 链接服务器，客户端自身一般不绑定bind某个端口，由操作系统随机分配
    tcp_client_socket.connet((server_ip,server_port))
    
    # 发送数据
    tcp_client_socket.send("来自客户的问候".encode("utf-8"))
    
    # 接受服务器的回复数据
    recv_data = tcp_client_socket.recv(1024)
    
    # 输出接受到的数据
    print(recv_data.decode("utf-8"))
    
    # 关闭socket
    tcp_client_socket.close()
    ```

    

- 服务端

    ```python
    import socket
    
    # 创建socket
    tcp_server_socket = socket.socket(AF_INET,socket.SOCKET_STREAM)
    
    # 本地socket
    tcp_server_addr = ("",8080)   # 服务器端需要绑定端口
    
    # 绑定端口
    tcp_server_socket.bind(tcp_server_addr)
    
    # 启动监听，进入阻塞状态
    tcp_server_socket.listen(128)  # 128表示最多监听多少个客户端链接
    
    # 启动监听后进入循环接受
    while True:
        # 获得客户端链接信息
        client_socket, client_socket_addr = tcp_server_socket.accept()  # 接受成功，获得元组
    
        # 用接受成功的链接，收发数据
        recv_data = client_socket.recv(1024)  # 收
        print(recv_data.decode("utf-8"))
    
        client_socket.send("你好我是服务器".encode("utf-8")) # 发
    
        # 关闭某个客户端链接
        client_socket.close()
    
    # 关闭tcp服务器端socket
    tcp_server_socket.close()
    exit()
    ```

    



### 线程

1. 线程的创建和执行

    ```python
    import threading
    import time
    
    def sing(temp):
      print(temp)  # 打印子线程传递过来的参数
      while True:
     	 		print("singing")
      		time.sleep(1) #每隔1秒唱歌
      
    def dance():
      while True:
          print("dancing")
          time.sleep(1)  #每隔1秒跳舞
          
    g_num = 100.  #全局变量   
    def main():
      t1 = threading.Thread(target=sing, args=(g_num))  # args是一个元组，表示专递给子线程的参数
      t2 = threading.Thread(target=dance)   # target 表示 子线程执行函数的入口函数地址
      t1.start()
      t2.start()
      print(threading.enmuerate())  #返回当前正在执行的线程列表
    if __name__ == "__main__":
        main()
    ```

    or 自定义类方法继承Thread类

    ```python
    import threading
    import time
    
    class MyThread(threading.Thread):
      """自定义的线程类，继承Thread类，并重写run方法。重写run方法等价于Thread(target=xxx)，中的target参数"""
        def run(self):     
            while True:
            		print("childThread is doing something")
                time.sleep(1)
                
    if __name__ == "__main__":
        t = MyThread()
        t.start()
               
    ```

2. 线程的锁机制

    ```python
    import threading
    
    g_num = 0
    def testLock():
        global g_num  # 声明全局变量
        for i in range(1000000): # 循环100万次
            g_num += 1 
            
    def main():
        t1 = threading.Thread(target=testLock)
        t2 = threading.Thread(target=testLock)
        t1.start()
        t2.start()
    
    if __name__ == "__main__":
        main()      # 调用主线程，启动两个子线程，各对全局共享变量g_num加100万，并输出一一
        print("final num = "+str(g_num))
        
        
        """结果不是200万"""
    ```

    加互斥锁

    ```python
    import threading
    import time
    
    g_num = 0
    
    # 上锁三部曲 
    # 1. 定义锁
    mutex = threading.Lock()
    def testLock():
        global g_num  # 声明全局变量
        for i in range(1000000): # 循环100万次
            mutex.acquire()   # 2.加锁
            g_num += 1 
            mutex.release()   # 3.解锁
            
    def main():
        t1 = threading.Thread(target=testLock)
        t2 = threading.Thread(target=testLock)
        t1.start()
        t2.start()
    
        
        
        
    if __name__ == "__main__":
        main()      # 调用主线程，启动两个子线程，各对全局共享变量g_num加100万，并输出一一
        time.sleep(5)  # 等5秒，让main函数线程彻底执行完毕
        print(threading.enumerate())
        print("final num = "+str(g_num))
        
        
        """结果是200万"""
    ```

    

### 进程

- 类似于线程

    ```python
    import multiprocessing
    import time
    
    def sing():
      while True:
        print("sing")
        time.sleep(1)
      
    def dance():
      while True:
          print("dancing")
          time.sleep(1)  #每隔1秒跳舞
          
    g_num = 100.  #全局变量   
    def main():
      t1 = multiprocessing.Process(target=sing)  # args是一个元组，表示专递给子进程的参数
      t2 = multiprocessing.Process(target=dance)   # target 表示 子进程执行函数的入口函数地址
      t1.start()
      t2.start()
    if __name__ == "__main__":
        main()
    ```

- 进程池

    ```python
    from multiprocessing import Pool
    import time,random,os
    # 导入进程池类
    
    def worker(msg):
        t_start = time.time()
        print("我是进程:%d -%s 开始执行" % (msg, os.getpid()))
        time.sleep(random.random()*2)  #让进程睡眠
        print(msg,"执行完毕，用时%.2f" % (time.time()-t_start))
    
    def main():
        po = Pool(3)  #创建一个进程池，里面至多3可放个进程
        for i in range(10):
            po.apply_async(worker,(i,))  #异步创建10个编号不同的进程，将进程放入进程池中
            # 此处的意思是，向进程池中添加了10个进程，但是进程池最多开通3个进程给他们并行使用。其他7个则等待进程池
            # 分配进程。已经获得进程池中的进程当运行完毕后，释放进程让出进程允许其他worker进入调度。
        print("--------pool start---------")
        po.close()   # 关闭进程池
        po.join()    # 等待子进程结束，必须放在close之后。因为pool进程创建了3个子进程，则需要等待
        # 默认情况下主进程会自动等待子进程执行完毕才推出。但是pool进程是一个子进程，他没有主进程的这种默认属性
        # 主进程不会等待一个由pool子进程拉起来的子孙进程。因而会关闭，从而需要写po.join()
        print("---------pool end----------")
    if __name__ == "__main__":
        main()
        
    ''' 执行结果
    --------pool start---------
    我是进程:1 -4694 开始执行
    我是进程:0 -4693 开始执行
    我是进程:2 -4695 开始执行
    1 执行完毕，用时0.27
    我是进程:3 -4694 开始执行
    0 执行完毕，用时1.16
    我是进程:4 -4693 开始执行
    3 执行完毕，用时1.17
    我是进程:5 -4694 开始执行
    2 执行完毕，用时1.60
    我是进程:6 -4695 开始执行
    4 执行完毕，用时0.58
    我是进程:7 -4693 开始执行
    5 执行完毕，用时0.50
    我是进程:8 -4694 开始执行
    8 执行完毕，用时0.20
    我是进程:9 -4694 开始执行
    7 执行完毕，用时0.65
    9 执行完毕，用时0.96
    6 执行完毕，用时1.61
    ---------pool end----------
    '''
    # 可以发现一直是4693、4694、4695三个进程在调度
    ```

    进程池中的队列可以这么创建

    ```python
    q = multiprocessing.Manager().Queue()
    ```

    



### 进程间的通信

###### 通讯队列

- 简单用法

    ```python
    from multiprocessing import Queue
    
    q = Queue(3)   # 创建一个进程通讯队列，该队列可以暂存3次数据
    q.put(232)
    q.put("abc")
    q.put([1,2,3])
    # q.put("cc")   阻塞，因为放入太多，等待提取清空
    print(q.get())
    print(q.get())
    print(q.get())
    #print(q.get()) 阻塞
    ```

- 进程``通讯队列``实例

    ```python
    import multiprocessing
    # 模拟网络下载器
    def recv(q):
        '''下载数据接收'''
        received_data = ["hello","world","123"] # 模拟下载好的数据
        for i in received_data:
            q.put(i)   # 放入数据到队列中
    
    def handle(q):
        '''处理下载好的数据'''
        handled_data = list() # 建立一个列表，用于提取数据
        while True:
            tmp = q.get()  # 从队列中提取数据
            # 也可以用q.get_nowait()该方法为非阻塞提取，但是如果队列为空，则会返回错误
            handled_data.append(tmp)
            if q.empty():  # 判断队列是否为空
                break
        print(handled_data)
    
    def main():
        q = multiprocessing.Queue(3) # 定义一个进程间通讯的队列
        p1 = multiprocessing.Process(target=recv,args=(q,)) #创建下载进程，传递进程队列为参数
        p2 = multiprocessing.Process(target=handle,args=(q,))  #创建处理进程，传递进程队列为参数
        p1.start()
        p2.start()
    if __name__ == "__main__":
        main()
    ```

    

