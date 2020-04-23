# docker

### 命令

- 限制容器的运行空间

    ```bash
    docker run --memory=200M image名字 
    # 默认memory设置200M ，memory-swap也memory相等。总的memory(虚拟内存)就是400M，因而运行一个300M的程序不会崩溃
    ```

- 设置运行docker优先级

    ```bash
    docker run --cup-shares=10  -d --name=test1 myImage # 后台运行了image，cpu份额为10
    docker run --cup-shares=5 -d  --name=test2 myImage  # cpu份额为5
    # 最终test1占用cpu达到 66.6%
    ```

- 进入正在后台运行的container

    ```bash
    docker exec -it containerId
    ```

    

### 配置Dockerfile

- ``RUN``，在构建build，image时候执行的命令

- ``EXPOSE``，暴露端口

- ``ENTRYPOINT``，在image run 的时候一定会被执行的的命令（后台）

- ``CMD``，在image run 时会被执行的命令（前台）。但是若执行run命令时提供了参数，该CMD会被覆盖。如``docker image run -it image名 /bin/bash``，则bin/bash命令会覆盖该命令。同理若有多条``CMD``，也只有最后一个会被执行

- 样例1(web服务器)

    ```dockerfile
    FROM python2.7  # 引用base image
    LABEL maintainer = "Chen Rui<chenrui.apple@gmail.com>"  # 设置联系信息标签
    RUN pip install flask # 当build运行dockerfile时，自动运行的命令
    COPY app.py /app/     # 拷贝本地文件app.py到image内 /app/路径下，该路径若不存在，自行建造
    WORKDIR /app          # 切换当前目录到 /app
    EXPOSE 5000           # 暴露要被访问的5000端口
    CMD ["python", "app.py"] # 当run该image时，执行的脚本。即自动启动flask程序
    ```

- 样例2(stress压力测试)

    ```dockerfile
    FROM ubuntu
    RUN apt-get update && apt-get install -y stress # 安装stress命令
    ENTRYPOINT ["/usr/bin/stress"]  # 主流的运行方式都这么写。写一个一定会执行的ENTRYPOIN，然后等待CMD传入参数
    CMD []   # 从终端输入命令启动时，CMD被替换，因而 最终执行的命令为   /usr/bin/stress +传入参数
    ```




### Docker Compose

docker compose 类似于bash的批处理文件``.bat``。它提供批处理运行、部署container的方法。

``docker-compose``，命令可以执行一个``.yml``类型的文件。该格式的文件可以指名image的拉取、分发、容器的运行部署等等。该文件默认名字为``docker-compose.yml``。

文件中包含三个概念：

``services``、``networks``、``volumes``

样例一：

```yaml
services:
	db:
		image: postgres:9.4
		volumes: 
			- "db-data:/var/lib/postgressql/data"
		networks:
			- back-tier
```

样例二

```yaml
# docker-compose.yml
version: "3"

services:   # service 类似于docker run命令
	wordpress:  # 服务名
		image: wordpress # 服务用到的image
		ports: 	         # 端口转换，外层port:容器port
			- 8080:80
		environment:     # 设置容器运行时的环境变量
			WORDPRESS_DB_HOST: mysql
			WORDPRESS_DB_PASSWORD: root
		networks:        # 所使用到的网络
			- my-bridge
		links:
			- mysql:mysql
			
	mysql: 
		image: mysql:5.7
		environment: 
			MYSQL_ROOT_PASSWORD: root
			MYSQL_DATABASE: wordpress
			
		# 运行mysql时，使用到的volume。 外部对应名为mysql-data的容器，对应容器内部/var/lib/mysql
		volumes: 
			- mysql-data:/var/lib/mysql
		networks:
			- my-bridge
volumes: # 定义一个名为mysql-data的容器空间
	mysql-data:

networks: # 定义网络
	my-bridge:
		driver: bridge # 定义网络驱动
```

样例三

```yaml
version: '3'

services: 
	redis: 
		image: redis
	web:
		build:        # 不通过image，通过docker构建image，然后启动container
			context: .
			dockerfile: Dockerfile
		ports:
			- 8080:5000
		environment:
			REDIS_HOST: redis
```



命令：

- ``docker-compose up``，启动yaml文件中的模块。``-f``参数，制定yaml文件路径，``-d``参数，后台执行

