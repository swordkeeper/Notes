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
    // 取不到hoby，只是取到hoby下的列表中第一个元素，然后将列表第一个值赋值给hoby1(列表中元素可以自己起名字)
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
            
    function add(a, b = 999){
        //函数b可以接受一个默认值
    }
    function add(a, b = 1+a){  // 默认赋值也可以进行简单运算
        console.log(b)
    }
    ```

- 属性解析

    ```javascript
    const key = "age";
    const student = {
        name :"xiaoming",
        [key]:15;
    }  // 则student对象有两个属性，name 和  age
    
    // 合并对象
    Object.assgin({a:1, b:2}, {name:"hahah"}, {age:144}); 
    // 返回{a: 1, b: 2, name: "hahah", age: 144}
    ```

- 数组展开

    ```javascript
    function foo(a,b,c){
        console.log(a,b,c);
    }
    foo(1,2,3); // 可以这么点用
    foo(...[1,2,3]) // 也可以传递一个数组参数，然后将其展开传递
    
    
    const arr1 = [1,2,3,4];
    const arr2 = [5,6,7,8];
    const arr3 = [...arr1, ...arr2];  // 合并多个数组
    const arr4 = [...arr1]   // 复制一个数组
    
    // 生成器函数。调用生成器，返回一个生成器对象.next()，继续返回{value:true , done: false} 值
    function *g(){  // 加 * 号，表示一个生成器
        yield "1";
        yield "2"
    }
    const attr5 = [...g()]; // 返回 ["1", "2"]
    ```

- 数组类

    ```javascript
    const obj = {  // obj为一个对象，不是数组
        0:1,
        1:3,
        2:false,
        length:2
    }
    Array.from(obj)  //该函数从obj 类数组对象中构造，数组[1,"22"]。因为length为2，所以只有两个
    Array.from(obj, item => item*2) // 该函数还能定义方法，类似于map。返回数组 [2,6]
    
    Array.of(1,2,3,false,"chc") //从值中构造数组，返回[1,2,3,false,"chc"]
    
    //制造空数组
    let arr = new Array(10) // 有占位符，但是没有值的10个空间
    let arr1 = new Array(10).fill(0) // 构造一个全部为0的数组
    
    for(let [k, v] of ["hello","world"].entries){ //返回键值对。还有[].keys 和[].values方法
        console.log(k, v)
    }   
    
    [1,2,3,4,5].includes(44); // 判断是否包含
    ```

    

#### 箭头函数

```javascript
// 语法格式为   () => xxx;

const add1 = (a, b) => a+b;   // 定义一个名为add1 的函数，参数为(a,b)，返回a+b
const add2 = function(a,b){return a+b;}  // 两个等价

const add1 = (a,b) => {
    a += 1;
    return a+b;
}

const pop1 = arr => arr.pop(); // 定义一个名为pop1的函数，参数为一个Array，返回arr.pop()的值，即弹出值，同时arr中内容减少
// 若只想pop，不想接收返回值，可以添加一个void参数
const pop2 = arr => void arr.pop(); 

// 箭头函数 没有 arguments 属性  // 这点很重要，用于闭包取消this 作用域
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

例如：``\u{1f436}``，就表示🐶。

获取unicode编码

```javascript
let myUnicode = '🐶'.codePointAt(0).toString(16); // 获取🐶的码点，然后在转换成16进制字符串
```



#### 正则表达式

```javascript
const regex1 = /^a/g;
const regex2 = new RegExp('^a', 'g');
const regex3 = new RegExp(/a/g);   // g 表示全局匹配，即匹配到了，仍然继续匹配，知道字符串结尾
```

- ``g``修饰符，全局修饰符

- ``i``修饰符，忽略大小写

- ``u``修饰符，表示unicode

    ```javascript
    console.log( /^\ud83d/.test('\ud83d\udc36') )  
    // 返回true，用正则式 /^\ud83d/ 来匹配'\ud83d\udc36'，结果匹配上了，因为js 把他当成字符匹配了。而不是unicode匹配
    console.log( /^\ud83d/u.test('\ud83d\udc36') )  
    // 返回false，因为\ud83d\udc36其实是一个unicode编码字符，所以匹配不上（unicode \ud83d\udc36 和  \ud83d 不同 
    ```

- ``y``修饰符，粘连修饰符

    ```javascript
    const r1 = /imooc/g;
    const r2 = /imooc/y;
    const str = "imoocimooc-imooc";
    console.log( r1.exec(str) )  // 返回 3个匹配
    console.log( f2.exec(str))   // 匹配到两个imooc，因为匹配完第二个后，发现后一个字符为 - 而不是imooc。因而匹配失败。这就是粘连。即不中断，不跳跃的匹配
    ```



#### 新进制表示

```javascript
let a = 123;
let b = 0xff;
let c = 0o77; // 8进制
let d = 0b111; // 2进制
console.log(a);
console.log(b);
console.log(c);
console.log(d);

// 幂运算
console.log(2**10) // 1024
console.log(2**10**0) // 输出 2 ，幂运算 右结合优先级

console.log(Number.parseInt("123.3")); // 123   这些函数现在挂载在Number下了
console.log(Number.parseFloat("123.3"));
console.log(Number.isNaN(NaN)); 
console.log(Number.isFinite(123))
console.log( Number.isSafeInteger(123123) )  // 判断一个值是否处在能精确表示的整数范围内
```



#### reduce

```javascript
function sum(...nums)  // 通过 ...来聚合获得的参数 使得nums为一个数组
{
    return nums.reduce(function(a,b){
        return a + b;
    }, 0);      // reduce( function, initial_number)
}
sum(1,2,3,4,5) // 返回 15
```

