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
