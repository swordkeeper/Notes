# Java 过滤器

## 过滤器 Filter

当Web访问请求来的时候，过滤器可以将请求拦截下来，完成一些特殊功能

### 作用

- 一般用来完成通用的操作，比如：``登录``、``验证``、``统一编码处理``、``敏感字符过滤``



### 使用步骤

1. 定义一个类，实现接口``Filter``

2. 复写方法

3. 配置``拦截路径``

    1. 第一种配置方式，``web.xml``

        ```xml
        <web-app>
        	<filter>
                <filter-name>filter1</filter-name>
                <filter-class>com.test.web.filter.FilterDemo1</filter-class>
            </filter>
            <filter-mapping>
                <filter-name>filter1</filter-name>
                <url-pattern>/*</url-pattern>   <!-- 拦截路径为/*  会拦截所有的请求 然后去过滤这些请求 -->
                <dispatcher>REQUEST</dispatcher> <!-- 设置拦截类型 -->
            </filter-mapping>
        </web-app>
        ```

        

    2. 第二种配置方式

        ```java
        @WebFilter(urlPatterns="/*")  // 默认需要传递的参数为 urlPatterns。 
        						//  /* 代表过滤掉所有，即所有的请求都需要经过滤器过滤。
        						//  /demo.jsp 这个配置，表示只要有请求访问demo.jsp 就需要经过该过滤器
        public class FilterDemo implements Filter{
            @public void doFilter(ServletRequest servletRequest, 
                                  ServletResponse servletResponse, 
                                  FilterChain filterChain){
                
            }
        }
        ```

4. 编写放行条件

    参数``filterChain``对象的``doFilter``方法表示放行

    ```java
    @WebFilter(urlPatterns="/*")
    public class FilterDemo implements Filter{
        @public void doFilter(ServletRequest servletRequest, 
                              ServletResponse servletResponse, 
                              FilterChain filterChain){
            
            // 放行条件
            if(xxxxx){
                filterChain.doFilter(servletRequest, servletResponse);  // 满足条件放行请求和响应
            }
        }
    }
    ```



### 执行流程

1. 浏览器发送某个请求
2. 由于请求的uri复合拦截pattern，所以被filter拦截
3. filter执行``chain.doFilter``之前的代码，对request请求进行筛选过滤
4. ``filter``执行chain方法，对请求进行放行
5. 请求通过，接受某种服务
6. 请求返回又被``filter``拦截，这时请求就是一个返回值。直接执行``chain.doFilter``之后的代码
7. 在此处可以对``response``对象进行增强操作。完善请求返回数据。



### 生命周期

1. 服务器创建，调用filter的``init``方法，一般用于加载资源
2. 每次请求来时，执行``doFilter``方法，可以执行多次
3. 服务器正常停止，调用filter对象的``destroy``方法，用于释放资源



### 配置详解

配置拦截uri

1. 拦截配置  ``/index.jsp``，访问 index.jsp 资源，才会被拦截

2. 拦截目录 ``/user/*``，访问user目录下的所有资源时，才会被拦截

3. 后缀名拦截 ``*.jsp``，拦截所有jsp资源。注意此处不加``/``

4. 拦截所有资源 ``/*``

    

配置拦截请求方式

1. 注解设置

    ```java
    // 只拦截 request 和 转发请求
    @WebFilter(value="/*", dispatcherTypes={DispatcherType.REQUEST, DispatcherType.FORWORD})  
    public class FilterDemo implements Filter{.....
    ```

    有五种请求分发方式：

    - REQUEST：默认值，浏览器的直接请求
    - FORWORD：通过转发访问的资源
    - INCLUDE：包含访问，例如jsp里面的include包含框
    - ERROR：错误跳转资源
    - ASYNC：异步访问



### 过滤链

如果有两个过滤器filter1 和 filter2。栈式链条 

- 执行顺序：

    1. 执行 filter1 
     2. 执行 filter2
     3. 执行资源
     4. 返回 filter2
     5. 返回 filter1 

- 过滤器排序顺序：

    - 注解配置 filters：按照类名的``字符串顺序``排序，排序在前面的先执行 
    - web.xml配置：那个的``filter-mapping``配置在前，哪个先执行

    



### 过滤器的增强

有的时候过滤器的作用是需要对发来的请求进行过滤，例如：``用户是否已登录``，若有登录则放行、没有则让其跳转到登录界面。这种过滤器实现的是拦截操作。

还有的过滤器，可以对数据进行加工操作，使得请求所传递的参数都正确、合法。 例如要向后台写入某段文字，过滤器就可以过滤掉这些文字中的敏感词。这就是一种``数据增强``。对数据进行排查修改、整理

通常使用的过滤器增强技术：

- 设计模式实现``数据增强``：

    1. 装饰模式：动态的给对象增加一些装饰器，额外的功能 

    2. 代理模式：

        - 概念：
            1. 真实对象，也就是真正要使用的对象
            2. 代理对象，通过代理对象，间接地访问真实对象
            3. 代理模式，通过访问代理对象，来增强真实对象的功能（代理对象可以执行额外的操作） 

        - 代理方式：

            1. 静态代理，有一个静态类文件描述代理关系
            2. 动态代理：在内存中，形成代理类

        - 实现步骤（动态）：

            1. 代理对象和真实对象实现相同的接口
            2. 代理对象 = Proxy.newProxyInstance(classLoader);
            3. 使用代理对象调用相同方法（相同接口）
            4. 增强了方法
                - 增强参数
                - 增强函数体
                - 增强返回值

            ```java
            interface realObjInterface{   // 定义公共的方法         
                public void sale(String str);
                public void show();
            }
            class RealObj implements realObjInterface{  // 真实对象实现接口
                @Override
                public void sale(String str){
                    System.out.println("真实对象卖出货物"+str);
                }
                @Override
                public void show(){
                    System.out.println("展示东西");
                }
            }
            
            public class ProxyTest(){   // 代理演示
                public void static main(String[] args){
                    // 1. 定义真实对象
                    RealObj real = new RealObj();
                    // 2. 定义代理
                  realObjInterface proxy_obj = 
                      （realObjInterface）Proxy.newProxyInstance(   // Proxy类的静态方法获取代理
                    	real.getClass().getClassLoader(),  // 第一个参数，真实类对象
                         real.getClass().getInterfaces(),   // 第二个参数，传递真实对象所实现的接口
                         new InvocationHandler(){       // 第三个参数，一个代理处理方法对象，实现invoke方法
                             public Object invoke(Object proxy, Method method, Object[] args){
                                 // invoke 方法第一个参数就指的当前代理对象
                                 // 第二个参数，表示访问对象所使用真实对象的方法，是一个反射器的方法类
                                 // 第三个参数，表示调用时所传递的参数
                                 // 任何通过代理对象访问方法时，都会调用invoke方法
                             }
                         }
                    )
                }
            }
            ```

            

