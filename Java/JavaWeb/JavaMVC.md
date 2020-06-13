# MVC 架构

MVC模式将一个web程序分为3个部分

- M（model）模型
- V（view）视图
- C（controller）控制器





## MVC具体内容

### Controller

1. 获取用户数据
2. 调用model
3. 将结果转发给view
4. 可以用``servlet``技术实现



### Model

业务逻辑，与database交互。实现数据的CRUD

通常情况下使用``JavaBean`` 来打包辅助完成业务



### View

视图是用来展示数据的。将控制器传递过来的数据与模板结合生成最终页面

可以通过``jsp``技术实现 





## EL表达式

### 概念

### EL = ``Expression Language``

### 作用 

他可以替换和简化，JSP页面中Java代码的编写。

JSP默认就``支持EL``表达式的，但如果有页面配置，则忽略页面所有EL

```jsp
<%@ page isELIgnored="true" %> 忽略EL表达式
```

### 语法

``${ 表达式 }``

``\${ 表示式 }``，主动忽略转义表达式，这样表达式就按字符串原样输出

### 案例

- 运算

```jsp
${ 3 > 4 }  页面输出false
${ 3+5 > 33 || true }
${ empty list}  判断元素中的值是否为0， ""等
```

- 获取值

    EL表达式，只能从``域对象``中提取值

    ```jsp
    ${域名称.键名}  // 从指定域中，获取指定值
    ${requestScope.getAttribute("name")}
    ```

    域的范围

    |    域名称    |   所指域对象   |    作用范围    |
    | :----------: | :------------: | :------------: |
    |  pageScope   |  pageContext   |    当前页面    |
    | requestScope |    request     |     转发链     |
    | SessionScope |    session     | 会话的多次交互 |
    | application  | servletContext |      全局      |

    域名在EL中是可以忽略的。若忽略，EL表达式在寻找属性时，根据域的大小，从小到大查找里面是否含有某个属性

- 获取对象属性

    ```jsp
    <%
    	User user = new User();
    	user.setName("zhangsan");
    	user.setAge(15);
    	request.setAttribute("u",user);
    %>
    
    使用方式  ${域名.键名.属性名}
    ${requestScope.u.name}      requestScope拿到request对象 .u获取属性中key为u的对象 .name调用属性方法getName
                                所以一定要有javaBean打包，这样才能有getName方法
    							即使没有JavaBean打包。只要有getXxx()方法，依然能调用 u.Xxx
    ```

- 获取list和mapt对象

    ```jsp
    <%
    	List<String> list = new ArrayList<String>();
    	list.add("hello");
    	list.add("world");
    	request.setAttribute("list", list);
    
    	Map<String, String> map = new HashMap<String, String>();
    	map.put("name", "张三");
    	map.put("age","15");
    	request.setAttribute("map", map);
    %>
    
    list 使用方式 ${域名.键名[index]}
    ${requestScope.list[0]}   打印hello
    
    map 使用方式 ${域名.键名.key}
    ${map.name} 
    或者
    ${map["name"]}
    ```

    





