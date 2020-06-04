# Javascript 深入讲解



#### Instanceof

Typeof 返回的值有时候是区分不了``[]``、``{}``、``/a/``(正则式)。此时只能用``instanceof``

```javascript
console.log([] instanceof Array) //返回的是boolean值，[]数组是Array包装类的实例
// instance 不能检测基本数据类型，只能检测引用类型。检测基本类型都会返回false
```



#### 作用域

JS中``没有``**块级**别作用域，只有**全局作用域**、**函数作用域**。

- 没有用``var``声明的都是全局作用域
- 不在``函数内部``且用``var`` 声明的变量也是全局作用域

```javascript
if(true){
  var xx = "hello world";
}
console.log(xx);  // 此处仍能访问到xx，因为js没有块级作用域，xx被当成全局变量对待。
```

#### 作用域链条

JS中变量的作用域会沿着函数内部一层层向外查找。

```javascript
var name = "小明" ; // 全局作用域中的name， 也等价于window.name
function fn(){
  var name = "小李"; // fn 作用域下的name
  function fn1(){
    var name = "小白";  //fn1 作用域下的name     
  }
}
```

JS作用域的操作：

- 内部使用变量名时，会根据作用链向上，向外查找。
- 外部的不能使用内部作用域下的局部变量
- "小白" -> “小李" -> "小明" 的顺序查找name
- 有时候全局作用域的链条太长，为了减少查找，提高效率，会将最上层的window当作一个变量传递给最内部的函数，来加快变量访问。

#### 预解析

- 以``var``声明的变量会预解析，并赋值undefined
- 以``let``声明的变量不会预解析
-  ``函数声明``会预解析 ，并赋值为函数体的内容
- ``函数表达式``不会预解析
- 当出现函数名和普通变量名``同名``时，在预解析时，函数名覆盖普通变量
- 预解析是分标签进行的即不同的``script``标签中的预解析不一样

预解析的时机在``执行所有代码之前``

```javascript
console.log(a);
var a = "xxh";
// 该代码，1. 先执行预解析（因为var定义的a）并把这些预解析变量赋值为undefine，
//        2. 再顺序执行代码，console.log(a)， 因而打印undefine
//        3. 再执行a = “xxh”
```



### Javascript 函数/对象

Javascript 中的函数其实是一个``对象``，可以有自己的方法和属性



#### 定义创建函数/对象的方式

1. ```javascript
   function add(num1, num2){
     //body
   }
   var add1 = function(num1,num2){
     //body
   }
   ```

2. ```javascript
   var func = new Function("num1","num2","return num1 + num2"); //构造函数的参数必须以字符串形式传递
   alert(func(2,5))  //返回7
   // 该方法使用系统级别的Function来构造函数。他有两个条件：1.参数可以无限多，2.最后一个参数必须是一个函数体
   var func1 = new Function(var1,var2,..., functionBody()); //var1 var2...最后都会传递给functionBody，当成其自身变量。
   ```

Javascript 函数最大的特点就是，它是一个``对象``

```javascript
function add(num1,num2){
  return num1+num2;
}
add.year = 1999;
add.setTime = function(time){
  this.time=time;
  return this.time;
}
console.log(add(2,3));
console.log(add.year);
console.log(add.setTime(25)); // 这三个log都能正常输出，说明function 就是一个对象
```

3. 工厂模式创建对象

   ```javascript
   function createObject(name,age){
     var obj = new Object();
     obj.name = name;
     obj.age = age;
     obj.run = function(){
       return this.name + this.age + "运行中...";
     }
     return obj;
   }
   ```

4. 原型模式创建

   ```javascript
   function test(){};  //首先创建一个空的函数
   // 然后修改这个函数的原型，因为函数也是一个对象，prototype属性返回的是该函数的对象
   test.prototype.color = "red";
   test.prototype.height = 18;
   test.prototype.fn = function(){alert(this.name)};
   var obj = new test();     
   // 该模式主要特别之处在于修改函数原形
   ```

