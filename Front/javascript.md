# JavaScript

JavaScript 是一种基于``对象``和``事件驱动``的客户端脚本语言



## Javascript 基础

#### 注释

```javascript
//  javascript 用 // 注释单行
/*  javascript 用该方法注释多行  */
//  javascript 语句用  ;  结束
```



#### 标识符

变量、函数、属性名称：

- 字母、数字、下划线_、美元符号$
- 不能数字开头
- 不能使用关键字



#### 变量定义和赋值

- 用``var``声明变量、变量赋值

  ```javascript
  var student_name;    // 声明变量
  var student_$_ID;    // 都合法，只是不能数字开头
  var $title;          // 合法，以美元符号开头
  var student_hobby="football";
  var gender;
  gender="male";
  var age=18, email="ssx@gmail.com", address;  //声明和赋值多个变量
  
  family="no family";    //省略var，则定义全局变量。该方式不推荐。
  ```



#### 数据类型

``typeof(变量)``操作符，用于返回变量的类别。

简单数据类型(ECMAScript)，``undefined``、``null``、``boolean``、``number``、``string``

- ``undefined``变量类型，即还没有被赋值的变量，或者浏览器解析不了的类型。

- ``null``空指针，类型为一个``object``的指针，但是还没有添加引用。

  - undefined 是从 null值 派生出来的，因而undefined == null 时，返回true。

- ``number``数据类型，可以表示整数、浮点数，都统称number类型

  - number里面有一个特殊类型的值``NaN``(Not a Number)，它也属于number类型，但是它是有特殊意义。

    - 任何涉及NaN的操作都会返回NaN，例如``NaN/10 返回NaN``

    - NaN和任何值都不相等，包括NaN本身

    - ``isNaN(n)``，该方法检测一个变量是不是非数值，只要是非数字（包括字符串和NaN本身）都可以让其返回真。

      ```javascript
      isNaN("1234")  // 返回false，因为isNaN会尝试将字符串转换为数值，然后再判断是不是数值
      isNaN(1234)  //返回false
      isNaN("abc")  //返回true
      isNaN(NaN)   //返回true
      isNaN("NaN")  //返回true
      ```

  - ``Number()``方法，将非数值强转成为数值

    ```javascript
    var num_str="16";
    typeof(num_str);   //返回string类型
    var id=Number(num_str);
    typeof(id);   //返回number类型
    Number("lalala")   // 转换不了，则返回NaN
    Number("123.54n")  // 返回NaN，他不像parseInt和parseFloat有截断功能，他会将所有参数内容作为考虑对象
    ```

  - ``parseInt()``，转换成数值

    ```javascript
    var topval="28px";
    var top_val=parseInt(topval);   // 将“28px” 转换成28 这个整数了
    var numc=parseInt("abc33");     // 返回NaN，parseInt默认会从开头读取数字并截断非数字的后面部分，若字母开头，则出错
    var oct_val = parseInt("0xfff",16)  //第二个参数表示进制
    ```

  - ``parseFloat()``，同理parseInt

- ``string``，用单引号或双引号。

  - ``toString()``，将一个变量转换为string 类型，写法是``str.toString()``
  - ``String()``，如果不知道一个变量是否是``null``或``undefined``的时候可以用该函数

- ``boolean``，布尔值

  - ``Boolean()``，将一个变量强制转换成boolean类型。``数值0，空字符串""，null变量，以及undefined变量``都转换为false， 其他为true。



#### 表达式

- 操作符

  1. ``+``、``-``、``*``、``/``、``%``，``++(自增)``

  2. ``=``、``+=``、``*=``

  3. ``==``比较值相等、```===```比较值相等的同时，也要比较类型是否相等、``!=``不相等，比较值不相等、``!==不全等``值和类型都不相等。

  4. ``条件? 代码1:代码2``

  5. ``&&``逻辑与，其返回值有点不一样

     ```javascript
     "abc" && 123 && "bbb";   // 逻辑与的返回值规则为：若前面所有的变量经过隐式转换后都为true，则直接返回最后一个变量，因而该表达式返回   "bbb"
     
     //类似
     ""  && 88 && "abc"    //从前向后扫描，发现第一个""经过隐式类型转换后为false，因而到此结束，返回""。
     
     //综上，该&&是短路的，返回值为能确定表达式值的变量值，而不是boolean类型的变量
     ```

     ``||``逻辑或，同理

     ``!``非操作则没有这种变量情况，无论接受什么参数，都返回boolean类型的返回值。工作中可能用到``!!88``这种操作。这种操作利用了``!``的只返回boolean的特性，可以直接将一个变量转 化成对应的boolean类型操作。



