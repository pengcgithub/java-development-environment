# activemq

## 安装步骤

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

- 下载[activemq](http://activemq.apache.org/activemq-5152-release.html)并上传至`/opt/setups`目录
- 解压文件：`tar -zxvf apache-activemq-5.15.2-bin.tar.gz`
- 移动解压文件至`/usr/local/`目录：`mv ./apache-activemq-5.15.2 /usr/local/`
- 修改命名，方便维护：`mv apache-activemq-5.15.2 activemq-5.15.2`
- 启动activemq：`/usr/local/activemq-5.15.2/bin/activemq start`
- 设置开机启动：`chkconfig activemq on`
	- 启动：`service activemq start`
	- 停止：`service activemq stop`
	- 查看状态：`service activemq status`
- 设置防火墙：`vim /etc/sysconfig/iptables`
	- 8161（web管理页面端口）：`-A INPUT -m state --state NEW -m tcp -p tcp --dport 8161 -j ACCEPT`
	- 61616（activemq服务监控端口）：`-A INPUT -m state --state NEW -m tcp -p tcp --dport 61616 -j ACCEPT`

- 访问：`localhost:8161`

## 配置

*TODO*


## 参考资料

[https://blog.csdn.net/baidu_35536997/article/details/77849372](https://blog.csdn.net/baidu_35536997/article/details/77849372)

## Author
- [pengcheng3211@gmail.com](https://github.com/pengcgithub)