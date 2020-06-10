# JDBC

JDBC (Java Database Connectivity) 数据库连接器

JDBC 定义了一套可以统一操作不同数据库的规则（接口）。各大数据库厂商，mysql，oracle等各自实现该接口。

因而Java用户可以通过统一的接口操作不同的数据库。实现Java与数据库的透明衔接 



## JDBC 使用

### 简单使用方法

1. 导入``jar`` 包
2. 注册驱动
3. 获取数据库连接对象 ``Connection``
4. 定义``sql``语句
5. 获取执行sql语句的对象 ``statement`` 
6. 执行sql，接受返回的结果 
7. 处理结果
8. 释放资源

案例

```java
package com.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

/**
 * JDBC 快速入门 共8步
 * 1. 导入 jar 包
 * 2. 注册驱动
 * 3. 获取数据库连接对象 connection
 * 4. 定义sql语句
 * 5. 获取执行sql语句的对象 statement
 * 6. 执行sql，接受返回的结果
 * 7. 处理结果
 * 8. 释放资源
 */
public class JDBCDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException{
        // 1. 导入mysql jar包。新建一个文件夹libs，将 mysql-connector-java-5.1.37-bin.bar 复制进libs文件夹。
        //    并add as library
        // 2. 注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver"); // 注册Driver驱动类，进内存。java-8.0.jar的格式

        // 3. 获取数据库连接
        //    连接器jdbc:数据库类型://套接字/数据库名称
        //    用户名，密码
        Connection conn = DriverManager.getConnection(
                "jdbc:mysql://127.0.0.1:3306/hello",
                "root","greatness");

        // 4. 定义sql语句
        String sql = "update t1 set name='lisi' where id=33";

        // 5. 获取执行sql的对象
        Statement statement = conn.createStatement();

        // 6. 执行sql
        int count = statement.executeUpdate(sql);  // 执行Update操作，结果count表示影响的行数
        
        // 7.操作数据结果
        System.out.println(count);

        // 8. 释放资源
        statement.close();
        conn.close();
    }
}

```



### 对象

1. ``DriverManager``，驱动管理对象

    - 注册驱动

        1. ``static void registerDriver(Driver driver)``，通过DriverManager 注册驱动程序

        2. 等价于``Class.forName("com.mysq.jdbc.Driver")``

        3. mysql 5.0 以后的jar包可以自动注册驱动。即上面两种语句可以省略。原因在于mysql jar中的

            ``META-INF/services/java.sql.Driver``中包含了加载的静态配置属性

    - 获取数据库连接

        - DriverManager.getConnection()

2. ``Connection``，数据库连接对象

    三个参数：url：``"jdbc:mysql://127.0.0.1:3306/db1"``，username，Password

    方法：

    - createStatement() 获取数据库执行的方法
    - preparedStatement(string sql) 获取数据库执行方法

    事务：

    - setAutoCommit(boolean autoCommit) 设置自动提交事务 。若要手动提交，需要关闭自动提交
    - commit() 提交事务
    - rollback() 回滚

3. ``Statement``，执行sql对象。执行静态sql，并返回执行结果

     方法：

    - Boolean execute(String sql)     可以执行任意的sql

    - executeUpdateString sql)    

        ​	执行 ``DML``语句，包括``insert``,``update``,``delete``

        ​	执行``DDL``，``创建表``，``修改``、``删除表``

    - ResultSet executeQuery(sql) 

        ​	执行 ``DQL``返回查询结果集对象

4. ``ResultSet``，结果集对象
    方法：

    - ``next()``游标向下移动一个单位

    - ``getXxx(String | int 从1开始)``获取一行中的某一列。 getInt()获取整形数据， getString()获取字符串数据。

        例如：getDouble("Salary")，获取double类型的数据，通过列名Salary获得。或者通过索引获得getDouble(2);

        ```java
        String sql = "select * from t1";  //  sql语句
        Statement statement = conn.createStatement();  // 执行体
        ResultSet rs = statement.executeQuery(sql);    // 执行sql语句 获取结果集
        
        while(rs.next()){    // 跳过表头行
                int id = rs.getInt(1);    // 读第一列的id
                 String name = rs.getString("Name");  // 读姓名列
                System.out.println(id + name);
        }
        ```

        

5. ``PreparedStatement``，预执行sql对象

    ``PreparedStatement``的出现主要是防止``sql注入``。因为在正常的statement中。数据字段的根据字符串拼接来完成。

    如果传如的数据`` a'  or 'a' = 'a ``。``PreparedStatement``效率更高

    PreparedStatement方式会帮助解决该问题。他用预编译占位符``?``进行预编译。然后在传入参数填补``?``

    ```java
    String sql = "select * from user where username=? and password=?";
    PreparedStatement ps = connection.preparedStatement(sql);
    ps.setString(1, "lisi");  // 从1开始   还有setDouble, setInt等
    ps.setString(2, "12345678");
    ResultSet rs = ps.executeQuery();
    ```



### 事务

JDBC中的事务，通过``Connection``对象管理

- ``setAutoCommit(boolean)``
- ``rollback()``
- ``commit()``

```java
Connectino conn = null; 
try{
	conn = JDBCUtils.getConnection();
	conn.setAutoCommit(false);     // 关闭自动提交，也就是开启手动 transction
    // ....
    // sql 执行
    
    conn.commit()   // 所有语句正常执行完毕，才commit
}catch(Exception e){
    conn.rollback();      // 回滚   执行期间出现异常，则回滚
}
```



