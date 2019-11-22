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

2. ``设置样式``

   - ``element.style.styleName=styleValue; `` ，设置element元素的样式， 如果有样式是用`` -``  链接起来的，例如 ``font-size``，这样在此不能用`` -`` ，必须改写成为  ``fontSize ``驼峰形式。

     ```javascript
     var chr = document.getElementById("h3_char");
     chr.style.color="#fee";
     chr.style.fontSize="50px";
     ```

   ``设置内容``

   - ``element.innerHTML``，用于设置 HTML 标签中的内容，如果该标签中还有别的标签，例如``<div><b>hello</b></div>``，则会连内部的b标签页显示

     ```javascript
     var div_c = document.getElementById("div_c");
     div_c.innerHTML="HELLO, THIS IS YOUR CONTENT"; 
     ```

   ``设置类``

   - ``element.className``，获取元素的类

     ```javascript
     var chr = document.getElementById("h3_char");
     chr.className+=" new_class";  // += 符号保证了是添加一个类，若是直接等号，则替换了原来的类。
     ```

   ``设置属性``

   - ``element.getAttribute("attribute")``，获取一个属性

     ```javascript
      //. <input type="text" id="input"/>  该标签有属性 type 和 id， style只是一个标签的一个属性而已
     var chr = document.getElementById("input");
     chr.getAttribute("type");   //可以以这种方式获得input标签的type属性，也可以如下
     chr.type     //该方式也可以获得input标签的type属性，但是该种方法不能获得 class类属性，因为class有点标记。而且该方法只能用于 ``标签自带的属性``， 用户定义的属性无效。
     ```

   - ``element.setAttribute("attribute","value")``，同理将某个属性设置成某些值，该方法也可以用于添加一个自定义属性。

   - ``element.removeAttribute("attribute")``，删除某个属性



#### HTML事件

- ``<tag 事件=“执行脚本> </tag>``，HTML事件就是写在HTML标签内部的事件，与HTML元素绑定。

  1. 鼠标事件：

     - ``onload``，页面加载完毕时立即触发

       ```javascript
       window.onload=function(){}
       ```

     - ``onclick``，点击时触发

       ```html
       <input type="button" value="弹出" onclick="alert("我是按钮")" />
       ```

     - ``onmouseover``，鼠标滑过时触发

     - ``onmouseout``，鼠标离开时触发

     - ``onmousemove``，鼠标在按钮上移动时触发

     - ``onmousedown``，鼠标按下

     - ``onmouseup``，鼠标松开 

     - ``onfocus``，获得焦点时触发 ，用于表单

     - ``onblur``，失去焦点时触发，用于表单

     - ``onsubmit``，当表单确认提交按钮被执行时触发，用在``<form>``标签上

     - ``onchange``，域内容发生改变时触发

       ```javascript
       function init(){
         var menu = document.getElementById("menu");
         menu.onchange=function(){ // menu 是一个多选框，即<select>，如果用户选择或改变了多选框中的内容，该事件触发
           var bgcolor=this.value;
           var bgcolor=this.options[menu.selectedIndex].value; 
         }  
       }
       ```

     - ``onresize``，窗口大小重新调整

     - ``onscroll``，拖动滚轮

     - ``onkeydown``，键盘按键被按下时发生

     - ``onkeypress``，键盘按键按下时发生，区别在于，该方法只监控``字符，数字，符号``，不监控特殊控制符号，包括上、下等 。而且，该事件会先出发事件，再打印字符，这时文本框中的数据还没发生改变；而onkeydown则是先打印字符再触发事件。

     - ``onkeyup``，键盘按键松开时发生

     - ``keyCode``，返回键盘事件的按键值

     ```javascript
     document.onkeydown=function(event){   //document 文本域监控键盘按键，按键则触发该function，而且自带一个
       console.log(event.keyCode)          //event变量。可以通过keyCode来的到按键值
     }
     ```

     在触发事件时，``this``是对DOM元素的引用

     ```html
     <body>
       <input type="button" value="变色" mouseover="mouseover(this)" /> <!--鼠标滑过变色，添加了this-->
       <script>
         	function mouseover(this){
             this.style.background="red";   //this对象就作为该事件触发的DOM的对象
           }
       </script>
     </body>
     ```

