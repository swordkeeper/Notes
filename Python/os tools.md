# python 系统编程

python 的系统模块包括

```python
import os , sys #系统服务
import socket #网络包和IPC
import threading, _thread, queue #同步，并发线程
import time, timeit  #系统时间
import subprocess, multiprocessing # 并行控制
import signal, select, shutil, tempfile #多系统
```



1. dir 函数

2. ```python
   import os, sys
   dir(sys) ##列出所有包，或对象下的属性 类似于ls. 列出所有的内嵌模块
   ```
3. os 偏向操作系统底层的库
   sys 偏向python解析器的底层库

### sys库
```python
import sys, os
sys.platform, sys.maxsize, sys.version
```
因而创建底层有差异的程序时，可以先测试```sys.platform```
1. sys.path 返回包导入时的所有搜索路径，它在python启动时有```PYTHONPATH```环境变量，以及Python目录下```.pth```等文件共同设置。 __该函数返回的是一个列表，因而你可以随时添加路径进入到sys.path中__。该方式是临时性的。
2. sys.modules 返回已经家在的模块, 他是一个字典，格式为 name:module
3. sys.exc_info 返回一个异常信息元组

### os库
该模块提供POSIX标准工具，操作系统跨平台移植工具。基本上```os，作为系统调用的接口```。
```python
os.environ  #shell变量
os.system, os.popen os.execv, os.spawnv #运行程序
os.fork, os.pipe, os.waitpid, os.kill #派生进程
os.open, os.read, os.write #文件描述符，文件锁
os.remove, os.rename, os.mkfifo, os.mkdir, os.rmdir #文件处理
os.getcwd, os.chdir , os.chmod, os.getpid, os.listdir, os.access #管理工具
os.sep, os.pathsep, os.curdir, os.path.split, os.path.join #移植工具
os.path.exists('path'), os.path.isdir('path'), os.path.getsize('path') #路径名工具
```
1. os.sep 返回不同操作系统路径分隔符，win是\，mac是:，POSIX是/           os.pathsep提供目录列表分隔符，POXIS是:，Win和DOS是;    
因而如果需要根据不同系统分离目录可以用
```python
>>> import os
>>> path = os.getcwd()
>>> path
'/Users/tmp'
>>> path.split(os.sep)
['', 'Users', 'tmp']
>>> os.linesep  #查看系统换行符
'\n'
```
2. os.path 路径工具
```python
os.path.isdir('path'), os.path.isfile('path')
os.path.exists('path')
os.path.getsize('path')
os.path.splitext('path')   #分离扩展名
os.path.abspath('') #显示绝对路径
```
3. 经典的路径分割
```python
>>> os.path.split('/Users/tmp/123.txt')
('/Users/tmp', '123.txt')
>>> os.path.join('/Users/tmp', '123.txt')
'/Users/tmp/123.txt'
```
4. os.system(‘command')执行shell命令；os.popen('cat 123.txt')创建一个管道文件，用于返回执行该命令'cat 123.txt'的返回值。popen管道的一端为命令执行体，另一端为打开的这个管道对象。默认为读管道。
5. subprocess 包
这个包类似于os.system 和 os.popen
```python
import subprocess
subprocess.call('cat 123.txt' shell = True)  # 调用shell, unix系统需要加上 shell = True
pipe = subprocess.Popen('python helloworld.py', stdout = subprocess.PIPE)
pipe.communicate() #返回管道读取的内容
pipe.returncode #返回返回值
```