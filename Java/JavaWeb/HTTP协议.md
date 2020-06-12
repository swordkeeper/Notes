### HTTP协议

## 格式

### Request

1. 请求行

    > GET /Login.html HTTP/1.1       # 方法  请求资源URL  协议/版本

    Http有7中请求方式:

    - ``Get``，请求参数在请求行中。也就是上面HTTP/1.1之前。也就是URL中。且请求长度有限制的。明文，不太安全

    - ``Post``，请求内容出现在请求体中。请求长度没有限制。在请求体里，相对安全

        

2. 请求头

    > Host: localhost               		 # 键值对形式的数据，Host 主机地址
    >
    >  User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36				   # User-Agent 用户浏览器信息。等等
    >
    > Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
    >
    > Accept-Encoding: gzip, deflate, br
    >
    > Accept-Language: en-GB,en-US;q=0.9,en;q=0.8,zh-CN;q=0.7,zh;q=0.6
    >
    > Cache-Control: max-age=0
    >
    > Connection: keep-alive

    常用请求头：

    ``Host``：主机地址，或域名

    ``User-Agent``：浏览器信息

    ``Accept``：可以解析文件的格式

    ``Accept-Language``: 可以解析的语言

    ``Accept-Encoding``：支持的压缩格式 

    ``Referer``：请求从哪里来。特别重要。1. 可以用来 ``防盗链``，防止其他网址盗用本网站资源  2. 可以用来``统计连接``

    ``Connection``：表示连接是否可以被服用。一般http1.1都是keep-alive

    ``Upgrade``：是否可以被升级

3. 请求空行

    >  
    >
    > #就是一个空行，用来分割请求头和请求体。Get方式没有请求体，POST方式有请求体

4. 请求体

    > usernae=zhangsan
    >
    > age=15

## Request 继承关系



Tomcat 对Request 定义了一个接口

继承关系

> ServletRequest.        // 原始request接口
>
> HttpServletRequest    // 子接口
>
> org.apache.catalina.connector.RequestFacade   // 实现类



## Request Tomcat实现

### 方法

#### 获取请求

1. ``String getMethod()``：获取请求方式，GET 或 POST

2. ``String getContextPath()``：获取虚拟路径，也就是工程的虚拟路径。例如 /project1

3. ``String getServletPath()``：获取项目的本服务Servlet路径。例如/module1

4. ``String getQueryString()``：获取url中的请求参数，也就是?之后的数据

5. ``String getURI()``：获取访问路径也就是 URI = contextPath +. servletPath. 例如   /project1/module1

6. ``String getURL()``：获取完整的URL连接。等于  协议+Socket+URI .例如："http://localhost/project1/module1"

7. ``String getProtocol()``：获取协议和版本 例如  HTTP1.1

8. ``String getRemoteAddr()``：获取远程请求的IP地址

    

#### 获取请求头

1. ``String getHeader(String )``：获取某个请求头

2. ``String getHeaders()``：获取请求头完整数据

3. ``Enumeration<String> getHeaderNames()``：获取请求头名称。即罗列所有请求头的Key

    

#### 获取请求体

Servlet将请求体封装成``流``，所以要获取请求体，就得通过获取流的方式来获取。并且，只有``POST``方式才有请求体

1. ``BufferedReader getReader()``，获取字符流
2. ``ServletInputStream getInputStream()``，获取字节流

通过访问这些流对象，来获取请求体中所携带的数据

第二种方式，如果通过POST提交参数。可以通过：

1. ``getParameter()``等方法获取

2. ``getParameterNames``获取所有参数名

**两种方式不能共同使用。它类似于读取文件，用了一种方式，游标就会向前推进。造成第二次读取时（换种方式读取）读不到数据**



#### 乱码问题

最常见的问题，就是读取中文时会出现乱码。由于Post传递的请求体是``流式``数据。流式数据就涉及到编码和解码问题。

 解决办法，在request中设置编码。这样当提取request中的数据前，就可以对其编码进行设置。这样获取的数据，就会按照编码格式输出

```java
request.setCharacterEncoding("utf-8"); 
response.setCharacterEncoding("utf-8");

// 如果不清楚浏览器默认用什么格式解码，可以添加响应头字段。 Content-Type，该字段推荐浏览器用某种格式解码
response.setHeader("content-type","text/html;charset=utf-8");
// 或者简写
response.setContentType("text/html;charset=utf-8");
```



#### 请求转发

由于有的请求服务很复杂。涉及到多个业务逻辑，所以不可能在一个servlet中单独完成，这就需要对该请求进行转发

转发有两个步骤：

1.  获取``dispatcher``对象

    ```java
    // 获取一个派发对象，并传递派发 路径
    RequestDispatcher dispatcher = req.getRequestDispatcher("newPath");      
    ```

