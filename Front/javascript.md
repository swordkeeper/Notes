# JavaScript

JavaScript 是一种基于``对象``和``事件驱动``的客户端脚本语言



## Javascript 基础（ECMAScript）

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

   - ``arrayObject.join(separator)``用于把数组所有元素放入一个字符串并返回，需要填写一个分割标志

     ```javascript
     list = [1,2,3,4,5];
     var str = list.join(","); //str为  "1,2,3,4,5"  字符串
     ```

   - ``arrayObject.reverse()``，倒序，直接将数组倒序，而不是返回一个倒序引用

   - ``arrayObject.sort(sortby)``，sort方法里面需要传递一个sortby参数，``默认的sort会将数组中所有元素转换成字符串，然后在比较字符串``

     ```javascript
     var list = [4,1,5,7,81,22,9];
     console.log(list.sort()) //再不写sortby比较函数时，默认会将list中元素转换成字符串比较，结果[1,22,4,5,7,81,9]
     //可以写一个匿名比较函数，如下：
     console.log(list.sort(
     	  function(x,y){
           return x-y;
         }
     ))
     ```

   - ``arrayObject.concat(arrayX, arrayY,....)``，链接多个数组，返回拼接之后的值

   - ``arrayObject.slice(start,end)``，返回数组中的默片段的index，start的必填，end可选，包含start，不包含end位置，start可以为负数。

   - ``arrayObject.splice()``方法，删除，插入，替换都可以实现，

     - ``arrayObject.splice(index, count)``，删除操作，删除从index开始的count个元素，并返回删除的数组，若count不填，则删除从index开始的所有

       ```javascript
       // 删除操作  arrayObject.splice(index, count)
       var arr=["a","b","c","d","e"];
       var delArr=arr.splice(2,2)   //从2开始删除2个，则原来剩下["a","b","e","f"]，返回["c","d"];
       ```

     - ``arrayObject.splice(index,0,item1,item2....)``，插入操作，count为0，表示不删除，后面添加的item项表示添加的内容，到index位置。

       ```javascript
       var arr=["a","b","c","d","e"];
       var delArr=arr.splice(3,0,"m","n")   // 在index=3的位置插入"m",和“n”，不删除则count=0
       ```

     - ````arrayObject.splice(index,2,item1,item2....)``，替换，同插入，只是count不再是0

       ```javascript
       var arr=["a","b","c","d","e"];
       var delArr=arr.splice(1,2,"x","y","z")   // 删掉index=1后的两个，再在这个位置插入xyz，则实现了替换
       ```

   - ``arrayObject.indexOf(searchvalue, statIndex)``，查找searchvalue，从startIndex开始，searchIndex必填，startIndex选填。找到返回index，未找到返回-1

2. ``String``字符串

   - ``stringObject.charAt(index)``和``stringObject.charCodeAt(index)``，一个返回字符，一个返回编码本身。老版本的浏览器不兼容使用``[]``进行索引字符串元素，所以为支持低版本的浏览器，就应该用该两种方法

     ```javascript
     var str="hello World";
     console.log(str[1]); //返回 e
     console.log(str.charAt(0))  //返回h
     console.log(str.charAt(100))  //返回一个空 ''
     console.log(str.charCodeAt(1))   //返回e的编码 111
     ```

   - ``stringObject.indexOf("o")``和``stringObject.lastIndexOf("o")``，返回一个子串的位置，找到返回index，未找到返回-1。

   - ``stringObject.slice(start,end)``和``stringObject.substring()``和``stringObejct.substr()``，截取子串

     - ``stringObject.slice(start,end)``，截取子串的开始位置start（必填），到end位置（不包括、选填），可以为负数

     - ``stringObject.substring(start,end)``，同理slice，只是不能为负数，substring会将负数转换成0。而且会将较小的数字作为较小的位置

       ```javascript
       var str="hello World";
       console.log(str.substring(5,-2))  //-2会被转换成0，然后0比5小，因而会取[0~5)截取
       ```

     - ``stringObject.substr(start, len)``，截取start开始的len个字符，start可以为负数

   - ``stringObject.split(separator)``，将字符串用separator分割成一个数组 

   - ``stringObject.replace(regex/substr,replacement)``，替换一个字符串，第一个参数可以是一个子串，也可以是正则式，第二个参数是要替换的内容，``该方法不会修改原字符串，而只是返回一个新的字符串``，若不写正则式，则``只替换一次``

   - ``string.Object.toUpperCase()``和``string.Object.toLowerCase()``大小写转化，不修改原字符串 

