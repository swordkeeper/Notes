#  ECMAScript

ECMA script (ES) 是一种JavaScript的标准，而Javascript是标准下的实际实现。



### 基础语法

#### let 关键字 

- ``var age = 18``，在所在作用于中定义了一个age属性，若不在函数中，则等价于``window.age=18``

- ``age = 18``，没有指定var，等价于``var age =18``

- ``let age = 18``，let的作用于**块级**作用域，且不能被**重复声明**，不存在**变量提升**

    - 块作用域

        ```javascript
        {
            var a = 1;
            let b = 2;
        }
        console.log(a);
        console.log(b);
        // a 输出 1 ， b报错
        ```

    - 重复声明

        ```javascript
        var a = 1
        var a = 4   // 正常
        let b = 5;
        let b = 6;  // 报错，重复声明
        ```

    - 变量提升

        ```javascript
        console.log(a);    
        var a = "hahaha"
        // 正常输出,输出undefined，等价于
        //         var a;
        //         console.log(a)
        //         a = "hahaha"
        
        console.log(b);    // let 不进行变量提升，如上。所以报错，b为定义
        let b = "biubiubiu"
        ```

    - 暂存死区

        ```javascript
        {
            let a = "hahaha"
            {
                console.log(a)  
                // 输出报错，未声明定义。若本块作用域中没定义a，则可以输出。即本作用域中定义的同名 let变量会屏蔽该块，而他由定义在log之后，所以报错
                let a = "biubiubiu"
            }
            console.log(a)
        }
        ```

#### const 常量

各种特性都``let``相似，只是const不能被改变。

如果const 所指的是引用变量，例如列表等。那么更改列表中的值，还是可以被允许的。

``freeze()``函数，可以冻结引用变量。使得引用变量也不能更改

```javascript
const student1 = {
    age: 15,
    name:"xiaowang"  // 可以通过student1.name来更改
}
Object.freeze(student1)  // 冻结引用变量后，name就不能在更改了
```

ES6之前，定义常量的方式

```javascript
var CST = {} //定义一个引用了字典的变量
Object.defineProperty(CST, "BASE_NAME", {  // 向CST变量中定义一个值，名为BASE_NAME
    value: "小白",      // 值为小白
    writable: false     // 不可写入。即BASE_NAME的值不能被修改
})
Object.seal(CST)  // 封住CST，使得CST中不能添加或删除元素
// 结合seal和defineProperty 可以达到 ES6 的 freeze 效果
```



#### 解构赋值

- 数组

    ```javascript
    const attr = [1,2,3,4,5];
    let [a,b,c,d,e] = attr;
    const attr1 = ['a', 'b', ['c', 'd',['e','f','g']]]; //多层嵌套数组
    let [,b] = attr1; // b = "b"
    let [,,g] = attr1; // g = ['c', 'd',['e','f','g']]
    
    // 默认值
    let [a,b,c,d="ppp",e] = [1,2,3,4,5]; // 若匹配不到则d默认等于'ppp'，而不是undefined
    ```

- 对象

    ```javascript
    obj1 = {
        name:"xiaoming",
        age: 11,
        hoby: ["football", "basketball", "pc gmaing"]
    }
    const {name, age, hoby} = obj1   // 名称，即key必须相同
    const {name}  = obj1  // 只匹配一个变量的赋值 等等价于 const name = obj1.name
    const {hoby: [hoby1]} = obj1 
    // 取不到skill，只是取到skill下的列表中第一个元素，然后将列表第一个值赋值给skill1(列表中元素可以自己起名字)
    const {hoby:[hoby1,hoby2]} = obj1 // 取两个
    // 注意 ： 的作用，表示赋值，即取到对象中 hoby的内容，然后继续 解构赋值给 ：后面的部分
    ```

- 函数参数

    ```javascript
    function pc({   // 定义函数
                cup,   // 定义函数结构参数
                memory,
                hd=512,
                network
            }){
                console.log(cup);
                console.log(memory);
                console.log(hd);
                console.log(network);
            };
            
    my_pc = new pc({
        cup:"i9",
        memory:4096,
        network:"eth0"
    })
            
    ```



#### 模版字符串

通过``  ` ``而不是``""``或``''``定义的字符串就是``模版字符串``

```javascript
const xiaoming = {
   name:"小明",
   age: 14,
   say: function(){
       console.log(`my name is ${ this.name}`);
   }
}
xiaoming.say()  // 打印  my name is 小明
```



#### 字符串新方法

```javascript
"hello".repeat(3); // 打印  hellohellohello
"Banana".startWith("B"); // 返回true
"Banana".endWith("a");
```



#### for 循环

``forEach``  

```javascript
str1 = ["helloworld", "chenchen","sss"]
str1.forEach(function(thisStr){
    console.log(thisStr); // forEach循环，每次循环调用function，并把iterator 传递给thisStr
}); 
str1.forEach(function(thisStr, index){
    console.log(`The ${index} string is ${thisStr}`);
});
```

``for of ``

```javascript
str1 = ["helloworld", "chenchen","sss"]
for (let word of str1){
    console.log(word);
}
```



#### Unicode

正常的unicode 表示为``\u1f43``。通常情况下JavaScript读取到``\u``开头的字符串，就解析为unicode码点编码。

但是Javascript通常只会解析``0000-ffff``之间的unicode编码，超出的部分就不会解析。

例如``\u1f436``这个字符串，就会被解析成ὃ6，``ὃ``代表``\u1f43``在跟一个``6``。

ES6 中可以对unicode添加一个``{}``将其阔住，代表这是一个整体unicode编码字串。

例如：``/u{1f436}``，就表示🐶。