2. 通过``forward``方法进行转发

    ```java
    // 转发请求，需要传递request，response对象
    dispatcher.forword(req, resp);
    ```

**转发的特点**：

1. 转发时，虽然访问了多个servlet。但是地址栏路径没有发生变化，还是最开始的那个URL
2. 转发是发生在服务器内部，跟浏览器之间没有多余的交互。仍是请求一次，返回一次（如果只返回一个资源）
3. 转发是发生在服务器内部，因而转发地址不能请求项目外部地址。例如百度等



#### 共享数据 -- 域对象

域对象是指由作用范围的对象，可以在一个范围内共享数据。由于request可以在不同servlet中forward，在一个servlet中产生的计算数据，可以被携带到request中，并传递给下一个servlet。

``request域``：一次请求的范围。一般用于请求和转发的多个资源中共享数据。具体使用方法：

1. ``setAttribute(String Object)``，将Object数据存到String指定的key中
2. ``Object getAttribute(String)``，获取某个数据
3. ``removeAttribute(String)``，移除某个数据 



**综上，GetParameter 是获取有关request参数列表，包括请求头和请求体中的数据。Get/SetAttribute是request在redirect时候添加到request对象中的中间生产的变量**

#### servletContext

ServlentContext表示数据servlet的运行环境变量





## Response

1. ``响应行``

    > HTTP/2.0 200 OK

    协议/版本   响应状态码   状态码描述

    响应码分类：

    1. ``1xx``，表示客户端给服务器发消息，但是客户端没有发完。服务器等待客户端继续发送完整数据，但是等待一段时间后依然没有收到完整数据。所以服务器发送1xx的响应码，询问客户端
    2. ``2xx``，成功。一般为``200``，表示成功。
    3. ``3xx``，重定向。重定向是指服务器通知浏览器，让浏览器重新访问另外一个资源来实现某种功能。这区别与服务器内部的``dispatch forward``操作。
        - ``302``，表示重定向某个连接。
        - ``304``，表示某个数据之前已经缓存到浏览器端，而服务器对该数据没有更改。因而服务器为了减少网络流量，通知客户端（等于重定向到浏览器端）304，表示该资源已经存在于浏览器端缓存。描述为 Not Modified
    4. ``4xx``，客户端错误
        - ``404``没有对应的资源
        - ``405``请求方式没有的对应方法。例如``POST``请求，而服务器端没有接受``POST``请求的方法，只有``GET``方法
    5. ``5xx`` ，服务器端错误。
        - ``500``，服务器内部异常
        - 

2. ``响应头``

    ​	键值对数据，常见的响应头

    - ``Content-Type``，告诉浏览器，响应体的数据格式和编码格式。text/html;charset=utf-8
    - ``Content-disposition``，高速浏览器以什么格式打开响应体体的数据
        - In-line 默认，当前页面打开
        - Attachment; filename=xxx，以文件附件方式打开（文件下载）

3. ``响应空行``

4. ``响应体``

    ​	HTML 返回页面



## Response Tomcat 实现

### 方法

#### 响应行

- ``setStatus(int)``，设置响应码

#### 响应头

- ``setHeader(String key, String value)``，设置响应头的键值对

#### 响应体

1. 获取输出流

    - 字节流

        ```java
        PrinterWriter getWriter();
        ```

    - 字符流

        ```java
        ServletOutputStream getOutputStream();
        ```

        

2. 通过输出流写入到响应体中，并最终被response携带到浏览器 



### 重定向

```java
response.setStatus("302");  // 设置响应码302，表示重定向
response.setHeader("location", "/newDemo"); // 设置重定向的目标uri，关键字为location

//----------- 另一种简单的重定向方式---------
response.sendRedirect("/newDemo");   // 只用填写重定向的uri
```

**重定向与转发的区别：**

重定向：

- 转发涉及到多次请求
- 转发地址栏改变
- 转发能访问服务器外部url
- **request域**，在重定向过程中失效。即request不能通过设置setAttribute来共享数据，来传递中间值。毕竟重定向是两次request

转发（与重定向完全相反）：

- 转发只涉及一次请求
- 转发不改变地址栏
- 转发只能在服务器内部转换request（项目）



#### 绝对路径

在服务器书写地址时，要分清如何写绝对路径

- 绝对路径需要包含``虚拟目录``的情况：
    **该地址是给浏览器用户使用的，包括<a>标签，重定向地址等**。这些是给浏览及让它重新发起新 request请求的，而这些请求（如上面的区别所说）可以是外部服务器url的。所以应该包含外层的``虚拟目录``地址，来区别项目。

    虚拟路径最好动态获取，防止项目更改了虚拟路径，造成大面积更改需求**request.getContextPath()**

- 绝对路径不包含``虚拟目录``的情况：

    **地址是给服务器内部使用的**。由于是在服务器内部使用，一般是指``转发``，不涉及到外部url，因而也不需要外层的虚拟目录