- ``docker-compose ps``，查看启动的container

- ``docker-compose stop``，停止某个container。``docker-compose down``，停止并删除某个容器

- ``docker-compose exec``，进入某个容器

- ``docker-compose scale``，扩展容器，即可以启动多个相同的容器，来动态配置服务能力

    ```bash
    docker-compose up --scale web=3 -d # 启动三个名为web的服务。如果出现端口绑定，则会报错（重复绑定端口）
    ```

Docker-compose 启动的命令的service名字，与实际运行的容器名字有一点出入



**在swarm模式下，不用``docker-compose``，用``docker stack``命令**

```bash
# 部署了一个名为wordpress的stack（service集合），并制定配置文件
docker stack deploy wordpress -c=docker-compose.yml 
```



### Swarm

Swarm是docker官方开发的集群不是套件。用于分布式集群式部署

1. 在集群manager机器上运行

    ```bash
    docker swarm  init --advertise-addr=192.168.205.10 # 初始化一个集群管理者，并声明本机的地址
    ```

2. 在worker节点上运行

    ```bash
    docker swarm join ......token.......
    ```

3. 返回manager节点，查看集群状态

    ```bash
    docker node ls
    ```

4. 创建``service``

    swarm 模式下的service是会被自动分配到一个worker节点上运行的。该分配机制有manager配置

    在swarm模式下（加入swarm集群的manager机器上），运行以下命令

    ```bash
    docker service create --name demo busybox # 创建service，名为demo，使用busybox image
    # 类似于run命令。但在集群模式下，不使用run命令，run命令只在本地建立一个container运行，
    # 而service命令则是根据集群的配置，动态分配该service下创建的container到worker上运行
    ```

5. 查看运行状态

    ```bash
    docker service ls  # 查看运行情况
    docker service ps demo # 查看demo service的运行状况
    ```

6. 扩展service的运行

    ```bash
    docker service scale demo=5 # 扩展5份，即同一个服务分配了5份，同时放到不同worker上运行。增加性能
    ```

    失效的、异常的service会被自动重建



### Swarm集群中的 Secret

``docker secret``只能在swam中使用

1. 创建密码

    ```bash
    # 从字符文件password中读取字符，来创建密码文件my-pw
    docker secret create my-pw password 
    # 从STDIN，来创建密码
    echo "THIS IS A PASSWORD" | docker secret create my-pw-1
    ```

2. 查看密码集

    ```bash
    docker secret ls
    ```

3. 创建服务时，分发密码

    ```bash
    docker service create --name client --secret my-pw-1 busybox # 分发密码 my-pw-1
    ```

    

### swarm 中service的更新（生产环境）

1. 首先对service 进行 ``scale``，因为如果只有一个replica，更新就会中断业务

    ```bash
    docker service scale web=2 # 跟新web service，让他变多，至少有两个。使得原先服务不会当机
    ```

2. 更新service 的image

    ```bash
    # 更新 web service，用新的image busybox:2.0来替换 busybox:1.0.
    docker service update --image busybox:2.0 web 
    ```

3. 更新service 的 port

    ```bash
    docker service update --publish-rm 8080:5000 --publish-add 8088:5000 web
    ```

    

---



### Kubernetes

Kubernetes 是与swarm类似的集群式编排框架，其架构与swarm类似也是为主从模式``master``和``node``

``master``，中包含4大模块：

- ``API Server``，用于与``User Interface``和``CLI``交互
- ``Scheduler``，执行调度算法，来分配task
- ``Controller``，实际执行调度，更新等操作
- ``etcd``，分布式``key-value store``，存储master中共享信息

``Node``，包含的模块

- ``pod``，具有**相同namespace**的container 的组合，是调度的最小单位。在k8s中，不对container直接进行操作。而是对pod进行操作。

    - 一个pod具有：``共享一个namespace``，``公用的一个或几个volume``，``一个或多个container``

- ``kubelet``，在node上接受master的命令，执行本node管理操作

- ``kube-proxy``，执行网络的操作，也进行负载均衡操作

- ``docker``

- ``fluentd``，实现数据采集工作

    

###  minikube

``kubectl``，是一个连接集群的用户端控制台工具。他在连接集群的时候回创建一个上下文`` kubectl context``，默认叫做``minikube``。可以通过以下命令查看

