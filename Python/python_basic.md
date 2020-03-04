# Python 的一些基础



### 简单的Python知识点

1. zip 打包两个列表，使他们一一批对

    ```python
       name = ['name','age','salary']
       values = ['bob',42,20000]
       zip(name,values) # produce a zip obj
       people1 = dict(zip(name,values))  #将绑定的zip对象转化成字典
       # return {'name':'bob','age':42,'salary':20000}
    ```

2. eval 和 repr 

    ```python
    # eval() 把str 变成 其值所对应的内容
    sss = '2'
    type(eval(sss)) # return 2
    # repr() 把值变为字符串
    repr(2) #返回'2'
    ```

3.  map

       ```python
       people = ['bob','LiLi']
       getInitCapital = map((lambda x: x[0]), people)
       # map 函数有两个参数。第二个参数为一个列表，第一个参数为函数名，map会对第二个参数列表中的每一个元素调用第一个参数的函数
       list(getInitCapital) # 返回列表
       ```

4. enumerate() 迭代器

    ```python
    myList = [1,"abc","hello",222]
    for x,y in enumerate(myList):
        print(x)
        print(y)
    # 返回 
    """
    0
    1
    1
    abc
    2
    hello
    3
    222
    """
    ```

5. 属性、静态方法

    ```python
    class T(object):
       @staticmethod
        function():
          pass
       @property
       function1(self):
         return 100     # property必须返回一个值，参数也必须只有self 调用时用 a = T()  a.size
       @function1.setter(self, value):
          pass
       @function1.deleter(self):
          pass 
        
       #  魔法方法也可以实现该方法，即property
       #  property(arg1,arg2,arg3,description)  前三个参数分别代表，表达式，设置式，删除式 
    ```

6. property方法，定义getter和setter

    ```python
    class income(object):
       def __init__(self):
           self.money = 0
       def __money_getter(self):
           return self.money
       def __money_setter(self, value):
           self.money = value
          
       money = property(__money_getter, __money_setter)
    
    myMoney = income()
    myMoney.money = 100
    print(myMoney.money)
    ```

7. 上下文管理器``__enter__()``和``__exit__()``方法

    ```python
    class File():
        # 自定义的方法类，如果定义了__enter__()和__exit__()方法，则就可以调用with关键字啦
        def __init__(self, filename, mode):
            self.filename = filename
            self.mode = mode
    
        def __enter__(self):
            print("entering")
            self.f = open(self.filename, self.mode)
            return self.f
    
        def __exit__(self, *args):
            print("will exit")
            self.f.close()
            
            
    with File('out.txt', 'w') as f:    # 自定义的类，可以使用File方法了。with关键字，就会自动调用enter和exit方法
        print("writing")
        f.write('hello, python')
    ```

    也可以用如下方法定义上下文

    ```python
    from contextlib import contextmanager
    
    @contextmanager
    def my_open(path, mode):
        f = open(path, mode)
        yield f
        f.close()
    ```

    

### Python 包的建立

1. 第一步建立一个``文件夹📁``，给文件夹起包的名字，比如``calc``文件夹

2. 在文件夹中放入需要打包的``.py``文件。比如，``add.py``和``substract.py``

3. 在文件夹下写入一个``__init__.py``文件

4. 在文件中写入

   ```python
   # __init__.py 文件
   from . import add
   from . import substract   # . 代表当前目录，也就是和 __init__.py 同级的add.py 和 substract.py文件
   # __init__.py 文件的作用是，给文件夹添加一个索引，用于倒入python包
   ```

5. 正常文件引入包

   ```python
   import calc
   ```



### Python 压缩包

1. 建立一个``setup.py``文件，该文件和需要打包的``calc``同级

2. ``setup.py``文件中写入

   ```python
   from distutils.core import setup
   
   # 写入几乎固定的内容，调用setup函数
   setup(
     name="calc", #包名
     version="1.0", 
     description="计算器包"  #简述
     long_description="计算加减乘除包" #详述
     author="zusz"
     author_email="xxxx@.com"
     url="www.mainpage.com" #主页
     py_modules=[   #包中要发布的内容
       "calc.add",
       "cacl.substract"
     ]
   )
   ```

3. 执行setup函数

   ```bash
   python3 setup.py build
   ```

4. 发布

   ```bash
   python3 setup.py sdist
   ```

   该命令会生成一个压缩包``calc-1.0.tar.gz``，从而可以分享给其他人

5. 接受压缩包并解压

   ```bash
   tar xvfz calc-1.0.tar.gz  #解压
   cd calc-1.0  # 进入解压后的文件夹
   sudo python3 setup.py install
   ```

   解压后安装的包一般会被安装到``/usr/local/lib/python3.xx/dist-packages``，即local环境下。这样就可以在全局使用``import calc``python倒入命令了。