#### 语句

1. ``if``

   ```javascript
   if(condition){
     alert("condition success");
   }else if(condition2){
     alert("condition unsuccess");
     var age=prompt("请输入正确的年龄：")    // 写一个输入框，让用户输入年龄。
   }else{}
   ```

2. ``switch``

   ```javascript
   switch(expression){
     case value:
     		statement;
    		 break;
     case value: 
       	statement;
    		 break;
     default: statement;
   }
   ```

3. ``for``

   ```javascript
   for(var i=1;i<101;i++){
     document.write(i);
     document.write("<br/>");
   }
   ```

4. ``while``/``do-while``

   ```javascript
   while(condition){}
   ```

   ```javascript
   do{
     
   }while(condition)
   ```

   

#### 函数

``function functionName(args..)``

```javascript
function myfirstfunction(arg1,arg2){   //函数定义
  document.write("Hello World!<br/>");
}

myfirstfunction("myname","myage");
```

函数默认定义一个关键字``arguments``，它反映的是参数列表，类似于不``Array``，但又不同于``Array``

```javascript
function myfunc(arg1,arg2){
  typeof(arg1);
  typeof(arg2);
}
myfunc("hello");   //没有传递第二个参数，则函数在typeof(arg2)时候就会返回undefined

//同理
myfunc("hello","wolrd","how are u");  //该函数调用有三个参数，但是函数只有两个，则第三个参数会被存在于默认函数栈空间

function myfunc2(){
  Document.write(arguments.length);  //查询arguments的长度
  Document.write(arguments[2]);
}
myfunc2("hello","wolrd","how are u");   //如上所示，则关键字  arguments可以返回所有传递进函数调用的参数
```

- arguments参数默认会接手所有参数，即所有参数都是arguments参数的引用，例子如下

  ```javascript
  function myfunc(arg1,arg2){
    arguments[0]=88;   //将arguments的第一个值改为88;
    console.log(arg1);  //将第一个参数打印到控制台，结果为88，不是函数传递进来的值，因而可以看出所有参数列表都是arguments的引用值
  }
  
  myfunc(123,"hellow")  //调用myfunc，并传递参数123，但是最终控制台会打印88
  ```

  



#### 内置对象

1. ``Array``，内置数组。

   - 该数组与其他语言中的数组不同，它的每一个元素都可以是任意类型的，可以不同。

   - 该数组的长度可以变动

   - 创建数组

     - ```javascript
       var color = new Array();
       console.log(color);   //返回一个[]，表示空数组
       var fruit = new Array(3);    //表示创建了一个大小为三个空间的数组fruit
       var nums = new Array(1,2,3,4,5) // 创建数组，并用5个不同的数字对其初始化。
       var studentInfo = [6,"jack", true, {"email":"cx@163.com"}] 
       //也可以用这种方式定义数组，而且元素类型可以不一样
       ```

   - 长度``list.length``，该属性不只是可读的，而且可以写。

     ```javascript
     var list=["hellow","world","how are you"];
     console.log(list.length);  //打印3
     list.length=2;    //则截断了最后一部分，即“how are you” 被删除了
     console.log(list)  //就没有了最后一个元素了 
     
     list[99]="?what happen"; //给第100个元素赋值，则length被重新被设置成为了100，中间空着的元素都是undefined类型
     ```

   - ``arrayObject.push(x1,x2,x3,x4..)``，push方法可以一次性将很多元素添加到已存在**数组尾部**

     ```javascript
     var colors=["red","blue"];
     colors.push("yellow","black");  // 追加多个元素
     ```

   - ``arrayObject.unshift(x1,x2,x3...)``，unshift方法，将多个值按顺序添加到**数组前面**

     ```javascript
     var nums=[4,5,6];
     nums.unshift(1,2,3);  //按顺序添加到数组头，数组里面现在是[1,2,3,4,5,6]
     ```

   - ``arrayObject.shift()``，删除数组第一个元素，并返回该值

   - ``arrayObject.pop()``，删除数组最后一个元素，并返回该值