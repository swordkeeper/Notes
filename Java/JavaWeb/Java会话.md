# 会话 Session

### 会话概念

1. 会话：一次会话中，可以包含多次请求和响应

2. 生命周期：浏览器第一次给服务器资源发送请求，会话建立，直到一方断开

3. 功能：在一次会话范围内，``共享数据``
4. 方式：
    - 客户端会话技术：``cookie``
    - 服务器端会话技术：``session``



### Cookie

#### 概念

客户端会话技术，将数据保存到客户端



#### 使用步骤

1. 客户端第一次访问时，创建Cookie，绑定一些数据

    ```java
    new Cookie(String name, String value); // 创建cookie，并赋值参数，cookie名和cookie值
    ```

2. 服务器响应客户端时，携带（发送）Cookie对象

    ```java
    response.addCookie(Cookie cookie);  // 通过响应对象发送给客户端
    ```

    实际上，服务器发送cookie是通过响应头中``set-cookie:msg=hello``这么一串数据，来实现。

    这串响应头会被浏览器解析，并保存为一个文件（cookie文件），其中包含msg=hello这串cookie数据

3. 客户端第二次访问服务器时，携带cookie，和请求一起发送给客户端

    ```java
    Cookie[] request.getCookies()  // 从请求中获得Cookie，由于Cookie可能有多个，所以返回的是一个数组
    ```

    实际上，请求携带cookie数据是通过，浏览器在请求头中设置数据``cookie:msg=hello``，来实现。



#### Cookie细节

1. 一次可以发送多个cookie

    ```java
    Cookie c1 = new Cookie("msg1", "hello");
    Cookie c2 = new Cookie("msg2", "world");
    response.addCookie(c1);
    response.addCookie(c2);
    ```

    实际发送的响应头为``Set-Cookie:msg1=hello``，``Set-Cookie:msg2=world``两个相同key的响应头数据

    服务器接受携带cookie返回时， request header为``Cookie:msg1=hello; msg2=world``

2. cookie在浏览器中保存时间

    - 默认情况，浏览器关闭，Cookie被清除。此时Cookie数据存在内存中，不生成对应的文件
    - 持久化存储： ``cookie.setMaxAge(int seconds)``，表示让cookie在浏览器方存储，让Cookie在浏览器方生成文件存储。 参数，正数表示存储秒数，负数表示设置cookie为默认情况，0表示服务器建议浏览器删除cookie信息

3. cookie中有没有中文

    - JDK8.5以后可以存储中文，但是特殊字符还是不支持，例如空格，换行等。所以建议还是用``URL编码``转换

    - JDK8.5以前一般需要转码，转码通常采用``URL编码``，格式%E2%A2%9A......

    - ```java
        // 对cookie.getValue()的值进行URL编解码
        URLEncoder.encode(str, "utf-8"); // 用URLEncoder类对str进行URL编码
        String value = URLEncoder.decode(backStr,"uft-8");  // 对backStr解码
        ```

        

4. cookie获取范围有多大

    - 默认情况下，cookie不会在多个项目之间共享，只会在一个项目下多个服务之间共享

    - 相同服务器，但是项目不同。通过``setPath(String path)``设置cookie的获取范围。默认情况下，path等于某个项目的虚拟目录，即只能在所对应的项目下共享。若要让其他的项目也能获取该cookie信息，可以更改该Path信息。例如，让所有项目都能访问某个cookie

        ```java
        cookie.setPath("/");    // 设置cookie的访问范围为/，这样忽略了项目的虚拟目录。这样所有的项目都能获取该cookie
        ```

    - 不同服务器，通过``setDomain(String path)``，设置cookie范围。如果设置的一级域名相同，那么多个服务器之间就能共享cookie

        ```java
        cookie.setDomain(".baidu.com");  // 设置百度一级域名为domain的范围，这样不同服务器（分布在不同域名）例如
        // tieba.baidu.com   和   news.baidu.com 就能共享该cookie
        ```



#### Cookie的特点和作用

1. Cookie存储数据在客户端，安全性较差
2. 浏览器对于单个cookie的大小有限制(4KB)，以及同一个域名下的总cookie数量也有限制，大约20个左右。
3. 因而cookie不能存储大数据。适用于存储少量，不信息敏感的数据