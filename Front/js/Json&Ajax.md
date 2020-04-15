 # Ajax and Json

## Ajax (Asynchronous Javascript and XML)

异步Javascript 和 XML 结合体。 **用于动态跟新网页局部**

#### Ajax 优点

1. 通过异步模式，提升用户体验（JavaScript为单线程，Ajax可以使用户不为单线程执行进行排队）
2. 优化了浏览器和服务器之间的传输，减少了非必要的数据传输
3. Ajax引擎再客户端运行，承担了部分服务器的工作，减少了服务器负载。

#### Ajax的缺点

1. 不支持浏览器``back``回退，因为他是局部操作
2. 暴露的Ajax与服务交互的一些细节
3. 对搜索引擎的支持比较弱

#### XMLHttpRequest 对象

- 可以向服务提出请求并响应处理，并且不阻塞用户
- 可以在页面加载后对页面进行局部更新

#### 使用Ajax

1. 创建XMLHttpRequest对象，也就是创建一个异步调用对象

   ```javascript
   // 创建XMLHttpRequest 
   function createXHR(){
     if(typeof XMLHttpRequest != "undefined"){   
       //针对IE7 Firefox opera safari chrome浏览器，判断浏览器是否支持XMLHttpRequest对象
       return new XMLHttpRequest();
     }else if(typeof ActiveXObject !="undefined"){   //针对老式IE浏览器 进行判断
         // 获取常用的版本对象，老版本IE对于该对象的名字不同。
         var xhrArr = ["Microsoft.XMLHTTP","MSXML2.XMLHTTP.6.0"
                      ,"MSXML2.XMLHTTP.5.0","MSXML2.XMLHTTP.4.0"
                     ,"MSXML2.XMLHTTP.3.0","MSXML2.XMLHTTP.2.0"]
        // 然后遍历创建，哪个存在创建哪个
         var len = xhrArr.length;
         var xhr;
         for(var i = 0;i<len;i++){
   				try{
             	// 创建XMLHttpRequest对象
             	xhr = new ActiveXObject(xhrArr[i]);  //存在则创建，并break终端继续循环
             	break;
           	}catch(ex){}
         }
         return xhr
     }else{
       // 不支持XHR 
       throw new Error("No XHR object available");
     }
   }
   var xhr = createXHR();
   ```

   

2. 创建一个新的HTTP请求，并制定HTTP请求的方法和URL

   - ``open(method, url, async)`` ，open打开一个http请求，不会向服务器发送真正的请求，它相当于初始化一个请求并准备发送。URL和协议端口不能发生改变。URL是必须的参数

   - ```javascript
     // open 打开并创建HTTP请求
     xhr.open("get",'./URL.....',true);
     ```

3. 设置响应HTTP请求状态变化的函数

   - ```javascript
     //响应XMLHttpRequest对象状态变化的函数，onreadystatechange 在其属性发生改变时触发。并执行响应的函数
     xhr.onreadystatechange = function(){
         // 异步调用成功
         if(xhr.readyState === 4){    //readyState =4 表示异步调用成功，响应内容解析完成
            //..  成功调用时的代码
              if(xhr.status >= 200 && xhr.status <300||xhr.status==304){ //表示页面响应代码
     					// 此时获得服务器返回数据 .......
              }
         }
     }  
     ```

4. 发送Http请求

   - ``send(string)``，string参数只在POST情况下有用，get时候null即可

   - 如果用``POST``方法send数据，需要填写HTTP头，方法如下

     - ``xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded")``

   - ```javascript
     xhr.send(null);  //GET方法时可以如此使用
     //or
     xhr.send({user:"zhangsan",passwd:"123456",date:"20110201"}) //POST方法时可以如此使用
     ```

5. 获取异步调用返回数据

   - 客户端在获取到HTTP请求响应的返回数据后，有四个属性会被填充

     - ``responseText``，从服务器进程返回数据的字符串形式
     - ``responseXML``，从服务器进程返回的DOM兼容的文档数据对象
     - ``status``，从服务器返回的数字状态码，如404，200
     - ``statusText``，伴随状态码的字符串信息 

   - 获取局部数据，该方法应该写在状态变化函数之中，即第三步处

     ```javascript
     var data;
     if(xhr.status >= 200 && xhr.status <300||xhr.status==304){ //表示页面响应代码
     		data = JSON.parse(xhr.responseText);			
     }
     ```

6. 使用JavaScript和DOM实现局部刷新

   - ```javascript
     var responseObj = eval("("+xhr.responseText+")") 
     // 之所以用eval函数，是因为reponseText返回的是string 类型
     // 之所以再用() 包裹，是因为防止字符串边界出错。此种方法是最一般的方法。
     ```

   - ``JSON``包装对象

     - ``parse()``方法，用法``JSON.parse()``，用于将JSON字符串转换成对象
     - ``stringify()``，用法 ``JSON.stringify()``，用于将一个值转换成字符串

   -  局部DOM刷新

     ```javascript
     function RefreshLocalWebPage(){
       // $(div). ...... data.username. data.password. .. .......
     }
     ```

     





