# Nginx 服务器

## Nginx 概念

Nginx是一个可以安装到LInux 系统上的软件。它可以用作

1. http服务器，多用于解析静态资源
2. 虚拟主机
3. 反向代理服务器，实现负载均衡

它的优点是高性能，高稳定性

## Nginx 安装

### 在安装Nginx之前需要安装

1. ``gcc``
2. ``pcre``
3. ``zlib``
4. ``openssl``

### 下载Nginx安装包

1. 下载``nginx-1.8.0.tar.gz``

2. 解压压缩包
3. 执行config 命令生成makefile文件
4. 执行make，生成编译文件
5. 执行 make install 命令，进行nginx的安装



## Nginx 运行

1. 切入到``sbin``目录下
2. 运行nginx ``./nginx``执行
3. 强制结束nginx`` ./nginx -s stop``
4. 正常（保存配置）退出nginx``./nginx -s quit``
5. 刷新配置文件，重启``./nginx -s reload``



## Nginx 功能

###  高性能 HTTP服务器（静态资源服务器）

静态资源就是不用动态配置内容的资源。Nginx可以很容易的**拿到**这种资源并转发出去

1. 打开``conf/nginx.conf``文件，进行nginx配置

2. 里面有配置如下

    ```conf
    server{
    	location / {					# 表示配置服务器的 / 路径
    		root html;					# 表示配置的该路径 / ，映射到服务器的的 html 文件夹下
    		index index.html index.htm;		# 默认浏览这个文件夹下的这两个两个文件
    	}
    }
    
    # 类似于这种方式 配置 location 的方式，就能让nginx 帮相互服务器解析静态资源。速度很快
    ```

    

### 虚拟主机

虚拟主机又叫“网络空间”技术。它就是把一台物理服务器，虚拟的划分为多台虚拟的服务器。服务器之间访问互不干扰。云技术，就是基于虚拟主机技术，它是对资源的一种虚拟划分、分配技术。

配置虚拟主机

1. 打开``conf/nginx.conf``文件，进行nginx配置

2. 配置多个server``端口``实现虚拟主机

    ```conf
    server{
    	listen 80;
    	server_name 192.168.1.22;
    	location / {
    		root index;		# 根目录指向服务器的index目录
    		index index.html;	# 默认返回页面为index.html
    	}
    }
    server{
    	listen 81;			# 另一项目监听81端口
    	server_name 192.168.1.22;   
    	location / {
    		root resig;		# 根目录指向服务器的 resig 目录
    		index register.html;	# 默认返回页面为register.html
    	}
    }
    # 配置了两个服务器，在同一个物理服务器。不同项目放在不同目录下，通过nginx的访问端口进行隔离访问，实现虚拟主机
    ```

3. 配置多个``域名``实现虚拟主机

    ```conf
    # 首先为了演示，在/etc/hosts文件下，将不同的域名都映射到一个主机（nginx所在的主机）上
    
    192.168.1.22 www.index.com
    192.168.1.22 www.regist.com
    
    # 这样不同域名都能解析到同一个安装了nginx的服务器上了
    ```

    

    ```conf
    server{
    	listen 80;				# 端口相同
    	server_name www.index.com; 	# 虽然IP相同，即DNS解析出的IP地址相同，但是域名仍然不同
    	location / {
    		root index; 	 # 根目录指向服务器的index目录
    		index index.html; # 默认返回页面为index.html
    	}
    }
    server{
    	listen 80;				  # 端口相同
    	server_name www.regist.com;   # 域名不同
    	location / {
    		root resig;		# 根目录指向服务器的 resig 目录
    		index register.html; # 默认返回页面为register.html
    	}
    }
    # nginx 能将不同域名所来的请求分别转发到不同虚拟主机上。即使端口相同（这样DNS解析的ip地址，和端口号都相同）。
    # 但是域名不同，nginx仍能将他们所请求的不同资源拿到。
    ```

    

### 反向代理服务器与负载均衡

1. 打开``conf/nginx.conf``文件，进行nginx配置

    ```conf
    # 反向代理的配置，upstream。配置一台tomcat服务器，名为 tomcat-server1
    # 即若有请求需要使用代理，nginx代理去这个服务器server 192.168.1.22:80;去查找资源
    upstream tomcat-server1 {    # 配置代理
    	server 192.168.1.22:80;   # 配置 需要被代理的服务器的IP和端口号
    }  
    
    
    server{
    	listen 80;				# 配置服务器资源，端口号
    	server_name www.index.com;  # 配置服务器资源域名
    	
    	# 配置代理，上面的例子是配置root。即有人访问 /时，从root目录拿取资源
    	# 但是此处是代理，所以不从 root index，中拿了，从代理所指明的地方http://tomcat-server1去拿
    	location / {
    		proxy_pass http://tomcat-server1;    # 使用代理，去找位置
    		index index.html; # 默认返回页面为index.html
    	}
    }
    ```

2. 搭建tomcat集群，通过nginx反向代理，实现负载均衡。

    > 在 192.168.1.22这台物理主机上，配置了一个tomcat服务器，端口号为8080
    >
    > 即 192.168.1.22:8080
    > 
    >
    >
    > 在 192.168.1.23这台物理主机上，配置了两个tomcat服务器，端口号为8080和8081
    >
    > 即 192.168.1.23:8080 ， 192.168.1.23:8081
    >
    > 总共3台tomcat服务器

    打开``conf/nginx.conf``文件，进行nginx配置

    ```conf
    upstream tomcat-server1 {      # 配置代理
    	server 192.168.1.22:8080;   # 在代理配置的搜索资源处，添加多条，就自动实现了nginx反向代理集群
    	server 192.168.1.23:8080;
    	server 192.168.1.23:8081; 
    	# nginx 反向代理会自动将访问 www.index.com:80的请求，平均分发到上面这三台tomcat服务器上去。
    }  
    
    
    # server 内容不变
    server{
    	listen 80;				# 配置服务器资源，端口号
    	server_name www.index.com;  # 配置服务器资源域名
    	
    	# 配置代理，上面的例子是配置root。即有人访问 /时，从root目录拿取资源
    	# 但是此处是代理，所以不从 root index，中拿了，从代理所指明的地方http://tomcat-server1去拿
    	location / {
    		proxy_pass http://tomcat-server1;    # 使用代理，去找位置
    		index index.html; # 默认返回页面为index.html
    	}
    }
    ```

    若要实现不同的负载比例可使用``weight``

    ```conf
    upstream tomcat-server1 {      # 默认权重为 1
    	server 192.168.1.22:8080 weight=2;  
    	server 192.168.1.23:8080;
    	server 192.168.1.23:8081; 
    	# 这样权重就形成了 2:1:1。这样 192.168.1.22:8080 这台服务器就占了50%的服务量
    }  
    ```

    

    