6. 删除某个安装好的模块

   ```bash
   cd /usr/local/lib/python3.xx/dist-packages  #切换到模块安装目录
   sudo rm -rf calc* #删除该包文件以及相关信息文件，就可以了
   ```



### Python 编码兼容性

Python2.x 默认使用的是``ASCII``编码

Python3.x默认使用的是``UTF-8``编码，所以默认情况下，python3 支持中文等特殊字符



演示代码如下``test.py``

```python
myStr = "你好世界"
print(myStr)

for c in myStr:
  print(c)
```

在Python3 下执行，没有问题输出

```
你好世界
你
好
世
界
```

1. 但是在python2下执行，会报错，错误类型为``SyntaxError: Non-ASCII character '\xe4' in file``。

   - 此处说明python2 无法解析代码``test.py``中的非ASCII字符（中文）

   - 解决方法，在test.py首行添加

     ```python
     # *-* coding:utf-8 *-*
     ```

     or
     
     ```python
     #coding=utf-8
     ```
     
     这样python2解析器就能解析代码中的中文字符了

2. 但是输出扔会成为这样

   ```
   你好世界
   ?
   ?
   ?
   ?
   ?
   ?
   ?
   ?
   ?
   ?
   ?
   ?
   ```

   - 原因是因为，虽然解析器知道字符串myStr是utf编码（这也就是为什么能输出：你好世界）。

   - 但是在当循环执行时，解析器仍然按照``ASCII``解析器的方法， 即一个字节一个字节的解析(unicode 占1-3个字节）。这也就是为什么当循环时，不能正确输出。

   - 解决方法

     ```python
     myStr = u"你好世界"  # 给字符串指明编码格式，在字符串前面添加 u
     ```




### 迭代器

- 迭代器实例

    ```python
    from collections import Iterable
    
    class Classmate(object):
        def __init__(self):
            self.names = list()  #学生列表
        def add(self,name):
            self.names.append(name)
        def __iter__(self):  # 如果想让类 Classmate 可以迭代，需要完善该类
            return ClassIterator(self)    # for tmp in xx: 实际上是调用该对象的__iter__方法，
                                      # 该方法返回一个迭代器对象
    
    class ClassIterator(object):
        '''
        迭代器对象会一步步的调用__next__方法，返回调用者的每一个内部对象
        '''
        def __init__(self,obj):   # 迭代器获取classmate对象指针
            self.obj = obj
            self.count = -1   # 定义迭代器的计数器
            self.len = len(self.obj.names)
        def __iter__(self):
            pass
        def __next__(self):
            if self.count < self.len -1:
                self.count += 1
                return self.obj.names[self.count]  # 每次循环next返回下一个对象
            else:
                raise StopIteration
    
    classmate = Classmate()
    classmate.add("老王")
    classmate.add("老李")
    classmate.add("张三") #添加了三个学生
    
    print(isinstance(classmate, Iterable)) # 返回True 因为类classmate是Classmate类的实例
                                           # 而因为该类实现了 __iter__方法，因而可以迭代
    
    for i in classmate:
        print(i)
    ```

    迭代器注意事项

    1. 一个类对象要实现迭代功能，则需要实现``__iter__``函数，该函数返回一个迭代器。这样该类对象才是``Iterable``
    2. 迭代器类要有一个``__next__``方法，定义每次循环迭代时返回的内容。
    3. 每次迭代时要返回不同索引位置的内容，需要在迭代器内部定一个``index``来保存当前迭代位置
    4. 迭代器不会自动判断停止，若要停止（比如说迭代完成）则需要发起``raise StopIteration``异常中断
    5. 一个类实现迭代功能也可以不 ``定义迭代器类``，只要在``__iter__``函数时返回自己``self``，并在自身函数内实现``__next__``即可
    
    ###### 迭代器的优势
    
    迭代器和直接返回一个列表的区别是，迭代器不会生成所有的返回内容，类似于``range 和 xrange``的区别。迭代器返回的是一种生成结果的方法，而不是所有的数据。比方说range(10)，会返回一个10个数字的列表，而xrange(10)，每次迭代则动态返回的是每次的生成内容。再比如range(10000000000)，机器会卡死，而xrange(10000000000)，不会。
    
- Fibonacci 迭代器举例（实现了不用列表存储Fibonacci值)

    ```python
    #coding=utf-8
    import time
    class Fibonacci(object):
        def __init__(self,num):
            self.num = num
            self.a = 0
            self.b = 1
            self.index = 1
        def __next__(self):  # 迭代器每次比较迭代次数
            if self.index <= self.num:
                if self.index == 1:
                    self.index += 1
                    return self.a
                elif self.index == 2:
                    self.index += 1
                    return self.b
                else:
                    self.a , self.b = self.b , self.a+self.b  #Fibonacci算法
                    self.index += 1
                    return self.b
            else:
                raise StopIteration  # 迭代上限
        def __iter__(self):
            return self
    
    def main():
        for i in Fibonacci(10):
            time.sleep(1)
            print(i)
    
    if __name__ == "__main__":
        main()
    ```



