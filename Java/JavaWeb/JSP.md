# JSP

## 概念

JSP ， ``Java Server Pages`` ，java服务器端页面。通俗的理解，一个种格式的页面，既可以定义``html``页面，也可以书写``java代码``。

JSP可以简化书写代码。



## JSP原理

JSP原理可以通过以下流程来反映：

1. 客户端访问某个``.jsp``文件
2. 服务器收到请求，检索路径是否有某个``.jsp``文件
3. 若存在，服务器就会对``.jsp``进行编译，生成某个``.class``字节码文件。这些字节码文件一般会被放到``work``文件夹下（临时文件文件夹）
4. 服务器调用该字节码文件，向用户返回响应文件。
5. 浏览器解析响应文件（html文件）并解析为页面展示。
6. 生成的``.class``字节码文件，就是``servlet``。其实现了service抽象方法



## JSP脚本

1. ``<%    java代码    %>``，这种方式定义的代码，会出现在servlet 的 ``service``方法中。

2. ``<%!   java代码    %>``，这种方法定义的代码，出现在servlet类中，也就是``成员变量``。不推荐，因为servlet最好不要添加成员变量。以防多线程并发访问servlet是，出现数据读写异常

3. ``<%=   java代码    %>``，这种方法定义的代码，会被``输出到页面``

4. ``<%@ 指令名称 属性名1=属性值1 属性名2=属性值2 %>``，用于配置jsp页面，导入资源 

    三种指令：

    - page，配置jsp页面属性

        ```jsp
        <%@ page contentType="text/html;charset=utf-8" pageEnconding="utf-8" language="java" buffer="8kb" %>  
        <%@ import="java.util.xxx....." %>
        contentType 表示页面内容miniType（响应头）
        pageEnconding 表示本页面编码
        buffer out对象的缓冲区大小
        import 导java包
        errorPage 发生异常后会跳转的页面
        isErrorPage 标识当前页面是否为Errorpage
        ```

    - include，导入**页面**资源文件。包含其他jsp页面

        ```jsp
        <%@ include file="top.jsp" %>
        <% ...... %>
        <%@ include file="base.jsp" %>
        ```

    - taglib ，导入资源，导入标签库。这样在jsp中就可以使用非html、非jsp的标签

        ```jsp
        <%@ taglib prefix="mytag" uri="http://java.sun.com/jsp/jstl/core" %>
        使用标签
        <mytag:xx......></mytag:xx......>
        ```

5. ``<%--  --%>``注释

## JSP 内置对象

JSP 有9个内置对象

1. ``Request`` 和``response``，由于jsp编译后就是servlet，所以这两个对象默认存在，可以直接调用。
2. ``out``对象，内置的jsp，类似于``response.getWriter()``对象。可以直接使用
    - ``response.getWriter()``与``out.write()``区别：tomcat服务器在给客户端响应之前，会先找response的输出缓冲区，再找out缓冲区数据。因而会使得response输出的内容，永远先于out输处。  所以建议，永远用out对象，不要用response.getWriter对象
3. session，HttpSession对象
4. application，实际上就是servletContext对象
5. page，当前的servlet对象
6. pageContext，实际是pageContext对象。``通过该对象可以获取其他所有的8个对象``
7. config  ServletConfig对象 
8. exception，声明了isErrorPage="true"的jsp页面，才能使用该对象

<img src="../image/jsp内置对象.png" />



