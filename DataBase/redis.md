# Redis 

## 概念

Redis是一款高性能的``NOSQL``非关系型数据库。Redis数据库多用户缓存。即关系型数据库如 mysql、oracle 数据库的内存备份。

 

在大数据、大流量的今天，如果频繁的操作关系型数据库，会造成性能的损失（由于操作关系型数据库的速度）。因而就需要把一部分数据提前拿到内存中，作为缓存数据来加速对数据的访问。然而，若没有非关系型数据库，我们只能定义类似于Map的内存对象，对数据进行缓存。这么做有2点不足：

1. Map、List等对象只存在单个机器，无法进行分布式操作

2. Map、List受限于JVM。无法跨越JVM对内存操作的瓶颈。

    

Redis就是这样一个专业操作、管理内存的数据库。它多用于缓存关系型数据库，对数据进行备份。可以跨平台、跨机器，不受JVM的限制，可以占用某个服务器的所有内存资源。



## NoSQL产品

主流的NOSQL产品

- 键值(Key-Value)存储数据库
    				相关产品： Tokyo Cabinet/Tyrant、``Redis``、Voldemort、Berkeley DB
    				典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
    				数据模型： 一系列键值对
    				优势： 快速查询
    				劣势： 存储的数据缺少结构化
- 列存储数据库
    				相关产品：Cassandra, ``HBase``, Riak
    				典型应用：分布式的文件系统
    				数据模型：以列簇式存储，将同一列数据存在一起
    				优势：查找速度快，可扩展性强，更容易进行分布式扩展
    				劣势：功能相对局限
- 文档型数据库
    				相关产品：CouchDB、``MongoDB``
    				典型应用：Web应用（与Key-Value类似，Value是结构化的）
    				数据模型： 一系列键值对
    				优势：数据结构要求不严格 
    				劣势： 查询性能不高，而且缺乏统一的查询语法
- 图形(Graph)数据库
    				相关数据库：Neo4J、InfoGrid、Infinite Graph
    				典型应用：社交网络
    				数据模型：图结构
    				优势：利用图结构相关算法。
    				劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。



## redis的应用场景
- 缓存（数据查询、短连接、新闻内容、商品内容等等）

- 聊天室的在线好友列表
- 任务队列。（秒杀、抢购、12306等等）
- 应用排行榜
- 网站访问统计
- 数据过期处理（可以精确到毫秒)
- 分布式集群架构中的session分离



## redis 数据类型

Redis 是键值对系统。

### 键

键都是``String``类型

### 值

值包括以下几种类型

1. string， 字符串类型

    ```redis
    set myKey myValue      用set关键字来设置本类型， set key value
    get myKey              用get 关键字 来获取 key的值
    del myKey    		 删除某个键值对
    ```

2. Hash，即map类型

    ```redis
    hset mapName field value  
    	// 由于hash里面存的不是字符串，而是键值对，所以语法为。 
    	// hset 标识此处存储hash
    	// key 标识键
    	// field value 一对map 标识 这对map 的键和值
    hget mapName field   // 获取键值
    hdel mapName field   // 删除键值域
    hgetall mapName.     // 获取所有属于这个键 的域数据
    ```

3. list，列表类型。实现其实是``LinkList``

    ```redis
    lpush listName value   将元素添加到列表 listName 的左端
    rpush listName value   将元素添加到列表的右端
    lrange listName start end  获取范围内的元素，从左端开始计算
    lpop listName          从列表的左端删除元素，并返回
    rpop listName		从列表的右端删除元素，并返回
    ```

4. set，集合类型，其中元素不能重复

    ```redis
    sadd setName value  // 向 setName 集合中添加元素value
    smembers setName    // 获取集合 setName 中的所有值
    srem setName value  // 删除
    ```

5. Sortedset，有序集合类型 

    ```redis
    zadd setName score value  // 增加一个值value 到有序集合setName中，并根据传递的score 进行排序
    zrange  setName start end // 查询某个范围 ，start end 表示索引值， 0 -1 表示查询所有
    zrange  setName start end withscores // 表示查询某个范围，并携带scores显示
    zrem setName value    // 删除
    ```

    

### redis 通用命令

