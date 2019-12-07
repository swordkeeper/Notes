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



### Javascript 函数

Javascript 中的函数其实是一个``对象``，可以有自己的方法和属性

定义函数的方式

1. ```javascript
   function add(num1, num2){
     //body
   }
   var add1 function(num1,num2){
     //body
   }
   ```

2. ```javascript
   var func = new Function("num1","num2","return num1 + num2"); //构造函数的参数必须以字符串形式传递
   ```

Javascript 函数最大的特点就是，它是一个``对象``

```javascript
function add(num1,num2){
  return num1+num2;
}
add.year = 1999;
add.setTime = function(time){
  this.time=time;
}
console.log(add(2,3));
console.log(add.year);
console.log(add.setTime(25)); // 这三个log都能正常输出，说明function 就是一个对象
```

