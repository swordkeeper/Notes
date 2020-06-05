# Mysql 全

## DDL (Data Definition Language)

数据定义语言是用来操作``数据库``、``表``的。



1. 操作数据库

包括``CRUD``

- ``C``：Create 创建数据库

    ```mysql
    CREATE DATABASE IF NOT EXISTS db1 CHARACTER SET utf8; # 若不存在则创建字符集为utf8的数据库 db1
    ```

- ``R``：Retrieve查询数据库

    ```mysql
    SHOW DATABASES; 
    SHOW CREATE DATABASE db1;     # 查询创建db1的语句
    ```

- ``U``：Update 更新数据库

    ```mysql
    ALTER DATABASE db1 CHARACTER SET GBK;  # 更改数据库字符集
    ```

- ``D``：Delete 删除数据库

    ```mysql
    DROP DATABASE IF EXISTS db1;
    ```

- 使用数据库

    ```mysql
    SELECT DATABASE();   # 查询正在使用的数据库
    USE db1;    # 切换使用的数据库到 db1 
    ```



2. 操作数据表 

``CRUD``

- ``C``：创建表

    ```mysql
    CREATE TABLE t1(
    	ID   INT,
        NAME VARCHAR(20),
        AGE  INT,
        SCORE DOUBLE(5, 2),    # 一共5位，2位小数位
        BIRTHDAY DATE,
        CHANGE_TIME TIMESTAMP  # TIMESTAMP类型就是DATETIME类型，只是能够自动赋值为系统时间
    );
    CREATE TABLE t2 LIKE t1;   # 通过t1，创建一个复制表t2
    ```

    <img src="./images/Screen Shot 2020-06-04 at 1.57.30 am.png">

- ``R``：查询表

    ```mysql
    SHOW TABLES;   # 查询所有表
    DESC 表名;      # 查询表结构
    SHOW CREATE TABLE t1;  # 展现创建表结构
    ```

- ``U``：更新表结构

    ```mysql
    ALTER TABLE t1 RENAME TO t1_new;   # 更新表名
    ALTER TABLE t1 CHARACTER SET GBK;  # 更改字符集
    ALTER TABLE t1 ADD MYFIELD VARCHAR(20);  # 添加列名和类型
    ALTER TABLE t1 CHANGE MYFIELD ADDRESS VARCHAR(30);  # 修改名为 MYFIELD的列 为 ADDRESD VARCHAR(30)
    ALTER TABLE t1 MODIFY ADDRESS VARCHAR(40);  # 只修改列类型
    ALTER TABLE t1 DROP ADDRESS; # 删除列
    ```

- ``D``：删除表

    ```mysql
    DROP TABLE IF EXISTS t1;
    ```

    

## DML(Data Manipulate Language)

数据操作语言

``CRUD``

- ``C``增加

    ```mysql
    INSERT INTO 表名(列名1，列名2...) VALUES(值1，值2...);
    ```

- ``R``查询

    即为DQL语言

- ``U``修改

    ```mysql
    UPDATE 表名 SET 列名1=值1，列名2=值2 ...  WHERE ... # 修改表中数据
    UPDATE 表名 SET 列名1=值1，列名2=值2 ...   # 修改表中所有数据
    ```

- ``D``删除

    ```mysql
    DELETE FROM 表名 WHERE ...    # 删除某一个条件的记录
    DELETE FROM 表名  # 删除表。但是不推荐这种方法，这种删除方法会逐条删除元素，效率没有下面的 TRUNCATE 高
    TRUNCATE 表名     # 删除表中所有内容，并创建一个新的空的同名表
    ```

    

## DQL(Data Query Language)

数据查询语言

``R``：

