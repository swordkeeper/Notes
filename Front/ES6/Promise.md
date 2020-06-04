# Promise

promise 函数，是针对**异步状态**下的回调函数的一种改进。它优化了JavaScript中异步的使用。

它形成了一个执行管道

### 定义一个promise

```javascript
let f = new Promise((resolve) =>{   //定义一个函数，传递一个回调函数（此处为一个匿名函数，该函数接受参数resolve
    setTimeout(function(){
        resolve();
    }, 1000);
});

f.then(function(){   // Promise.then方法，代表开始运行promise对象，回调函数也就被执行。此处的匿名函数传递给了resolve
    console.log(1);
});

function cb(){
    console.log(2);
}
f.then(cb).then(cb).then(cb); //将cb函数传递给resolve，并执行三遍

```

### 成功/失败

```javascript
function f(val){
    // do something  执行某程序
    return new Promise((resolve, reject) =>{
        // .... 判断 环境，选择执行resolve还是reject
        if (val){
            resolve();
        }else{
            reject();
        }
    })
}

function rrr(){
    console.log("执行失败");
}
function vvv(){
    console.log("执行成功");
}

// 测试失败
f(false).then(vvv, rrr);
// 测试成功
f(true).then(vvv, rrr);

```

返回失败调用``f.catch(() => {})``方法

### all/race

```javascript
let all_p = Promise.all([p1, p2, p3]) // p1, p2, p3 为三个返回promise对象的函数。
// 只有所有的都执行成功，才返回成功
let race_p = Promise.race([p1, p2, p3])  // 只要有一个执行成功，就返回成功
```



#### 状态

1. pending  进行中
2. Fulfiled  成功
3. rejected  失败