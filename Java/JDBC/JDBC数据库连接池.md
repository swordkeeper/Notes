# 数据库连接池

所有的连接池都实现了：

``javax.sql``下的``DataSource``接口，具体实现由各大数据库厂商实现

#### 连接池方法：

1. 获取连接 ``getConnection()``
2. 归还连接 ``close() ``。   各大厂商的实现重写了close()方法，使他不再是关闭，而是归还连接到连接池



#### 两种实现

- ``C3P0``

    实现步骤：

    1. 导入jar包
    2. 配置文件``c3p0.properties``或者``c3p0-config.xml``
    3. 创建核心连接池对象``combPooledDataSource``    DataSourcr ds = new CombPooledDataSource()
    4. 获取连接``getConnection`` 

- ``Druid``，由阿里实现 

    实现步骤：

    1. 导入jar包

    2. 配置相应properites文件

    3. 获取数据库连接池对象：通过工厂类获取。

        ``DruidDataSourceFactory.createDataSource(FileInputStream druid.properties)``

    4. ``getConnection()``



#### Spring Template

Sprint Template 是由Spring框架对JDBC的封装

实现步骤：

1. 导入jar包

2. 创建 ``JdbcTemplate``对象

    JdbcTemplate jt = new JdbcTemplate( DataSource )

3. 调用template 的方法 来完成CRUD

    方法：

    - ``update（sql, args...)``增删改

    - ``queryForMap``查，并封装为map

    - ``queryForArray``查，并封装为Array

    - ``query``查，并封装为 JavaBean

    - ``queryForObject``，查并封装为object 

        

