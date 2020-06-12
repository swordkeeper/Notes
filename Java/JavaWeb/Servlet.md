# Servlet

服务类必须实现``Servlet``接口，才能被Tomcat服务器调用

- ``Servlet接口``的子接口``GenericServlet接口``实现了空的其余方法。只留下``service``方法。用于简化编程
- ``Httpservlet``也是对``Servlet接口的实现``。该实现更好的封装了HTTP协议。它对HTTP协议的 GET 和 POST 方法进行了封装。具体使用时，重写某个方法就好。例如``doGet``或``doPost``方法。



### URL mapping

可以通过注释配置

```java
@WebServlet({"/hello", "/Hell*", "*.do"})  // *表示任意一个或多个通配符。但是不能写成 /*.do。只能写成 *.do
```





### ServletContext 对象

1. 概念：代表整个web对象，可以和程序的容器（服务器）进行通讯

2. 它是一个接口。他可以让一个servlet跟运行它的（tomcat)容器进行沟通

3. 获取：

    - ```java
        // 第一种方式获取 servletContext对象。通过request获取
        request.getServletContext();
        // 第二种方式，通过HttpServlet获取
        this.getServletContext()
        ```

        

4. 功能：

    - 获取``MIME``类型

        - MIME类型：在互联网通讯过程中定义的一种文件数据类型

        - MIMIE 格式： 大类型/小类型，  例如 ``text/html`` ， `` image/jpeg``，``text/htm``。这就是三个不同的MIME类型。它标识一个文件是什么类型，MIME类型提供了该信息。

        - ``String getMimeType(String file)``

        - ServletContext，根据后缀名获取MimeType的原理是，查询服务器配置web.xml文件中的配置表，从而找到对应后缀的MIMETYPE

        - ```java
            ServletContext context = this.getServletContext();  //  获取ServletContext对象
            
            String fileName = "a.jpg";   // 文件名
            String mimeType = context.getMimeType(fileName);   // 获取该文件的mimetype
            System.out.println(mimeType); 
            // 打印    image/jpg
            ```

            

    - 该对象是一个``域对象``，可以用来共享数据

        - 该对象的``域``是最大的，可以包括所有的request。可以使得数据在所有的request中共享
        - 通过一个request的Servlet添加，不经过转发，就可以让另一个request的servlet访问

    - 获取文件真实（服务器）路径

        - 方法：``String getRealPath()``，获取服务端真实项目路径，而不是开发环境路径

        - 返回的路径以``发布项目的路径``为根目录

        - 这种获取方式与``classLoader``方式获取的路径区别：

            > classLoader是获取src目录下的，是以src（开发环境）或WEB-INF/classes（部署）为根目录
            >
            > getRealPath()是以web（开发环境）目录为根目录的，在项目根目录（部署 环境）

  

### 文件名乱码问题

乱码会发生在文件内容解析上，也就是页面或者文件内容的编码/解码上。

但是``文件名``也可能出现乱码的。乱码的出现会让服务器找不到资源，或者保存文件到客户端时出现文件名乱码问题 。

解决办法：

- 根据不同浏览器，对文件名进行调整

    1. 获取user-agent请求头，获取浏览器和版本信息

        ``String agent = request.getHeader("user-agent");``

    2.  通过网上各种，文件名工具类，对文件名进行重写。使得其适应不同浏览器

        ``filename = DownLoadUtils.getFileName(agent, filename); `` 重写文件名

    3. 重新设置文件名（响应头），传递正确文件名

        ``response.setHeader("content-disposition", "attachment; filename="+ filename)``

- 网上的重写文件名工具类

    ```java
    import sun.misc.BASE64Encoder;
    import java.io.UnsupportedEncodingException;
    import java.net.URLEncoder;
    
    
    public class DownLoadUtils {
    
        public static String getFileName(String agent, String filename) throws UnsupportedEncodingException {
            if (agent.contains("MSIE")) {
                // IE浏览器
                filename = URLEncoder.encode(filename, "utf-8");
                filename = filename.replace("+", " ");
            } else if (agent.contains("Firefox")) {
                // 火狐浏览器
                BASE64Encoder base64Encoder = new BASE64Encoder();
                filename = "=?utf-8?B?" + base64Encoder.encode(filename.getBytes("utf-8")) + "?=";
            } else {
                // 其它浏览器
                filename = URLEncoder.encode(filename, "utf-8");
            }
            return filename;
        }
    }
    
    ```

    