### 生成器

生成器是一个种特殊的迭代器

- 生成器实例

    ```python
    def create_fibonacci(all_num):  # 定义一个函数
        a, b = 0, 1
        current_num = 0
        while current_num < all_num:
            # print(a)
            yield a                 # 一个函数中如果出现yield， 则该函数就不是函数了，就是生成器
            a, b = b, a+b
            current_num += 1
    
    obj = create_fibonacci(10)     # 生成器这样写，而且，此处不再是调用函数，而是创建了一个生成器对象
    for num in obj:                # 生成器迭代时，只有第一次从函数 def开头向下走，
      	print(num)								 # 走到yield处返回，类似于迭代器中的__next__函数的return。并且函数阻塞在yield处
                                   # 直到函数下一次迭代时唤醒，并继续循环
        
    ```

- 生成器注意事项

    1. 生成器不是函数，调用他会返回obj对象
    2. 生成器中有``yield``，他的功能类似于``return``，类似于``__next__``迭代器中的返回
    3. 生成器只有第一遍从头开始执行，每次循环迭代``yield``返回后，生成器会阻塞，直到下一次需要迭代时解除阻塞。

- 生成器也可以用函数``next(obj)``来循环，或者``obj.send("hahah")``。send 可以传递一个参数，用如下方法来接受``arg = yield a``，这样通过send发送的数据就会被arg接收



### Python 正则表达式

```python
import re

# pattern = "hello" str="hellohahahah", match匹配以pattern开头的字符串
result = re.match("hello","hellohahahah")  
print(result.group())   # group返回匹配到的组
```

|      字符       | 功能                                                         |
| :-------------: | :----------------------------------------------------------- |
|        .        | 匹配任意字符，除了\n                                         |
| ``[]``和``[^]`` | 匹配[]中列举的字符，[^]表示不在括号中的字符                  |
|       \d        | 匹配一个数字0~9                                              |
|       \D        | 匹配非数字                                                   |
|       \s        | 匹配空白，包括空格、换行、制表符                             |
|       \S        | 匹配非空白                                                   |
|       \w        | 匹配单词字符a-z、A-Z、0-9、_                                 |
|       \W        | 匹配非单词字符                                               |
|        -        | 量词``-``，[0-9]表示匹配0～9之间任意数字                     |
|                 | [0-35-9]匹配0～3或5～9之间的数字                             |
|        *        | 量词``*``。匹配前一个字符出现0次或者无限次，即可有可无       |
|        +        | 量词``+``。匹配前一个字符出现1次或者无限次，即至少有1次      |
|       ？        | 量词``?``。匹配前一个字符出现1次或者0次，即要么有1次，要么没有 |
|       {m}       | 量词``{m}``。匹配前一个字符出现m次                           |
|      {m,n}      | 量词``{m,n}``。匹配前一个字符出现从m到n次                    |
|        ^        | 匹配开头                                                     |
|        $        | 匹配字符串结尾                                               |
|        \        | 转义字符                                                     |
|       \|        | 匹配任意一个，或的意思                                       |
|      (ab)       | 将括号中字符作为一个分组                                     |
|      \num       | 引用分组num匹配到的字符串。转义字符加数字，代表第几个匹配的分组 |
| ``(?P<name>)``  | 分组起别名<br />``ret = re.match(r"<(?P<name1>\w*)><(?P<name2>\w*)>.*</(?P=name2)></(?P=name1)>", "<html><h1>www.itcast.cn</h1></html>") ``<br />前面用``<>``起了别名，后面用``=``使用别名 |
|    (?P=name)    | 引用别名为name分组匹配到的字符串                             |
|    .search()    | 搜索某内容，即不是从字符串开头匹配。``ret = re.search(r"\d+", "阅读次数为 9999")``，返回9999 |
|   .findall()    | 搜索所有<br />``ret = re.findall(r"\d+", "python = 9999, c = 7890, c++ = 12345")``<br />返回 ['9999', '7890', '12345'] |
|   .replace()    | 替换所有搜索<br />``ret = re.sub(r"\d+", '998', "python = 997")``<br />返回python = 998。即将997换成998 |
|    .split()     | 切割字符串<br />``ret = re.split(r":| ","info:xiaoZhang 33 shandong")``<br / >以``:``或者`` ``（空白）来切割字符串，返回<br />['info', 'xiaoZhang', '33', 'shandong'] |
|       ??        | 在任何量词包括``?``、``+``、``*``、``{m,n}``后面在添加一个？，则使得该量词变为非贪婪模式 |
|                 |                                                              |
|                 |                                                              |
|                 |                                                              |
|                 |                                                              |
|                 |                                                              |
|                 |                                                              |

