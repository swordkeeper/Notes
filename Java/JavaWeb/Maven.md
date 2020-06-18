# Maven

Maven是对Java工程的管理打包系统。

## 功能

1. 依赖管理
2. 一键构建





## 依赖管理

一个电脑中，可能有多个工程在运作或处在开发阶段。这样就需要对每个工程中使用的包进行加载。

**传统上，一个Web工程，都有自己的目录结构，保存所依赖的Jar包**。这样在有多个工程的开发环境，就需要存储多份相同的Jar包。

**由Maven管理的Jar包，实际的Jar包存储在一个``Jar仓库``之中**，每个工程在使用时，只存储一个指向Jar仓库的映射连接。因而大大减少了空间的重复占用 



### 仓库位置。

Maven所构建的工程中，Jar包实际是一个指向Maven包仓库的地址。实际包存在maven仓库中

Maven仓库的位置由 ``conf/settings.xml``中的:

1.  localRepository 配置。默认在``~/.m2/repository``下。该仓库表示本机在的Maven仓库路径
2. 若本机能联网，则Maven工具还会去 ``互联网上中央仓库``去寻找Jar包
3. 为了方便开发，公司都将``互联网中央仓库``中的某些Jar包，下载到``远程仓库``中做备份。然后将Maven仓库包查找索引指向该 ``远程仓库``。即从本地Maven仓库寻找不到Jar包，先到``远程仓库``查找。然后再到中央仓库查找。
4. ``远程仓库``，远程仓库又称为私服。表示里面的包，或者从互联网上下载到局域网服务，或者从本机上传。



### 一键构建

传统的Web工程构建方式为

> 1. 编码
> 2. 编译
> 3. 测试
> 4. 试运行
> 5. 打包
> 6. 上传服务器，并安装
> 7. 部署，运行

这每一步都需要认为的设置与操作。

``Maven``一键构建，可以帮助我们根据配置文件，一键完成上述操作。 该配置文件为``pom.xml``。其中包含Jar包的映射地址





## Maven工具的目录结构

1. 首先解压安装Maven下载安装包

    - 目录结构：
        - bin：Maven工具命令目录。
        - boot：Maven运行的类加载器
        - conf：settings.xml是Maven配置文件
        - libs：Maven自身运行所以来的依赖包

2. 配置环境变量

    ``MAVEN_HOME`` = 主目录，

     在Path中配置MAVEN_HOME/bin。将maven命令添加进指令路径

3. 运行mvn -v 命令，成功显示，表示安装成功

    



## Maven项目的目录结构

Maven项目目录结构

```
Maven项目名
	src
		main			代码主目录
			java 		核心代码目录
			resources 	核心代码配置目录
			webapp		如果是web工程，还会有本目录，里面放置的都是web页面资源
				js
				css
				img
		test			测试主目录
			java		测试核心代码目录
			resources	测试资源配置目录
		
		
```



## Maven 命令

1. ``Mvn clean``，清除本工程下的原始生成代码，用于重新生成干净的Maven工程环境。该命令会清除目录下的``target``目录
2. ``mvn compile``，编译工程项目，会在本目录下生成``target``目录。该编译只是编译``src/main``下面的代码
3. ``mvn test``，编译test目录下的代码， 并运行测试。该方法也会在target目录下生成相关的目录结构。若没有编译main中的主代码，该命令还会编译src/main下的代码
4.  ``mvn package``，将test，main中的代码进行编译，然后并打包成war包存于target目录之下
5. ``mvn install`` ，该命令不仅执行了``mvn package``命令的所有操作，他还将该包安装到了 ``maven 本地仓库`` 
6. ``mvn deploy``，部署。将打包并安装的maven工程进行部署。部署之前需要对maven进行配置





## Maven 项目配置

Maven结构图如下

<img src="../image/maven结构图.png" width=80%/>

### 项目配置模型

醒目配置模型，即对maven要构建的项目进行配置，也就是对``pom.xml``文件进行设置。POM表示 ``Project Object Model``项目对象模型

该配置文件中包含三个大的部分：

1. 项目自身信息(POM模型)
2. 项目所依赖的Jar包的信息（依赖管理模型）
3. 项目所用到的插件信息，包括JDK，Tomcat服务器配置，JDBC连接配置等。甚至maven命令，包括compile、test、install等都需要这些底层插件