1. 查询基本

    ```mysql
    # 查询某表某些数据，并分组，分组条件，并分组排序，并限制分页
    SELECT 列名1，列名2... FROM 表名 WHERE ...  GROUP BY .... HAVING... ORDER BY .. LIMIT
    
    SELECT DISTINCT 列名1， 列名2 FROM 表名    # 查询列1 列2 。并对结果 去重复
    SELECT * FROM 表名   # 查询所有字段，不推荐。公司希望将所有列名，清晰的输出，所以还是一列一列的表示
    
    SELECT NAME，MATH, ENGLISH, (MATH + ENGLISH) FROM...  # 查询并 计算总成绩
    SELECT NAME, MATH, ENGLISH, MATH + IFNULL(ENGLISH, 0) FROM... # 查询并计算总成绩，如果英语成绩为NULL，IFNULL，将其设置为0
    SELECT NAME, MATH, ENGLISH, MATH + IFNULL(ENGLISH, 0) AS TOTAL FROM...   # AS 语句为某一列设置别名，AS可以省略
    ```

2. 条件查询

    ```mysql
    SELECT * FROM t1 WHERE AGE >= 20;   # 年龄大于等于20岁
    SELECT * FROM t1 WHERE AGE = 20;    # 年龄等于20岁的
    SELECT * FROM t1 WHERE AGE != 20;   # 年龄不等于20岁的
    SELECT * FROM t1 WHERE AGE > 20 AND AGE <30;  # 年龄介于20到30岁之间的
    SELECT * FROM t1 WHERE AGE BETWEEN 20 AND 30; # 年龄介于20到30岁之间的（包含20，30）
    SELECT * FROM t1 WHERE AGE = 20 || AGE = 19;  # 年龄为20或19岁的
    SELECT * FROM t1 WHERE AGE = 20 OR AGE = 19； # 年龄为20或19岁的
    SELECT * FROM t1 WHERE AGE IN (20, 19);       # 年龄为20或19岁的
    SELECT * FROM t1 WHERE ENGLISH IS NULL;       # 查询英语成绩为NULL的，NULL 不能通过 ENGLISH=NULL 来获得
    ```

3. 模糊查询 ``LIKE``

    - ``_``表示单个任意字符
    - ``%``表示任意多个字符

    ```mysql
    SELECT * FROM t1 WHERE NAME LIKE '李%';  # 查询行李的人
    SELECT * FROM t1 WHERE NAME LIKE '_建国'; # 查询名为建国的所有人
    ```

4. 排序查询 ``ORDER BY``

    ```mysql
    SELECT * FROM t1 ORDER BY MATH DESC, ENGLISH ASC;   # 排序，现根据数学成绩 降序排序，然后在根据英语升序
    # 默认为ASC 升序
    ```

5. 聚合函数

    把列作为一个整体，进行纵向计算 。

    - ``count``，计算个数
    - ``max``，计算最大值
    - ``min``，计算最小值
    - ``sum``，求和
    - ``avg``，计算平均值
    - **所有的聚合函数都会排除null的数据**

    ```mysql
    SELECT COUNT(ID) FROM t1  # 统计中数据总数
    SELECT AVG(MATH) FROM t1; # 统计数学平均成绩
    
    SELECT AVG(IFNULL(ENGLISH,0）） FROM t1;   # 统计英语平均成绩，如果有数据的英语成绩为null，返回0并统计。
    ```

6. 分组查询

    ``GROUP BY``

    分组查询后，只能使用 ``聚合函数``或者``分组数据``，使用数据列没有任何意义

    ```mysql
    SELECT AVG(MATH) FROM t1 GROUP BY GENDER;  # 根据性别来查询统计数学平均成绩
    
    SELECT AVG(MATH) FROM t1 WHERE AGE > 15 GROUP BY GENDER; # 首先筛选年龄>15 ，在进行分组统计
    
    # 另一种筛选条件 HAVING
    # 查询结果进行聚合函数，如果 COUNT(ID) > 2 则筛选 
    SELECT AVG(MATH) FROM t1 GROUP BY GENDER HAVING COUNT(ID) > 2;
    
    ```

    ``where``和``having``区别：

    - Where 在分组之前进行限定，如果不满足条件，不参与分组
    - Where 之后不可以使用聚合函数
    - having在分组之后进行限定，如果不满足条件，则不会被查询出来
    - Having 之后可以使用聚合函数

