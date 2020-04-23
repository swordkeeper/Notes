# Mysql 数据库

### 注释

``--``

### 链接数据库

```bash
# 链接数据库
mysql -u root -p

# 退出数据库
exit
```

### 常用字段类型

- 整数，int(各种int)
- 小数，decimal。decimal(5,2)，表示小数占5位，小数部分占2位 
- 字符串，varchar，char。char(3)，表示固定长度字符串，3表示固定长度为3。varchar(3)，表示可变长度字符串，最长为3
- text，存储大文本
- 日期类型，date，time，datetime
- 枚举类型，enum
- 数据库中只存储图片在服务器的路径，或者是url
- Unsigned, 无符号

### 约束

- 主键 ``primary key``，物理存储顺序
- 非空 ``not null``
- 唯一``unique``，此字段不允许重复 
- 外健``foreign key``

### 数据库操作

- 创建数据库

    ```sql
    -- 创建数据库
    create database myDatabase;
    
    -- 显示创建数据库的详细信息
    show create database myDatabase; 
    
    -- 创建数据库时，添加字符集
    create database myDatabase1 charset=utf8;
    ```

- 删除数据库

    ```sql
    drop database `myDatabase`;
    ```

- 使用数据库

    ```sql
    use myDatabase;
    -- 查看当前使用的datbase
    select database();
    ```

- 创建数据表

    ```sql
    -- 显示数据库中的表
    show table;
    
    -- 创建表格
    create table  myTable(id int, name varchar(20));
    
    -- 显示表的详细信息
    desc myTable;
    
    -- 添加数据主键
    create table  myTable(id int primary key not null auto_increment, name varchar(20));
    
    create table students(
       id int unsigned not null auto_increment primary key,
       name varchar(20),
       gender enum("男","女") default "保密"
    );
    ```

- 改变表

    ```sql
    alter table 表名 add 列名 -- 增
    alter table 表名 change 原名 新名 约束  --改名
    alter table 表名 modify 列名 约束    -- 改字段
    alter table 表名 drop 列名    -- 删
    alter table add foreign key (本列) references 外健表名(外健列)
    ```

- CDUR（增删改查）
	- 插入数据

    ```sql
    insert into students values(0, "老王", "男")
    ```
    
  - 修改数据
  
      ```sql
      update 表名 set 列名="value",列名="value" where 列="value" and 列="value"
      ```
  
  - 查找
  
      ```sql
      select * from 表名 where id>8; 
      select name as "姓名" from 表名 where id >11
      ```
  
  - 删除数据/标记删除
  
      ```sql
      delete from 表名 where 列="value"
      alter table student add is_delete bit default=0 -- 添加一个字段，默认为0，用于标记删除
      ```
  

### 查询

 - 查找
  
      ```sql
      select * from 表名 where id>8; 
      select name as "姓名" from 表名 where id >11
      ```

- 去重

    ```sql
    select distinct gender from student where id>18 -- 去重查询，只返回去重复的性别
    ```

- 条件查询

    - ``=``等于
    - ``<>``或``!=``不等于
    - ``>``和``<``
    - ``and``、``or``、``not``

- 模糊查询条件

    - ``like``，用``%``替换多个，用``_``替换一个

        ```sql
        select * from myTable where name like  "小%" --匹配所有以小开头的，如小明，小王
        ```

    - ``rlike``，用正则表达式来匹配

- 范围查询

    - ``in (1, 3, 8)``表示一个非连续的范围，即只能在1，3，8三个值中选。 `` not in (1,2,6)``
    - ``between 12 and 18``表示一个连续范围，在[12, 18]。``not between xx and xxx``

- 判断为空

    - ``is null`` / ``is not null``

- 查询时间

    - ``set profiling=1``
    - ``show profiles``查看执行时间

### 管理命令

```sql
-- 显示数据库
show databases;

-- 显示当前时间
select now();

-- 显示当前版本
select version();
```

### 引擎

```sql
ENGINE=InnoDB
-- InnoDB和MyISM引擎
-- InnoDB支持事务、外健、行级锁
```

### 索引

```sql
create index index_name on myTable(title(10));   --在myTable表下的title列，建立每隔10个单位的索引，名为index_name
-- 为了查询更快速
show index from myTable; -- 查看一个表的索引 

drop index 索引名 on 表名 ; -- 删除索引， 所以一般用于查询多的 列， 更新频繁的不适合建立索引
```

### 授权用户

```sql
grant 权限列表 on 数据库 to '用户名'@'访问主机' identified by '密码';

-- 例如
grant select on myDatabase.* to 'xiaowang'@'localhost' identified by '123456';
-- 授权xiaowang在localhost下登陆时，可以使用select命令查询myDatabase下的所有表， 密码123456

-- grant all priviliage on ....

-- 修改权限, with grant option 添加了 update权限
grant select,update on myDatabase.* to 'xiaowang'@'localhost' identified by '123456' with grant option;
flush privileges;  -- 刷新权限
```



### 修改用户密码

```sql
update user set authentication_string=password('新密码') where user='用户名';
例：
update user set authentication_string=password('123') where user='laowang';
flush privileges
```

### 删除用户

```sql
drop user "用户名"@"地址"
```

### 导出数据库

```bash
mysqldump -uroot -p --lock-all-tables 数据库名字 > newsql.sql # --lock-all-tables表示锁住所有表，防止导出时有数据变动
```

## 服务器管理

### 主从服务器

1. 主服务器，接受删除，插入，修改的sql
2. 从服务器，接受查询sql
3. 主服务器接受修改之后，会有log文件
4. 从服务器检测，读主服务器的log文件，如果有更新，则执行log
5. 所有查询的sql，访问从服务器