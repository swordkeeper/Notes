# 会话 Session

##  会话概念

1. 会话：一次会话中，可以包含多次请求和响应

2. 生命周期：浏览器第一次给服务器资源发送请求，会话建立，直到一方断开

3. 功能：在一次会话范围内，``共享数据``
4. 方式：
    - 客户端会话技术：``cookie``
    - 服务器端会话技术：``session``



## Cookie

### 概念

客户端会话技术，将数据保存到客户端



### 使用步骤

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



### Cookie细节

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



### Cookie的特点和作用

1. Cookie存储数据在客户端，安全性较差
2. 浏览器对于单个cookie的大小有限制(4KB)，以及同一个域名下的总cookie数量也有限制，大约20个左右。
3. 因而cookie不能存储大数据。适用于存储少量，不信息敏感的数据





## Session 

### 概念

服务器端会话技术，在一次会话的多次请求之间共享数据，将数据保存在服务器端的对象``HttpSession``中。



### 使用步骤

1. ``HttpSession``对象中的方法，该方式类似于域对象：

    - setAttribute
    - getAttribute
    - removeAttribute

2. 获取``HttpSession``对象

    ```java
    HttpSession session = request.getSession();
    ```

3. 设置数据

    ```java
    session.setAttribute("msg","hello");   // 一旦执行，数据msg=hello session的一个属性就被存储到服务器中了
    ```

4. 获取数据

    ```java
    Object obj = session.getAttribute("msg");  // 获取数据
    ```



### 原理

``Session`` 技术是依赖于``cookie``的。

1. 在第一次获取session时，由于请求是没有携带cookie。则服务器会在创建session时会创建一个cookie。 其内容为（响应头格式）``Set-Cookie: JSESSIONID=3126a1239``。即创建一个Cookie，并设置SeesionId等于某一个值，来作为cookie的内容。

2. 浏览器收到该cookie。当第二次访问时，则携带并发送该cookie

3. 服务器第二次收到请求时，由于携带cookie。服务器解析该cookie，并找到该SessionId。对比服务器中存储的SessionId（内存中）。这样服务器就知道某个访问是否是同一个Session会话。

    

### Session 细节

1. 客户端关闭，服务器不关闭，两次Session默认情况下``不是同一个``。由于其依赖Cookie，默认情况下，关闭浏览器Cookie就被销毁了。
2. 客户端不关闭，服务器关闭或重启，两次Session默认情况下``不是同一个``。由于服务器的SessionId是在内存中的，服务器重启，会使得客户端的Session数据与服务器中存储的SessionId匹配不到。这样所有客户端Session 全部失效。下面是解决该问题的方式（存储到硬盘），``钝化``和``活化``：
    - 钝化，存储服务器内存中的SessionId到磁盘上。钝化文件到``work``临时文件夹，名字为``SESSION.ser``
    - 活化，将存储文件重新加载到内存。Tomcat自动从``work``文件夹中读取文件``SESSION.ser``，读取完则删除文件
    - Tomcat默认自动实现了钝化和活化功能
    - ``Idea`` IDE下的Tomcat不能实现Tomcat上面的活化操作，它能实现钝化，但不能实现活化。因为Idea启动Tomcat时，首先会删除旧的work临时文件夹，然后创建一个新的work临时文件夹，用以启动新服务。这样work里面钝化文件就丢失。
3. Session什么时候被销毁
    - 服务器关闭
    - session对象调用``invalidate()``方法
    - session默认的失效时间，30分钟。可以在conf/web.xml下配置 Session-config元素中配置失效时间



### Session特点

1. Session用于存储一次会话的多次请求数据，存在服务器端，并通过向客户端的cookie中设置SessionId来实现其匹配机制
2. Session可以存储任意类型，任意大小的数据（由于实际存储的数据在服务器端，客户端只通过cookie存储一个SessionId
3. Session与Cookie区别/换脸
    - Session数据在服务器端，cookie在客户端
    - Session数据没有大小限制，cookie有
    - Session数据安全，cookie不安全





## 登录验证码案例

验证码一般是一个动态生成的小图片，包含一个特定的字符串，用来验证用户是否为bot

采用的方式为``Session``

流程：

1. 用户请求某个登录页面
2. 该请求被分发到  ``LoginServlet``处理，并返回给客户登录页面html。
3. 用户请求验证码图片，该请求验证码的request被分发到`` ImageServlet``中，生成图片，并返回。该过程中生成的验证码实际值，保存在服务器端``Session``中（内存），并将包含SessionId的cookie一并返回给浏览器（Session的原理）
4. 浏览器拼接两次请求为一个完整的登录页面。
5. 用户输入用户名、密码、验证码，并登录。浏览器发送form表单，以及cookie给服务器``checkInServlet``检查。
6. 服务器``checkInServlet``，首先从form表单中提取用户输入的验证码。并通过cookie中携带的SessionId来查找服务器内存中匹配的Session，从中找到验证码的值。两者进行比较，若匹配则继续提取用户名密码，进行登录校验。若不匹配，则返回登录失败错误码，返回给浏览器，让其对用户做出相应的错误提示。