# Json

JSON（Javascript Object Notation），是一种数据交换的文本格式。

Json包含三种数据类型的值：

- 简单值：字符串``"xxx"``（必须双引号）、数值``14``（十进制，且没有NaN和Infinity）、布尔值、null、不支持undefined
- 对象，即一组有序的键值对。**对象的键名不像Javascipt那样可单独出现，其必须放在引号里面**
- 数组，用``[]``扩起来，成员用``,``隔开





# Jquery 和 Ajax

1. ``$.ajax()``

   ```javascript
   $.ajax({    // $.ajax 直接封装了好多方法，使得jquery可以一步到位的执行ajax方法
     url:"xxxxxx",
     type:"post",
     async:true,
     dataType:"json"  // 数据类型
     success:	function(responseDate){ // 请求成功后的回调函数
     					console.log(responseDate)   //responseDate就是成功后获取解析后的Json对象
     					$.each(responseDate,function(index,obj){})  //通过jquery遍历所有json成员对象
   				}    
   })
   ```

2. ``$.get()``

3. ``$.post()``

4. ``$.getJson()``





# 跨域

- ``同源``：如果一个资源的``协议``、``域名``、``端口号``都相同，则被称之为同源

- ``跨域``：如果上面三个有一个不同，却要访问，则被称之为跨越

- 解决跨域的4种方法

  - 跨域资源共享（CORS）
  - 使用JSONP
  - 修改document.domain
  - 使用window.name

- JSONP

  JSON with Padding，是一种新的Json使用方法，用于解决跨域问题。

  JSONP包括两个部分：``回调函数``和``数据``，分别代表响应来时页面调用的函数和传入回调函数中的Json数据。

  JSONP``原理``：在页面上引入不同域的js脚本，来实现XMLHttpRequest请求不同域的效果。

  JSONP使用步骤：

  1. 通过script标签引入js文件
  2. js文件装在成功
  3. 执行我们在url参数中指定的函数 

  JSONP使用实例：

  ```html
  <div>
    <img src="http://www.baidu.com">  <!-- 只有src属性，或者href属性是可以访问别的域名资源的 -->
    <!-- JSONP 正是利用了 src属性可以访问其他域的特性来实现，跨域访问数据的功能的 -->
  </div>
  ```

  ```javascript
  // 实际的过程如下
  // 比如说我们要访问。   http://www.baidu.com?callback=def
  // 我们url就可以写成 http://www.baidu.com?callback=def
  // 这样百度服务器就收到这个url后，就会返回一个回调函数def，并给def传入应有的参数，即def(json)
  // 所以我们写的内容就是要访问的url，以及需要使用的那个callback函数 
  
  // 1. 封装JSONP函数
  function getJsonp(url, callback){   //接受两个参数，一个为跨域访问的地址，另一个为回调函数
      if(!url) return ;  // 如果没有写地址，则不用操作
    
      // 随机生成一个函数名
      var tmp = ['a','b','c','d','e','f','g','h','i','g','k'];
      var r1 = Math.floor(Math.random()*tmp.length);
      var r2 = Math.floor(Math.random()*tmp.length);
      var r3 = Math.floor(Math.random()*tmp.length);
      var randomFunctionName = tmp[r1]+tmp[r2]+tmp[r3]; //拼接出一个随机的字符串函数名
      var callbackName = "getJsonp." + randomFunctionName; //此为一个getJsonp的回调函数名，名称随机，作为getJsonp函数的一个属性
    
  // 2.定义好了一个随机的函数名之后，就要修改url,即拼接成正确的请求数据url
      if(url.indexOf("?")===-1){
         url += "?jsonp=" + callbackName;  //传入的是一个回调函数名
      }else{
        url += "&jsonp=" + callbackName;
      }
    
  // 3. 动态创建script标签
    var aScript = document.createElement("script");
  
  // 4. 定义回调函数
    // 因为上面2.时已经声明回调函数，所以现在就可以定义该函数的具体功能
    getJsonp[randomFunctionName] = function(data){
        try{   // try的原因在于，可能服务器无法解析jsonp请求
            callback && callback(data) // 在回调函数中，执行用户传入的具体callback函数
        }catch(e){}finally{
            delete getJsonp[randomFunctionName]; //删除对应的随机函数名，否则太多会造成卡顿
            aScript.parentNode.removeChild(aScript); //同理标签
        }
    }
    
    // 定义完毕，添加出script标签3
    aScript.src = url;
    document.getElementsByTagName("head")[0].appendChild(aScript);
    // 这样在head里面就会添加一个script标签，并且url = 域名？jsonp=随机函数名
    
  
  }
  
  //5. 调用封装好的jsonp函数
  getJsonp("http://www.baidu.com/abc/1.json",function(data){
      console.log(data);
  })
  ```

  

  