#### DOM0事件

即先通过``docuement``查找到元素，在写入到对象内部的事件，而不通过HTML页面

- ``ele.事件=执行脚本``

  ```javascript
  var btn = document.getElementById("btn");
  btn.onclick=function(){    //执行脚本为匿名类   
    console.log(this); 			 //打印本对象
  }    // 以上是匿名事件的写法
  function btn_click(){
    console.log(this);  //这种方式默认传递this参数
  }
  btn.onclick=btn_click;   //如果有名类，传递给onclick时候，不能添加括号
  ```

#### DOM 节点

- ``document.createTextNode()``，创建文本节点

- ``document.createElement()``，创建标签节点，重点

  ```html
  <body>
    <ul id="myList" ></ul>
    <script>
      var ul = document.getElementById("myList");
      var li = document.createElement("li");    //创建标签节点
      var text = document.createTextNode("Item");
      ul.appendChild(li);     //添加到ul标签中
      li.appendChild(text);   // 创建带 文本的节点，并添加到li下
    </script>
  </body>
  ```

  

- ``document.createComment()``，创建注释节点

- ``document.createDocumentFragment()``，创建片段，主要用于分段整合节点树

- ``document.getElementsByName("name")``，获得所有``name``属性为指定值的对象集合

- ``document.getElementsByTagName()``，返回所有标签名为指定值的所有元素。较为常用

  ```javascript
  var inputs = document.getElementsByTagName("input");
  console.log(inputs[0].value); //输出input的值
  var comment = document.getElementsByTagName("!");  //获取所有的注释对象
  console.log(comment[0].nodeValue);      //输出comment的节点内容
  ```
  
- ``getElementsByClassName()``，根据类名查找

- ``querySelector()``，返回文档中匹配的指定CSS选择器的一个元素

  ```javascript
  var myDiv = document.getElementById("myDiv");
  var ul = myDiv.querySelector("#myUL");   // CSS格式的ID，即#myUL
  var li = myDiv.querySelector("li:last-child");
  ```

- ``querySelectorAll()``，返回文档中匹配的指定CSS选择器的一组元素

#### 操作节点的方法

- ``appendChild()``，为nodeList列表尾部添加一个新的节点，返回新增的节点/

- ``insertBefore()``，指定子节点之前插入一个新的节点 

- ``replaceChild()``，替换某个节点

- ``cloneNode()``，克隆一个节点，返回该节点的副本。接收一个参数，可以为true（默认为false），当为false时，只复制节点本身，不复制该节点的子节点。

- ``normalize()``，合并相邻的Text节点

  ```javascript
  var div = document.createElement("div");
  var text1 = document.createTextNode("text1");
  var text2 = document.createTextNode("text2");
  div.appendChild(text1);
  div.appendChild(text2);
  console.log(div.childNodes.length); // 返回 2
  div.normalize();   // 将一个节点中的文本节点合并
  console.log(div.childNodes.length); // 返回 1
  console.log(div.firstChild.nodeValue) // 返回合并后的文本值
  ```

- ``splitText()``，与normalize正好相反，传入一个整数参数，参数之前的所有文本内容作为一个旧的节点值，参数返回一个新的节点（参数后所有的文本内容）。

- ``removeChild()``  ，删除某个节点下的子节点，返回删除的节点。传入参数为要删除的节点

- ``removeNode()``，IE私有方法，删除目标节点，参数为布尔值，默认为false

- ``innerHTML``

- ``deleteContents()``

- ``textContent``

#### HTML属性的Js操作

