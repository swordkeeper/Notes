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