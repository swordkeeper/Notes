# 模式



### python闭包

1. 变量都有作用范围，``局部变量``只在对应的``大括号``内有效
2. ``全局变量``或``外层局部变量``可以被``内层函数``引用

3. Python 可以``嵌套定义函数``

   ```python
   def storeInfo(string):
   	 infoList=[]          # infoList，string是外层函数storeInfo的局部变量，它可以被内层函数insertString访问
     def insertString():
         infoList.append(string)   # 内层函数做的事情就是把string添加到infoList中
         print(infoList)
       return storeInfo         #外层函数返回内层函数的引用
    f=storeInfo("hello")    # 返回insertString函数名给f，则f就是insertString
    f()  #执行内层函数，执行添加
    #会打印出”hello“
    #再执行一次
    g=storeInfo("world")
    g()
    #会打印出“hello”和“world”两个字符串
    #可见闭包实际上是：
    #        闭合了函数内部的变量，使得它能长期存在内存中，而且可以通过函数接口限制性的访问
   ```

4. 外层函数的局部变量可以不被销毁，即在执行上面``f=storeInfo("hello")``以后，infoList仍可以在后面多次执行，``不被销毁``，且``可以被外部访问``。

5. ``优势``：避免了使用全局变量



### 匿名函数

1. 没有函数名

   ```python
   lambda x: x*x    #该表达式，返回一个定义好了的匿名函数名，该函数名接收一个参数x，并返回它的平方
   ```



### 装饰器

1. 对一个已完成的函数进行修饰补充，但不想修改原始代码

   ```python
   # 我们有一个函数
   def f(x):
       return x*x   # 该函数返回平方
   #### 但是现在需要 一个log函数，也就是打印出来x的平方值
   # 我们可以给f函数里面添加一个 print(x) ,但这样就修改了函数，不方便
   
   ```

2. 现在给该函数添加``外层包裹``

   ```python
   def new_f(f):  #new_f接收f函数名作为参数
       def fn(x):    #内部函数
           print("call" + f.__name__+"()")    # 用到了闭包，内部函数打印该函数log，并返回调用该函数的接口
           return f(x)
       return fn
   ```

3. 调用

   ```python
   # 用一个变量接受这个闭包函数
   g = new_f(f)
   print(g(5))     # 这样g函数就等于闭包后的函数，不仅打印了log，还计算返回了5的平方
   
   
   # 现在不用g变量来接受被修改后的函数，就用原来的函数名变量来接受，这样就彻底屏蔽了原来的函数，以及修改
   f = new_f(f)       # 最后一步《》
   print(f(10))    
   ```

4. ``@装饰器``

   ```python
   #首先先定义如上的  new_f函数
   #然后在定义一个函数时候，使用装饰器，就等与做了上面的最后一步《》
   @new_f
   def f(x):
       return x*x
   ```

5. ``优势``

   极大的简化代码，避免每个函数都写重复的代码如：

   ``@log``，打印日志

   ``@performance``，检测性能

   ``transaction``，数据库事务等