HTML属性分为：固有属性``property``和自定义属性``attribute``，所有的都被称为属性。

- 固有属性可以通过``.``访问。例如，``div.id``，自定义属性不可以通过``.``访问

- 返回所有属性``namedNodeMap``集合，``div.attributes``。也可以通过该集合拿到具体某个属性的值``div.attributes.getNamedItem("xxx").nodeValue``，该方法只能用于在标签属性区域显示定义了的方法。

- 删除属性``div.attributes.removeNamedItem("yyy")``

- 添加一个属性，需要传递属性对象，而不是字符串

  ```javascript
  var attr= document.createAttribute("data-title");
  attr.value="ddd";  //创建属性对象并赋值
  div.attributes.setNamedItem(attr);
  ```

- ``getAttribute()`` ，获得一个自定义或者固有属性的值。注意：对于``onclick``和``style``属性，用``getAttribute()``和``.``方法访问是有区别的，``.``一般返回对象，或函数。``getAttribute``一般返回值内容。

- ``setAttribute()``，设置一个属性的值 。第一个参数为属性名称，第二个参数为属性值

- ``removeAttribute()``，移除某一个属性。

##### 常见字符串属性

1. id
2. title
3. href
4. src
5. Lang ，表示浏览器解析时候的语言
6. dir ，文字方向，值为"RTL"，"LTR" 左到右
7. accesskey ，快捷键
8. name 
9. value , 表单值
10. Class

##### 全局属性

　1. accesskey
 　2. class
 　3. contenteditable ，表示内容是否可以编辑，一般与spellcheck属性 组合使用
 　4. dir
 　5. hidden
 　6. id
 　7. lang
 　8. spellcheck
 　9. style
 　10. tabindex ，定义tab见进行导航时的顺序
 　11. title
 　12. Translate

##### data属性

存储页面或程序的私有数据，用于配置信息。通常以``data-``开头

```html
 <button id="btn" data-xxx-yyy="abc" data-toggle="modal"> small modal </button>
<script>
	console.log(btn.dataset.toggle);   //可以直接访问toggle自定义的数据
  console.log(btn.dataset.xxxYyy);   //多个分隔符分开的需要用驼峰形式 
</script>
```

##### class属性

```javascript
function(ele,cls){
  ele.classList.add(cls);   // classList 返回class对象，并调用add方法，就把cls添加到ele的类中了
  ele.classList.remove(cls);
  ele.classList.contains(cls);    //判断存在与否某个类
  ele.classListtoggle(ele,cls);       //如果存在，则删除，否则添加
}

```



## Javascript BOM

BOM（browser object model）浏览器模型，用于提供浏览器的一些功能

1. ``window``，浏览器的实例对象，是所有``全局变量和全局函数``的根。

   ```html
   <script>
     var age =15;  // 定义了一个全局变量，等价于定义window对象的一个属性，是全局变量，如下：
     window.age=15;  
   </script>
   ```

   - ``confirm``方法，用于生成一个确认的弹出框

     ```javascript
     var result = window.confirm("您是否确定删除")  // 点击确定返回true
     ```

   - ``open``方法，``window.open(pageURL,name,parameters)``，打开一个新的浏览器窗口，或者查找一个以命名的窗口。其中name为子窗口句柄（即新窗口的名称，方便引用）

     ```javascript
     window.onload=function(){
       window.open("new.html","new_page_name","width=400,height=200,left=0,top=0,toolbar=no,menubar=no,scrollbars=no,location=no,status=no");
       //设置了在本页面加载完毕后瞬间打开一个新的页面，名称变量为new_page_name，并设置一些新页面的属性。
     }
     ```

   - ``close``方法，关闭一个窗口，返回一个调用ID

