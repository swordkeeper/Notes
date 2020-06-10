# Servlet

服务类必须实现``Servlet``接口，才能被Tomcat服务器调用

- ``Servlet接口``的子接口``GenericServlet接口``实现了空的其余方法。只留下``service``方法。用于简化编程
- ``Httpservlet``也是对``Servlet接口的实现``。该实现更好的封装了HTTP协议。它对HTTP协议的 GET 和 POST 方法进行了封装。具体使用时，重写某个方法就好。例如``doGet``或``doPost``方法。



### URL mapping

可以通过注释配置

```java
@WebServlet({"/hello", "/Hell*", "*.do"})  // *表示任意一个或多个通配符。但是不能写成 /*.do。只能写成 *.do
```







 