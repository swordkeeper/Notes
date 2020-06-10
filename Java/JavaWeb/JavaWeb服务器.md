# Web服务器

## WebLogic 

Oracle 公司的，支持大型JavaEE的收费软件Web服务器



## WebSphere

IBM公司的，支持大型JavaEE的收费软件Web服务器



## JBOSS

JBOSS公司，支持大型JavaEE的收费软件Web服务器



## Tomcat

Apache基金开发的免费开源JavaEE服务器，中型。只支持少量Java规范



- 项目的部署

    1. Tomcat 支持动态加载，更新

        将项目打包成``.war``包，并放入``webapp``目录下。只要服务器启动着，服务器就会自动更新，解压.war包。解压后的目   录名和包名相同。这中部署方式的虚拟路径即为目录名

    2. 在Tomcat配置文件中，设置。

        将Tomcat主目录，conf文件夹下的``server.xml``文件进行修改。在``Host``下添加，一串内容``Context``即可

        ```xml
        <Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
        	<Context docBase='/User/chenrui/testing/hello' path='hehe'/>
            <!-- docBase 为项目路径， path 为虚拟路径 -->
        </Host>
        ```

    3. 热部署（推荐）

        - 在 ``conf/Catalina/localhost``下，创建一个``xxx.xml``。该xml文件名，会最终被当成虚拟路径名。

        - 在文件中，写入

            ```xml
            <Context docBase=".....xxx 项目path" />. <!--即可-->
            ```

        - Tomcat 热启动，加载``xxx.xml``。xxx就被当成虚拟路径了。其中的文件，就是docBase中设置的文件

        该种方式部署的项目，项目不用添加到``webapp``目录下，且解析后也不会再该目录下生成对应的工程。这样就简化了部署。

- Servlet

    ``servlet``是Tomcat服务器端的小程序，可以用来实现业务。

    Servlet需要业务类，需要实现``Servlet``接口。实现该接口的类需要重写：

    ```java
    public class testServlet implements Servlet {
        @Override
        public void init(ServletConfig servletConfig) throws ServletException {
    		// 重写该方法，该方法是当本 Servlet加载到服务器内存时调用
        }
    
        @Override
        public ServletConfig getServletConfig() {
            // 该方法用于对Servlet进行配置
            return null;
        }
    
        @Override
        public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
    		// 该方法是真正的服务方法，即每次客户端访问相应的服务。该方法就会被调用
        }
    
        @Override
        public String getServletInfo() {
            // 重写该方法，用来打印Servlet一些相关信息
            return null;
        }
    
        @Override
        public void destroy() {
    		// 该Servlet被清理出内存时候调用。一般是服务器停止服务，或者强制撤销某个Servlet时执行。
        }
    }
    ```

- Servlet 解析

    Servlet 解析有两种方式，一种是``web.xml``配置，另一种是``WebServlet注解``

    1. ``web.xml``配置servlet

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
                 version="4.0">
            <! -- 从这里开始，在web-app中配置 Servlet 和 访问类的mapping关系-->
            <servlet>
                <servlet-name>demo1</servlet-name>  <!-- 定义一个servlet名字 -->
                <servlet-class>com.testTomcat.testServlet</servlet-class>  <!-- 定义对应的服务类-->
                <load-on-startup>1</load-on-startup> <!-- 可选配置。表示该服务类是否在服务器启动时加载。
        			当为负数时（默认），表示第一次访问该服务时加载。正数和0表示服务器启动时加载。 -->
            </servlet>
            <servlet-mapping>
                <servlet-name>demo1</servlet-name>
                <url-pattern>/Demo1</url-pattern>   <!-- 定义对应的访问url -->
            </servlet-mapping>
        
        </web-app>
        ```

        

    2. ``@WebServlet 注解``

        在定义的服务类（实现了Servlet接口）前面添加注解

        ```java
        
        @WebServlet("/Demo2")   // 该注解，可以接受一些参数。默认的参数value是url。也就是默认映射的url
        public class testServlet2 implements Servlet {
            @Override
            public void init(ServletConfig servletConfig) throws ServletException {
        
            }
        .......
        ```

        