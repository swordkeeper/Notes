# Python 基础

选自python programming <毒蛇>

1. 注释

   ```python
   # python 用井号注释
   ```

2. 列表

   ```python
   bob = ['Bob Smith', 42, 3000, 'software']
   bob[0],bob[3] ##返回二元组（'Bob Smith', 'software'）
   bob[1] += 5 ## bob 列表值改变
   
   
   bob.append(['address','burwood','2023'])  ##向列表末尾中添加元素 
   # 结果就变成['Bob Smith', 42, 3000, 'software',['address','burwood','2023']]
   
   
   bob.extend(['address','burwood','2023']) ## 向列表结尾一个个元素添加，结果为['Bob Smith', 42, 3000, 'software','address','burwood','2023']
   
   
   len(bob) ##查看列表长度
   ```

3. String

   ```python
   #split()方法 可以传入分隔符，默认为空白
   >>> line = 'aaa\nbbb\nccc\n'
   >>> line.split('\n')
   ['aaa','bbb','ccc','']
   >>> line.splitlines()
   ['aaa','bbb','ccc']
   ```

4. map

   ```python
   people = ['bob','LiLi']
   getInitCapital = map((lambda x: x[0]), people)
   # map 函数有两个参数。第二个参数为一个列表，第一个参数为函数名，map会对第二个参数列表中的每一个元素调用第一个参数的函数
   list(getInitCapital) # 返回列表
   ```

5. sum函数

   ```python
   detail = [222,333,111]
   sum(detail) ### 返回666
   ```

6. lambda 函数

7. ```python
   lambda x:x[0] # 匿名函数，对调用函数的每个参数x，取x[0]返回。:前是调用参数，后是返回列表
   ```

7. dict

   ```python
   bob = {'name':'Bob Smith', 'age':42, 'pay':3000}
   bob['name'] # return Bob Smith
   Lili = dict(name='LiLi la', age=44, pay = 10000)
   sun = {} # 空字典
   sum['name'] = "sun da sheng "
   
   # 设立一个默认初始值的的字典
   fields = ('name','age','job')
   record = dict.fromkeys(fields)
   ```

8. zip 打包两个列表，使他们一一批对

   ```python
   name = ['name','age','salary']
   values = ['bob',42,20000]
   zip(name,values) # produce a zip obj
   people1 = dict(zip(name,values))  #将绑定的zip对象转化成字典
   # return {'name':'bob','age':42,'salary':20000}
   ```

9. eval 和 repr 

10. ```python
    # eval() 把str 变成 其值所对应的内容
    sss = '2'
    type(eval(sss)) # return 2
    # repr() 把值变为字符串
    repr(2) #返回'2'
    ```