5. **创建对象的标准方法，类似于class**

   ```javascript
   function student(){   //类似于类名student
     var info = {}   // var 定义的变量外界都访问不到，类似于private 关键字
     function _set(name,age){   //嵌套定义的function 是私有 private 方法，外界定义的对象也访问不到。
       info.name = name;
       info.age = age;
     }
     this.set = function(){   //this.set 定义了一个跟对象相关的方法， 只有这个方法才能被创建出来的对象塞调用.
       	return _set;      //此处是一个闭包，它返回了外界调用不到的  _set方法，和外界接触不到了的info信息
     }
   }
   
   //. 创建对像
   var stu = new student();
   stu.set()("张三","18");
   ```

   

#### 函数调用

```javascript
var person = {};
person.name="xiaoming";
person.getName=function(){
  this.name;  // 此处的this返回person对象本身
}
person.getName() //直接加括号 就回调用person的getName方法，此时的this指的是person本身

////////// Call 方法调用
person.getName.call(window); // 可以通过一个方法名的.call方法来调用该方法。
// .call方法接受一个参数，这个参数会替换 this 用来访问对象，因而此处时访问的是 window.name值

// .call 方法和 .apply 方法都是函数的间接调用，唯一不同是，.call后面的参数可以是多个，.apply第二个参数
// 是一个参数列表
// 如下
function add(num1,num2){
  return num1+num2;
}
add.call(window,1,2) //第一个参数还是对象，后面的参数可以是多个，可以一直写入
add.apply(window,[1,2]);  // apply 的第二个参数是一个列表
```



#### 函数原型``prototype``

任何函数都是对象，都有一个prototype属性，他指向内存中的一个地址



#### Javascript 闭包

由于js中只有两种作用域，``全局作用域``和``局部作用域``。``局部作用域``只有在函数中存在。

因而函数中定义的``var``变量，函数外是无法访问的。因而闭包就是为了``传递出去该内部变量``，并使该``变量长期存在内存中``。

```javascript
function func1(){
		var test = 1;         //函数func1外面是看不到test的内容的。
    function fn(){        //因为函数名fn和test同一级别，因而能看到test。
      	return test;
    }
   return fn;            // 返回该函数fn，从而可以通过调用得到test的值。
}
var g = func1();
g();    //g() 返回test的内部值，并且test长期存在于内存中
```



#### Javascript 继承

 Javascript 通过``prototype（对象属性）``和类``__proto__（内置属性）``来实现继承。方法为``p.__proto__`` = ``obj.prototype``

下面实现一个继承:

```javascript
function person(name,age){ //定义一个 “人” 父类
  	this.name = name;     //父类的属性，会被继承
  	this.age = age;
} 
person.prototype.speaking = function(){   //人会说话，父类方法，也会被继承
      alert(this.name+" are speaking"); 
}

function student(sid){   //定义一个学生类，继承 ”人“这个父类
  	this.sid = sid			 //学生的id
}
student.prototype = new person("小明", 18)   // 设置继承关系，student类继承person类，
// 因此需要设置student对象的prototype属性指向“人”类的对象。
student.prototype.grade == 5 //定义学生自己的prototype内容，表示小明是5年级学生

var stu = new student(2822); // 创建一个student对象
```

1. JavaScript的继承，是通过prototype实现的，如果一个类对象要使用``某个变量``，它会先通过本类中定义的变量查找，如果找不到则在通过其内置的``__proto__``属性查找。

2. 以为在一个类对象初始化的时候，默认系统会进行``p.__proto__=.obj.prototype``操作，因而第一步就是在``prototype``中查找，也就是继续在``person``类中查找相应的变量。

3. 这种向上级类``prototype``链式的查找就是``原型链``

4. 比如调用``stu.name``，因为stu中没有name属性，因而原型链为

   ``stu对象中找name（没有）---> stu.__proto__找 ，因为stu.__proto__ = student.prototype （该步骤是创建对象时赋值的）,也就是到student.prototype 中查找 --> 也就是到new person中查找，该对象中有name``