3. ``Math``对象

   - ``min(num1,num2...)``，``max(num1,num2...)``

     ```javascript
     var min = Math.min(5,-2,0,102,44);  //返回这几个数字的最小值
     var min2 =  Math.min(5,-2,0,102,44,"abc");  //如果出现一个非数字，则会返回NaN
     ```

   - ``ceil(num)``，向上取整，即大于该数字的整数，``floor(num)``，只返回整数部分，``round(num)``四舍五入

   - ``abs()``

   - ``random()``方法随机生成[0~1)的随机数

     ```javascript
     var rd = Math.random();
     ```

      

4. ``Date``对象

   - 创建``date = new Date()``不传参数，返回当前时间的对象

     ```javascript
     var today = new Date();
     console.log(today);  //返回当前时间
     ```

   - 具体方法

     - ``getFullYear()``，返回四位数年

     - ``getMonth()``，返回0～11

     - ``getDate()``，返回月份中的某一天

     - ``getDay()``，返回0～6

     - ``getHours()``，返回小时

     - ``getMinutes()``，返回分钟数

     - ``getSeconds()``，返回秒

     - ``getTime()``，返回表示日期的毫秒

     - 同理都有``set``方法，如``setFullYear(year)``，``setSeconds(sec)``，这些set选项如果设置的超过了最大值，则上一个单位一会进位。例如

       ```javascript
       var date = new Date() ; //拿到 2019.10.10
       date.setMonth(48);
       console.log(date)  //会显示 2023年1月10日。因为加48个月会除以12个月，为4，然后余0，因而就是加4年，然后0月
        //同理其他操作
       ```

       

#### JavaScript 错误/调试

##### JavaScript错误类型

1. ``SyntaxError``，语法错误

   ```javascript
   var 1a;  //错误，不能以数字开头定义变量名称
   ```

2. ``Uncaught ReferenceError``，引用错误

   ```javascript
   a()
   console.log(b)   //引用了不存在的对象
   ```

3. ``RangeError``，范围错误

   ```javascript
   [].length= - 5; // 长度不能设置为负数
   function f(){   // 无限递归
     f(); 
   }
   ```

4. ``TypeError``，类型错误

   ```javascript
   123()  //整数类型的，被当成函数
   ```

5. ``URIError``，URL错误

   ```javascript
   decodeURI("%");
   ```

6. ``EvalError``，eval()执行错误

##### JavaScript 调试

1. 添加``debugger``

   ```javascript
   var a =5;
   debugger;    // 添加debugger关键字，则程序在运行到此处时，就会在控制台停下来
   document.write(a);
   
   ```

2. ``throw``，主动抛出一个错误

   ```javascript
   var input_A = prompt("输入一个数字");
   var input_B = prompt("输入另一个数字");
   var parse_A = parseInt(input_A);   //将其解析成一整数
   var parse_B = parseInt(input_B);
   if(parse_A!=parse_A){   //如果解析不成功则返回NaN，而NaN是不等于任何值的
      throw new Error("argument should be an integer");   
     //一旦throw一个错误，该程序就结束了，后面任何程序都不会继续进行
     //而该错误会一致向外层函数传递，直到遇到一个  try语句。
   }
   ```

   ``try catch``，主动接受一个错误

   ```javascript
   try{
    // ..... 语句 ，如果此处遇到throw语句执行（如上的  parse_A!=parseA  的情况），
     // 则try会吧这个抛出的错误投递给catch进行执行，try后续的语句就不在执行了。
   }catch(e){
    // ....  执行错误接收到后的处理函数，处理完毕后继续执行 try/catch 快后面的代码
     // e 变量会接收到该错误对象
   }finally{
     // .. 无论执行try还是catch ，最终finally部分一定会执行
   }
   ```

   

## JavaScript DOM

DOM（Document Object Model 文档对象），提供了JavaScript一种操作HTML、XML文档的接口

1. ``查找``DOM元素

   ```javascript
   document.getElementById("id");    // 通过id查找一个html元素，返回该元素对象的引用，只返回第一个id的引用
   document.getElementsByTagName("li");   // 通过标签查找一组元素，返回该元素对象的引用
   ```

   