- 完善命令文档（可以tab显示命令补全）

    ```bash
    source < (kubectl completion bash) # souce一下，一个配置。配置文件会通过kubectl completion命令补全到bash
    ```

- 上下文

    ```bash
    kubectl config view  # 查看 minikube 上下文中的配置
    kubectl config get-contexts  # 查看所有的上下文
    kubectl config use-context tectonic # 切换使用集群到 tectonic 上下文（集群）
    ```

- 查看集群

    ```bash
    kubectl cluster-info 
    ```

- 进入集群虚拟机

    ```bash
    minikube ssh
    ```

- 资源（pod，network等）

    ```bash
    kubectl create -f "yml file url " # 根据yaml文件创建资源
    kubectl delete -f "yml file url " # 根据yaml文件删除资源 
    kubectl get pods # 查看pod集合信息
    kubectl get pods -o wide # 查看详细信息
    kubectl get rc # 查看 replication Controller 的详情
    kubectl exec -it nginx /bin/bash # 进入集群中nginx 容器
    kubectl describe pods nginx # 查看pod nginx 的详细信息
    kubectl port-forward nginx 8080:80 # 将外部minicube外部8080端口映射到，内部名为nginx 的pod的80端口 
    
    ```

- 对pod进行横向扩展

    ```yaml
    # 配置yaml的文件(rc模式)
    apiVersion: v1
    kind: ReplicationController # 指定rc模式
    metadata:
        name: nginx
    spec:
        replicas: 3
        selector: 
            app: nginx
        template:
            metadata: 
                name: nginx
                labels:
                    app: nginx
            spec:
                containers:
                - name: nginx
                  image: nginx
                  ports: 
                  - containerPort: 80
    #------------------------------------------------------------------- #                      
    # 配置yaml的文件(rs模式)
    # 配置yaml的文件
    apiVersion: apps/v1
    kind: ReplicaSet # 指定rs模式
    metadata:
        name: nginx
        labels:
            tier: frontend
    spec:
        replicas: 3
        selector: 
            matchLabels:
                tier: frontend
    
        template:
            metadata: 
                name: nginx
                labels:
                    tier: frontend
            spec:
                containers:
                - name: nginx
                  image: nginx
                  ports: 
                  - containerPort: 80
    ```

- 删掉某个资源

    ```bash
    kubectl delete pods nginx-5kcbj # 删掉某个pod下 nginx-5kcbj replicas
    ```

- 增加某个pod的replication

    ```bash
    # 通过scale 命令增加 replication controller下的nginx 容器的relicas数目
    kubectl scale rc nginx --replicas=2
    ```

- **deployment**

    该模式类似于``ReplicationController``和``ReplicaSet``模式，他是一种动态布局模式。它规定了一种倾向，

    即维护人员，有倾向做什么。比如扩展scale，删除，等。在时机合适的时候，系统会执行update等操作。

    在``deployment``模式中，维护人员不能单独的对replica，pod进行操作。所有的操作必须经过deployment。有它来操作

    1. ``kubectl get deployment -o wide``，列出所有的deployment
    
    2. 升级版本，``kubectl set deployment nginx-deployment nginx=nginx:1.13``。设置名为niginx-deployment的 deployment中nginx参数，使得其为nginx:1.13
    
    3. 查看部署版本和回滚
    
        ```bash
        kubectl rollout history deployment nginx-deployment # 查看历史部署
        kubectl rollout undo history deployment nginx-deployment  # 回滚到上一次部署
        ```
    
    4. 将deployment 部署服务，映射到node 端口
    
        ```bash
        kubectl expose deployment nginx-deployment --type=NodePort
        ```
    
    5. 动态编辑yaml文件，使得cluster配置动态更新（编辑后，cluster就更新）
    
        ```bash
        kubectl edit deployment serveice_name # 编辑deployment部署的，名为service_name的服务所使用的yaml文件
        ```
    
        
    
- service 

    service 是集群暴露给外界的一个一个接口，他可以固定某些IP地址给某些pod，使其在update时候不发生变化。service通过``kubectl expose``命令创建。他有三种模式``ClusterIP``，``NodePort``，``LocalBalancer``。

    1. ``ClusterIP``，即cluster内部可以访问的IP地址，集群外部访问不到该地址