1. ``keys *``，获得redis中，所有定义的键。也可以传递一个pattern 正则
2. ``type key``，查询某个一键，是什么类型
3. ``del key``，删除指定的键值 n系统



## redis持久化

Redis是内存数据库，需要持久化操作。两种机制实现Redis持久化：

1. ``RDB``，Redis的默认持久化方式。在一定时间内，检测key值的变化情况。达到某种指标，例如，有10w个key被更改了，则记性持久化

    - 编辑redis.conf文件

    - 修改 

        > save 900 1     # 表示900秒内有1个数据发生改变就持久化一次
        >
        > save 300 10  # 300秒内，10次
        >
        > save 60 10000  # 60秒内 10000次

    - 启动redis服务器，并携带该配置文件作为参数

        ```bash
        redis-server redis-conf
        ```

        

2. ``AOF``，日志方式，即每有一个key发生修改，就进行持久化。这种方式效率低，也不推荐。这实际上使得Redis退化成磁盘文件，即关系型数据库的操作机制。该种方法使用如下：

    - 修改redis.conf文件
    - 修改``appendonly`` 为 yes。则开启AOF方式
    - ``appendfsync always``，修改为同步always，他有三中备选值always，sec(默认)，no。表示总是同步、每秒同步、不同步



## Jedis

Jedis类似于JDBC，他是一款操作redis的Java工具



### 使用步骤

1. 导入jar包
2. 获取连接
3. 操作
4.  关闭连接

```java
Jedis jedis = new Jedis("localhost", 6379);   // 获取连接

// String 类型
jedis.set("username", "zhangsan");  // 设置键值对。其他方法如，get，del同理
jedis.setex("key", 60, "value")    // jedis中的特殊方法 setex，他可以设置一个键值对，并设置一个过期时间，时间到了，删除键值对。可以用于激活码，等有短期时限的数据
    
// hash 类型
jedis.hset("user", "username","zhangsan");  // 设置
jedis.hset("user", "password", "123456");
String username = jedis.hget("user", "username"); // 获取
Map<Stirng, String> userInfo = jedis.hgetall("user");  // 获取某键的所有数据

// Jedis 中的方法 和 redis 中的方法神同步

jedis.close();   // 关闭连接
```



### Jedis 连接池

Jedis连接池，``JedisPool``

使用步骤：

1. 创建JedisPool对象
2. 调用getResource()方法，获取Jedis连接

```java
JedisPoolConfig config = new JedisPoolConfig();  // 配置连接池的对象
config.setMaxTotal(50);   // 配置连接池连接数
config.setMaxIdel(10);    // 最大的空闲连接数量。  config里面有一大堆配置函数

JedisPool jedisPool = new JedisPool(config);   // 创建连接池，并传递配置信息
Jedis jedis = jedisPool.getResource();
jedis.set("username","zhangsan"); ......

jedis.close()  // 关闭redis连接资源，此处不是真正的关闭，而是返回资源给连接池 
```



连接池的其他配置属性

```
可以将这些数据写到一个.properties 文件中，在加载jedisPool时，将其读取并设置给config

#最大活动对象数     
redis.pool.maxTotal=1000    
#最大能够保持idel状态的对象数      
redis.pool.maxIdle=100  
#最小能够保持idel状态的对象数   
redis.pool.minIdle=50    
#当池内没有返回对象时，最大等待时间    
redis.pool.maxWaitMillis=10000    
#当调用borrow Object方法时，是否进行有效性检查    
redis.pool.testOnBorrow=true    
#当调用return Object方法时，是否进行有效性检查    
redis.pool.testOnReturn=true  
#“空闲链接”检测线程，检测的周期，毫秒数。如果为负值，表示不运行“检测线程”。默认为-1.  
redis.pool.timeBetweenEvictionRunsMillis=30000  
#向调用者输出“链接”对象时，是否检测它的空闲超时；  
redis.pool.testWhileIdle=true  
# 对于“空闲链接”检测线程而言，每次检测的链接资源的个数。默认为3.  
redis.pool.numTestsPerEvictionRun=50  
#redis服务器的IP    
redis.ip=xxxxxx  
#redis服务器的Port    
redis1.port=6379   
```