2. ``setTimeout(code,millisec)``，在指定毫秒数后执行代码code。``clearTimeout(timeoutId)``取消某个计时器

   ```javascript
   window.setTimeout("alert('HELLO')",1000)  //1s之后执行代码code，即alert("hello")
   var timeoutID = setTimeout(function(){   //推荐用这种方法
     alert("hello");
   },1000)
   window.clearTimeout(timeoutID) ; // 取消执行setTimeout所设置的函数
   ```

3. ``setInterval(code,millisec)``，同理，每隔指定时间执行一次指定代码code。该方法也会返回一个Id，``clearInterval(id)``，同理清除。

4. ``location``对象，提供与当前窗口加载有关的一些信息

   - ``location.href``，返回当前加载页面的完整URL

     ```javascript
     console.log(location.href);
     ```

   - ``location.hash``，URL的锚位置

   - ``location.host``，主机地址+端口号

   - ``location.hostname``，主机地址

   - ``location.port``，端口号

   - ``location.pathname``，返回的是目录path

   - ``location.protocol``，网络协议

   - ``location.search``，返回URL查询字符串。这个字符串以``?``开头，例如

     ```javascript
     // url =. https:www.baidu.com:8080/a/dir/im?id=55&username=hello
     location.search //返回id=55&username=hello
     ```

   - ``locartion.replace(url)``，也是重新定向网页的，区别于href在于，他不会产生历史记录，即回退按钮失效。

   - ``location.reload()``，重新加载当前页面，括号内提填写true，表示从服务器加载，否则可能会从本地缓存加载。 

5. ``history``，用户浏览记录。

   - ``history.back()``，该方法返回历史记录的上一步，即重写回退按钮的功能，可以指向别的url，而不是上一个浏览的页面。等价于``history.go(-1)``，返回上一个页面
   - ``history.forward()``，该方法跳转页面到下一步（表明他是以退回的页面），等价于``history.go(1)``
   - ``history.go(n)``，跳转页面到历史的记录。 

6. ``screen``对象，表示浏览器对象

   - ``screen.availWidth``，屏幕可用宽度（包括一切边栏等），``window.innerWidth``（窗口宽度）（不包括打开的边栏，例如打开chrome调试工具所占的空间)
   - ``screen.availHeight``，屏幕 可用高度，``window.innerHeight``（窗口高度）。 

7. ``navigator``，导航

   - ``navigator.userAgent``，属性用来识别浏览器名称、版本、引擎、操作系统信息

   

#### JavaScript 事件

``事件``是指浏览器上内容发生的变化，包括点击鼠标、移动鼠标、键盘键入以及页面加载等。 

``事件句柄``，事件句柄是用来接收可能发生的某一个事件，即``事件处理函数``。

绑定事件

```javascript
var btn = document.getElementById("btn");
btn.addEventListener('click',function(){    //添加事件监听器，即事件句柄。此处监听按键被点击事件
  // 事件处理函数
})
```

1. ``事件定义``

   - 在HTML元素中定义

     ```html
     <button onclick="alert('hello')" onload="alert('lalala')">按钮</button>
     <!-- 该方法可以实现效果但是，违反了行为与内容分离原则-->
     ```

   - DOM0 级事件定义方式，即从javascript中为``dom元素的属性赋值``，该方式只能绑定一个处理函数

     ```javascript
     document.getElementById("btn").onclick=function(){//.....}
     ```

   - DOM2 级事件定义方式，他是高级事件处理方式，一个事件可以绑定多个处理函数，多个事件处理函数并存执行。

     ```javascript
     var btn = document.getElementById("btn");
     btn.addEventListener("click",function(){},true)  
     // 有三个参数，第一个为要监听的事件，第二个为处理函数，第三个参数
     ```

2. ``事件移除``

   - DOM2 级事件移除方式

     ```javascript
     btn.removeEventListener("click",functionName, false); 
     ```

3. ``IE事件流``，用于IE8级一下的浏览器

   - ``attachEvent(event,function)``，IE的事件添加，类似于addEventListener

      ```javascript
     btn.attachEvent("onclick",function(){
       alert("IE8, click");
     });
      ```

   - ``detachEvent(event,function)``，移除事件

