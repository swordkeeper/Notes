# OOP python 面向对象

1. 类

   ```python
   class Person:
       def __init__(self,name,age,pay=0,job=None):
           self.name = name
           self.age = age
           self.pay = pay
           self.job = job
       def lastName(self):
           return self.name.split()[-1] #类函数，返回姓氏
   ```

2. 继承

3. ```python
   class Manager(Person): # 写括号里，表示继承
       def giveRaise(self,percent,bouns=0.1):  # 类函数
           self.pay *= (1.0+percent+bouns)  
   ```

3. 调用

   ```python
   instance.method(arg1,arg2) # 用实例掉用
   class.method(instance,arg1,arg2) #用类调用
   ```

4. 类输出```__str__```

   ```python
   class Person:
       def __str__(self):  # 当用print(实例)时，默认调用__str__,此处重写了str
           return '%s  =>  %s' %(self.__class__.__name__ , name)
       # 此种输出格式为。      ‘%s’    %(元组)
   ```

5. 类标志```__class__```

   ```python
   # 调用 __class__返回 最底层继承的类信息
   print(instance.__class__)
   print(instance.__class__.__name__) ##返回类名
   ```

6. 类属性```__dict__```

7. ```python
   # 返回类的所有属性
   print(instance.__dict__)  #返回一个属性字典{'name': 'TOM', 'age': 22, 'pay': 3000, 'job': 'None'}
   ```

7. Getter 和 setter ```getattr和setattr```

8. ```python
   getattr(instance, field)
   setattr(instance, field, values)
   ```


