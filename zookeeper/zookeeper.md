# zookeeper

[zookeeper download](https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/)，以下安装步骤以`zookeeper-3.3.6.tar.gz`为例。zookeeper安装需要JDK环境，所以提前准备[JDK环境](https://github.com/pengcgithub/java-development-environment/blob/master/jdk.md)。

## 安装

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

- 进入软件安装包存放目录：`cd /opt/setups`
- 解压：`tar zxvf zookeeper-3.3.6.tar.gz`
- 移动解压文件：`mv /opt/setups/zookeeper-3.3.6 /usr/local/zookeeper`
- 新建文件夹：`mkdir -p /usr/local/zookeeper/data`
- 重命名zoo_sample.cfg配置文件：`mv /usr/local/zookeeper/zookeeper-3.3.6/conf/zoo_sample.cfg zoo.cfg`
- 修改zoo.cnf配置文件：
	- 原值：`dataDir=/tmp/zookeeper`
	- 改为：`dataDir=/usr/program/zookeeper/data`
- 防火墙开放2181端口，如果需要更改端口，则需要修改`zoo.cnf`中的`clientPort`的value
	- `iptables -A INPUT -p tcp -m tcp --dport 2181 -j ACCEPT`
	- `service iptables save`
	- `service iptables restart`
- 启动 zookeeper：`sh /usr/local/zookeeper/zookeeper-3.3.6/bin/zkServer.sh start`
- 停止 zookeeper：`sh /usr/local/zookeeper/zookeeper-3.3.6/bin/zkServer.sh stop`
- 查看 zookeeper 状态：`sh /usr/local/zookeeper/zookeeper-3.3.6/bin/zkServer.sh status`

## 服务启动

- 配置环境变量： `vim /etc/profile`
```
export ZOOKEEPER_HOME=/usr/local/zookeeper/zookeeper-3.3.6
export PATH=$ZOOKEEPER_HOME/bin:$PATH
export PATH
```

- 刷新profile文件：`source /etc/profile`
- 启动：`zkServer.sh start`
- 查看状态：`zkServer.sh status`
- 关闭：`zkServer.sh stop`
- 重启：`zkServer.sh restart`

## 问题

- 关闭zookeeper服务报`could not find file /usr/local/zookeeper/data   /zookeeper_server.pid`错误，并且服务也没法关闭。

<pre>
JMX enabled by default
Using config: /usr/local/zookeeper/zookeeper-3.3.6/bin/../conf/zoo.cfg
Stopping zookeeper ... no zookeeper to stop (could not find file /usr/local/zookeeper/data   /zookeeper_server.pid)
</pre>

由于`zoo.cnf`中存在多余的空格导致的，建议删除**行与行**以及**头部尾部**的空格。

## Author
- [pengcheng3211@163.com](https://github.com/pengcgithub)