4. ``事件冒泡和事件捕获``

   - 事件捕获：从外向内传播。即一个事件同时满足父子事件触发的条件，父节点先执行处理函数。

   - 事件冒泡：从内向外传播。同理，子节点先执行处理函数。

   - 事件委托：

     ```html
     <ul id="ul">
       <li id="1"></li>
       <li id="2"></li>
       <li id="3"></li>
       <li id="4"></li>
     </ul>
     ```

     ```javascript
     var ul = document.getElementById("ul")
     ul.addEventListener("click",function(event){
       	alert(event.target.id);  
       //点击一个li，会弹出相应的id。事件委托的意思就是，将事件托付给其父节点设置，利用了冒泡机制
     },false);
     ```

5. ``event 对象常用属性和方法``，event必须通过``function(event){}``传入

   - event.type ：事件类型
   - event.target：事件触发的目标，可以为孩子节点上发生的事件，根据冒泡原理，在父节点出被截获
   - event.currentTarget：事件绑定的位置，即返回绑定处理函数的对象（该事件被触发）
   - event.preventDefault() ：该函数阻止系统自带的行为函数的执行，例如``<a>标签``的点击事件的默认跳转行为会被阻止。
   - event.stopPropagation()：该函数阻止事件继续传播。即阻断冒泡传播。
   -  event.clientY, event.pageY, event.screenY：分别代表，浏览器顶端到鼠标点击位置的距离，页面顶到鼠标位置，屏幕到该位置。 clientY，永远指浏览器到位置，pageY的位置可能会根据浏览器滚动轴的变化而变化
   - eventKey：包括event.shiftKey、event.ctrlKey、event.altKey、event.metaKey等，表示事件触发时同时按下的辅助按键。
   - ``event.button``
     - 正常``event.button == 0``鼠标左键，``event.button == 1``鼠标中，``event.button == 2``鼠标右键
     - IE8系列``event.button == 0``鼠标没有按下，``event.button == 1``鼠标主键，``event.button == 2``鼠标次键，``event.button == 4``鼠标中键，

   

6. ``IE8以下的浏览器event方法``

   - event.type : 事件类型
   - event.returnValue = false  ：等价于preventDefault()，阻止默认行为。
   - event.cancelBubble = true;  ：等价于stopPropagation()，阻止事件继续派发
   - event.srcElement  ：等同于target，返回事件触发源

