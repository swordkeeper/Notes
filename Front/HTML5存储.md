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

使用方法：

- ```javascript
  localStorage.setItem('key1',"value1");  //存储一个键值对
  localStorage.getItem("key1"); // 返回value1的内容
  localStorage.key2="value2";   //另一种方式设置键值对
  localStorage.removeItem("key1"); //移除一个键值对
  
  ```

- 





