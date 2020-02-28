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

    