7. ``事件类型``

   - ``load``，页面加载完成后在window上触发，也可以用于图片加载完毕，即image标签；也可以加载js文件

     ```javascript
     // 图片预加载
     var image = new Image()   //创建的图片对象会提前保存到内存中
     image.addEventListener("load",function(event){},false);
     image.src = "smile.gif";
     ```

     ```javascript
     // js动态加载
     var script = document.createElement("script") ; //产生了一个与加载script标签
     script.addEventListener("load",function(event){});   //添加js加载后的操作
     script.src="jquery.js"; //添加句加载js的原地址
     document.body.appendChild(script) ; //真正加载js
     ```

     ```javascript
     // css动态添加
     var link = document.createElement("link");
     link.type="text/css";
     link.rel="stylesheet"; 
     link.addEventListener("load",function(event){});
     link.href = "example.css";
     document.getElementsByTagName("head")[0].appendChild(link);
     ```

   - ``unload``，当一个页面卸载的时候执行

   - ``resize``，窗口大小发生变化时触发。窗口是指不包括浏览器菜单栏，边栏的部分

   - ``scroll``，窗口滚动时候触发

   - ``focus``，获取焦点时触发，该方法不支持冒泡

   - ``focusin``，获取焦点时触发，该方法支持冒泡

   - ``blur``，元素失去焦点时触发

   - ``click``，点击事件

   - ``dbclick``，双击事件

   - ``mousedown``，鼠标按下

   - ``mouseup``，鼠标松开

   - ``mousemove``，鼠标在控件上移动就触发

   - ``mouseout``，鼠标离开控件触发

   - ``mouseover``，鼠标进入控件触发

   - ``mouseenter``，鼠标进入控件触发，区别mouseover在于，mouseenter只能进入``目标元素``时触发。``目标元素``是指添加事件的对象，即``div.addEventLisener("mouseenter",function(){})``中的div，如果进入该div下的任何子控件则不触发。

   - ``mouseleave``，鼠标离开控件触 发，同理mouseenter

   - ``keydown``，任意键盘按键按下触发。配合``event.keyCode``，可以取得所输入的按键

   - ``keyUp``，释放某个键盘按键时触发

   - ``keyPress``，按下某个字符键，不包括回车、F11、delete等键。keyPress事件支持ASCII码的``event.charCode``。该charCode返回按下的ASCII码值。

   - ``textInput``，该事件用于input等输入框。当输入内容时候触发，配合``event.data``输入内容，使用。

   - ``DOMNodeInserted``，对象中有新元素插入触发。

   - ``DOMNodeRemoved``，表示当对象中有元素内容被删除，删除后触发。

   - ``DOMNodeInsetedFromDocument``，对象中有元素添加触发

   - ``DOMNodeRemovedFromDocument``，对象中元素被删除之前触发

   - ``DOMSubtreeModified``，表示对象中的结构发生任何变化时触发

   - ``DOMContentLoaded``，表示对象的dom树加载完成后就会触发，其速度非常快。区别于``load``（加载完成所有内容触发，包括图像、js文件、css样式），DOMContentLoaded会很快。

   - ``readystatechange``，它有四个状态``uninitialized未初始化``，``loading加载中``，``interactive可操作对象，但还没加载完成``，``对象已经加载完成``

   - ``hashchange``，只能给``window``对象添加，即浏览器的一个页面标签添加。即当浏览器url地址栏中的``#``号后面的值发生变化时触发。 配合``event.oldURL``和``event.newURL``使用

   - ``touchstart``，用于移动端，表示对象被touch时触发。``event.touches``表示当前触屏的触点数组、``event.changedTouches``，只包含引起事件的触点数组、``event.targetTouches``只包含放在元素上的触点信息。

   - ``touchmove``，用于移动端，表示手指在对象上滑动时触发

   - ``touchend``，用于移动端，表示手指离开对象屏幕时触发

   - ``touchcancel``，用于移动端，表示当系统停止跟踪触摸时触发

     

   

### 其他

1. 自封装，不污染整个环境

   ```javascript
   (function(){
     //函数体积
   })()   // 该定义的函数值本作用域内有效，不污染整个作用域空间
   // ()包括整个匿名函数的定义，匿名函数的定义返回一个函数的引用，然后后面紧接着一个()表示调用。
   ```

2. ie hack 条件编译

   ```javascript
   // IE 浏览器的条件编译，以 /*@cc_on  开始，以  @*/  结束，中间的部分，如果是IE浏览器就被执行，如果是其他浏览器就被跳过。例如：
   function(){   //经典写法
     if(!
        /*@cc_on!@*/     
       0) return;      //该if语句中间包含了一句IE条件编译语句（即只有IE能识别执行），这条语句在开始符号和结束符号之间
     						//只有一个  ！ 号（非号），然后结合外面的 !0，则非IE浏览器就等于!0，即直接返回return，
     						//后面的代码就被跳过。若是IE则被解析成 !!0，即if不成立，后面的语句其他功能就被执行了
     // 其他功能代码
   }
   ```

   ```javascript
   if(!+"\v1"){
      // IE 浏览器 不会转义 \v   解析结果为 "v1"， 自动类型转换为NaN
      // 其他浏览器，默认转义 \v 为一个垂直的制表符（类似于空格） 解析结果为 " 1"  自动类型转换为1
   }
   ```
   
   

