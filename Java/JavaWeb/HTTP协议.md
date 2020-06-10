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
    >   User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36				   # User-Agent 用户浏览器信息。等等
    >  Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
    > Accept-Encoding: gzip, deflate, br
    > Accept-Language: en-GB,en-US;q=0.9,en;q=0.8,zh-CN;q=0.7,zh;q=0.6
    > Cache-Control: max-age=0
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



### Request 实现类

Tomcat 对Request 定义了一个接口

继承关系

> ServletRequest.        // 原始request接口
>
> HttpServletRequest    // 子接口
>
> org.apache.catalina.connector.RequestFacade   // 实现类