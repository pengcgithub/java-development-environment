# mycat 读写分离

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

## 环境准备
基于mycat读写分离是基于mysql主从复制的基础之上进行搭建的，如果不熟悉主从复制可以参考[mysql 主从复制](./mysql主从复制.md)博文

- 三台服务器
	- 106.15.92.216 Mycat服务器
	- 47.100.62.123 Mycat主库
	- 106.15.183.40 Mysql从库
- JDK： `jdk1.8`
- Mycat：`Mycat-server-1.6.5.tar.gz` [下载地址：http://dl.mycat.io/](http://dl.mycat.io/)

## mycat

### 安装
- 安装JDK，并配置JAVA_HOME环境。
- 添加用户：`useradd mycat`
- 设置密码：`passwd mycat`
- 进入软件包存在目录：`cd /opt/setups`
- 解压：`tar -zxvf Mycat-server-1.6.5-release-20180122220033-linux.tar.gz`
- 移动解压目录至安装目录：`mv /opt/setups/mycat /usr/local`
- 设置 Mycat 的环境变量：`vim /etc/profile`
<pre>
export MYCAT_HOME=/usr/local/mycat
export PATH=$PATH:$MYCAT_HOME/bin
</pre>
- 刷新配置：`source /etc/profile`

### 配置

博文只简单配置，详情配置可参考Mycat官方文档。

- schema.xml：用于设置 Mycat 的逻辑库、表、数据节点、dataHost 等内容，分库分表、读写分离等等都是在这里进行配置的

```

	<!-- 定义MyCat的逻辑库 -->
	<schema name="TESTDB" checkSQLschema="false" sqlMaxLimit="100" dataNode="store"></schema>
	<!-- 定义MyCat的数据节点 -->
	<dataNode name="store" dataHost="dh1" database="store" />
	<dataHost name="dh1" maxCon="1000" minCon="10" balance="1" writeType="0" 	dbType="mysql" dbDriver="native" switchType="1"  slaveThreshold="100">
		<!-- 心跳监测 -->
		<heartbeat>select user()</heartbeat>
		<!-- 主库 -->
		<writeHost host="hostM1" url="47.100.62.123:3306" user="root"
				   password="123456" />
		<!-- 从库 -->
		<writeHost host="hostS1" url="106.15.183.40:3306" user="root"
				   password="123456" />
	</dataHost>

- server.xml：主要用于配置系统变量、用户管理、用户权限等。

```

	<!-- 用户1，对应的MyCat逻辑库连接到的数据节点对应的主机为主从复制集群 -->  
    <user name="root">  
        <property name="password">123456</property>  
        <property name="schemas">TESTDB</property>  
    </user>  
                              
    <!-- 用户2，只读权限-->
    <user name="user">  
        <property name="password">123456</property>  
        <property name="schemas">TESTDB</property>  
        <property name="readOnly">true</property>  
    </user>  


- log4j2.xml：开启debug模式，启动之后会在logs文件夹中生成mycat.log日志文件，一般生产环境会设置为info

![](https://i.imgur.com/vXMkYyH.png)


- 端口：如果开启防火墙，需要配置访问端口
	- 默认数据端口：8066
	- 默认管理端口：9066

- 启动/停止/重启
	- 后台启动：`mycat start`
	- 控制台启动：`mycat console`
	- 重启：`mycat restart`
	- 停止：`mycat stop`

### 测试

连接 Mycat 的过程跟连接普通的 MySQL 表面上是没啥区别的，使用的命令都是一个样。

- 连接： `mysql -uroot -h127.0.0.1 -P8066 -p`
- 验证：
	- 查询验证
![](https://i.imgur.com/9RxCgju.png)
	- 插入验证
![](https://i.imgur.com/Lxvy89v.png)

## 问题

- mycat启动后自动关闭服务，并在mycat根目录出现类似于`hs_err_pid28000.log`的日志文件

由于服务器内存不足，导致java虚拟机崩盘，所以才会在mycat根目录出现`hs_err_pid*.log`类似的问题。[https://blog.csdn.net/u013938484/article/details/51811400](https://blog.csdn.net/u013938484/article/details/51811400)

- 数据乱码问题
	- my.cnf配置 防止乱码问题`default-character-set=utf8`