7. 分页查询

    ``LIMIT``

    ```mysql
    SELECT * FROM t1 LIMIT 0, 3;   # 查3条记录，从索引值为0的数据开始查
    # 页码索引公式 index = (当前页面-1) * 每页显示条数
    ```



## 约束

约束是对表中的数据进行限定，以保证数据的有效性和完整性

- ``primary key``主键约束

    ```mysql
    # 删除主键
    alter table t1 DROP primary key
    create table t3{
    	uid int,
    	sid int
    	primary key(uid, sid)   # 联合主键，两个字段共同作为主键 
    }
    ```

- ``not null``非空约束

- ``unique``唯一约束 

    ```mysql
    # 删除唯一约束
    alter table t1 DROP INDEX 列名;
    ```

- ``foreign key``外键约束

    ```mysql
    CREATE TABLE t2(
    	...
        外键 constraint 外键约束名 foreign key 外键列名 references 主表名称(主表列名)
        
        # 定义一个外键列，然后给这个外键列添加约束 
        course_id INT, 
        # 例如 学生表和选课表。外键名一般命名为 t1_t2。关联课程表的id字段 COURSE(ID )
        CONSTRAINT  stu_course foreign key course_id references COURSE(ID)
    )；
    
    # 删除外键
    ALTER TABLE t2 DROP FOREIGN KEY stu_course; 
    # 添加外键
    ALTER TABLE f2 ADD CONSTRAINT  stu_course foreign key course_id references COURSE(ID);
    ```

    

- ``auto_increment`` 自动增长

    ```mysql
    # 一般是用来配合主键 int 类型 自动增长
    ALTER TABLE t1 MODIFY ID INT PRIMARY KEY AUTO_INCREMENT;  # 添加主键, 自动增长
    ```

    

- ``级联操作``，级联操作表示相关联的表的操作会映射到所关联的表。包括删除数据，更新数据

    ```mysql
    # 添加级联  CASCADE
    ALTER TABLE f2 ADD  
    CONSTRAINT  stu_course foreign key course_id references COURSE(ID) ON UPDATE CASCADE; # 级联更新
    
    ALTER TABLE f2 ADD  
    CONSTRAINT  stu_course foreign key course_id references COURSE(ID) ON DELETE CASCADE; # 级联删除
    ```

    

## 三范式

1. 1NF

    第一范式：``列不能再分``

2. 2NF

    第二范式：``消除部分依赖``。主属性可能有多列（联合主键等）。如果非主属性某列只依赖部分主属性，则就违背了第二范式

3. 3NF

    第三范式：``消除传递依赖``。某一列依赖于另一列，而另一列依赖于主属性。即传递依赖。3NF消除了传递依赖



## Mysql备份与恢复

保存导出

```bash
mysqldump -uroot -proot 数据库名 > 保存路径
```

还原导入

```bash
# 1. 登录数据库
# 2. 创建数据库
# 3. 使用数据库
# 4. source 文件
source 文件
```



## 多表查询

1. 内联查询

    - 隐式：通过 ``where``语句查询

        ```mysql
        select * from t1, t2 where t1.'foreign_key' = t2.id; # 通过 where 进行关联的查询
        
        
        # 公司标准格式
        SELECT
        	t1.NAME 姓名,
        	t1.AGE 年龄，
        	t2.NAME 部门
        FROM
        	Employee t1,
        	Department t2
        WHERE
        	t1.dept_id = t2.id;
        ```

    - 显示：通过``inner``语句查询

        ```mysql
        # SELECT 字段列表 FROM 表名1 inner join 表名2 on 条件
        
        SELECT 
        	* 
        FROM 
        	Employee 
        INNER JOIN    # INNER 关键字可以省略
        	Department 
        ON
        	Employee.dept_id = Department.id;
        ```

