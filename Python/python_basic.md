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

     

   