# HTML5 存储

---


移动端使用``无线网络``，网路不稳定的情况时有发，所以有情景需要存储到本地。 也有可能存储的原因是``离线应用``。

``Cookie``，大小最大4K，单个域名下只能存储50个左右，污染请求头部，浪费流量，基本被淘汰

### WebStorage 网络存储（本地存储）

包含两个``localStorage``和``sessionStorage``

```javascript
console.log(window.localStorage);
console.log(window.sessionStorage );
```

####  使用方法：

- localStorage，储存客户端浏览器端，大小2～15MB

  ```javascript
  localStorage.setItem('key1',"value1");  //存储一个键值对
  localStorage.getItem("key1"); // 返回value1的内容
  localStorage.key2="value2";   //另一种方式设置键值对
  localStorage.removeItem("key1"); //移除一个键值对
  localStorage.clear();  //清除所有存储内容
  ```

  

- sessionStorage，浏览器结束后删除，大小不限制

  ```javascript
  // 同理
  ```

#### 注意事项

1. 存储大小超出限制，会抛出``QuotaExceededError``错误，用trycatch捕获
2. 仅能``存储字符串``
3. 刷新页面不能使sessionStorage失效，相同url不能区别同一个session，只能是不同的标签页



#### 定时机制

```javascript
localStorage.setItem("timer",new Date().getMinues() + Number(3)); //存储当前时间+3分钟，则表示三分钟后过期 
```





### IndexedDB (html5数据库)

html5提供的一个特殊的数据库

1. 创建/打开数据库

   ```javascript
   var myIndexedDB = indexedDB.open("testDB",1) //两个参数，第一个为打开数据库名，第二个为版本号码，版本号只能升，不能降 
   ```

2. 创建表

   ```javascript
   var db = myIndexedDB.result;        //拿到打开数据库对象中的result对象 该函数应该被包含在myIndexedDB.onupgradeneeded 方法之中，否则会报错显示数据库连接未完成
   db.createObjectStore("firstTable",{autoIncreament:true}); 
   //创建objectStore 对象，参数为表的名字。第二个参数表示设置一个自增主键，即自己生成一个主键
   //第二个参数还可以成如下
   db.createObjectStore("secondTable",{keyPath:"studentId"});  //自定义一个主键，主键为studentId。
   
   ```

3. 打开表，添加表数据

   ```javascript
   var trasaction = db.transaction("firstTable","readwrite"); 
   //以读写模式打开第一个数据库事物，transaction表示打开表 
   // 如果是读写多个表，则打开时需要将["firstTable","secondTable"]传入第一个参数
   
   var store = trasaction.objectStore("firstTable"); // trasaction执行存储表方法，并传入存储的表名字
   var json = {   //设置要存储的数据
     id:123,
     "name":"halo",
     "age":18
   }
   store.add(json)  //将表格写入表firstTable中
   ```

4. 取数据

   ```javascript
   var getRequest = store.get("keyId");
   var getAllRequest = store.getAll();   //获取所有数据
   getRequest.onsuccess=function(){   //查询数据成功以后执行
       console.log(getRequest.result);
   }
   ```

5. 修改数据，添加数据，删除数据

   ```javascript
   store.put({
     id:222,
     "name":"cc",
     "age":12
   });
   store.delete("keyId");
   store.clear();  //删除所有数据
   ```

6. 创建索引/使用索引

   ```javascript
   store.createIndex("indexName", "name",{
      unique:false;   //索引字段是否是unique的
   })
   //创建一个索引，第一个参数为索引名字，第二个参数为索引字段名称，第三个参数为索引匹配参数
   
   // 使用索引
   var myIndex = store.index("indexName");  //获得索引对象
   myIndex.get("Ray"); //通过索引获得某个字段
   ```

7. 使用游标

   ```javascript
   var cursor = store.openCursor(IDBKeyRange.only("keyId"),"prev");  //参数为一个范围函数 
   // 其他范围函数有： upperBound(x),  upperBound(x,true), lowerBound(y), lowerBound(y,true)
   // 第二个参数为 顺序查询，或者逆序查询。prev next
   cursor.onsuccess=function(){
     console.log(cursor.result.value);
     cursor.continue() // 游标继续运行
     //当游标搜索到某个确切值时，可以改变数据
     if(cursor||cursor.result.value.name="Ray"){ //游标不为结尾的undefine 且为ray
          cursor.update({
               id:123,
             "name":"Ray",
             "age":20    //更改该条数据 
          })；
          // 或者删除该条数据
          cursor.delete();
     }
   }
   ```

   

### 方法列表

1. ``onsuccess=function(){}``，创建数据库成功
2. ``onerror``，创建失败
3. ``onupgradeneeded``，当版本号需要升级了之后，执行



### 注意事项

1. 需要给数据库连接添加````onupgradeneeded````事件，并将所有的东西写入次事件的回调函数中，因为数据库请求会滞后。
2. 需要个transaction设置一个定时器，即``setTimeout``，否则下次刷新回显示transaction仍在链接中