2. 外连接

      哪边连接，哪边表的数据是全的。包括空的数据

    - 左外连接

        ```mysql
        SELECT 
        	*
        FROM
        	EMPLOYEE
        LEFT OUTER JOIN   # OUTER 可以省略
        	DEPARTMENT
        ON
        	EMPLOYEE.dept_id = DEPARTMENT.id;
        ```

        

    - 右外连接

3. 子查询

    ```mysql
    SELECT * FROM EMPLOYEE    #  查询工资小于平均工资的人
    WHERE  EMPLOYEE.salary = (SELECT AVG(salary) FROM EMPLOYEE);
    
    ```



## 事务

1. 提交事务

    ```mysql
    start transaction;
    ```

2. 回滚

    ```mysql
    rollback;
    ```

3. 提交事务

    ```mysql
    commit
    ```

4. 查看事务提交方式

    ```mysql
    select @@autocommit    # 返回值为1 代表自动提交， 返回值是0 代表手动提交
    set @@autocommit=0;    # 修改自动提交
    ```

5. 事务特性

    - ``原子性``，最小单位，不可分割，要么全部成功，要么全不失败
    - ``持久性``，如果事务一旦 提交，或者回滚。数据库的表就会被永久的被更改
    - ``隔离性``，多个事务之间相互操作时，不影响对方。事务之间相互独立。
    - ``一致性``，表示事务操作前后，数据总量不变

6. 事务的隔离性遇到的问题

    多个事务之间，同时处理一个数据，可能出现：

    - ``脏读``：一个事务读取到了另一个事务中已经修改了但没有被提交的数据。
    - ``不可重复读``：一个事务，重复读一个数据，但是读到了两个不一样的值
    - ``幻读``：一个事务操作一个表的所有数据，另一个事务添加了一条数据。第一个事务，感受不到、早做不到第二个事务的修改

7. 事务的隔离级别：

    - ``读未提交 read uncommited``：引发脏读，不可重复度，幻读
    - ``读已提交 read committed``：引发不可重复读，幻读   oracle默认级别 
    - ``可重复读 repeatable read``：引发 幻读   .mysql 默认级别
    - ``序列化 serializable``：解决所有问题

8. 设置隔离级别

    - 查询隔离级别：``select @@tx_isolation``

    - 设置隔离级别：

        ```mysql
        set global transaction isolation level read uncommitted;  # 设置 读未提交级别
        set global transaction isolation level read committed;    # 设置 读已提交
        set global transaction isolation level repeatable read    # 设置 可重复读
        set global transaction isolation level  serializable      # 设置序列化
        ```



## DCL(DATA CONTROL LANGUAGE)

DCL数据控制语言

1. 管理用户

    - 添加用户

        ```mysql
        create user '用户名'@'主机名' identified by '密码';
        create user 'zhangsan'@'localhost' identified by '123';  # 创建localhost可访问密码为123的张三用户
        create user 'lisi'@'%' identified by '123';
        ```

    - 删除用户

        ```mysql
        drop user '用户名'@'主机名';
        ```

    - 修改用户密码

        ```mysql
        update user set password = password('新密码') where user='用户名';
        # 或者
        set password for '用户名' @ '主机名' = password('新密码')；
        
        # mysql中忘记root密码的修改方式
        	1. 停止mysql服务， 例如windows下 net stop mysql
        	2. 启动mysqld服务， mysqld --skip-grant-tables
        	3. 打开一个新的终端 进入mysql，此时无验证进入mysql
        	4. 修改root密码
        	5. 结束掉 mysqld 服务
        ```

    - 查询用户

        在mysql db下有一个user表

        ```mysql
        use mysql;
        select * from user;  
        ```

        

2. 管理权限

    - 查询权限

        ```mysql
        show grants for '用户名'@'主机名'；
        ```

    - 授予权限

        ```mysql
        grant 权限列表 on 数据库.表名 to '用户名'@'主机名'；
        grant update, select on db1.student to 'lisi'@'%';
        grant all on *.* to 'zhangsan'@'%'   # 授予所有库的所有表给张三，给张三所有权限
        ```

    - 撤销权限

        ```mysql
        revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
        ```

        

