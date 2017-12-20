# Nexus 安装与配置

[http://www.sonatype.org/nexus/](http://www.sonatype.org/nexus/)

## 安装步骤

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

- 解压压缩包
`tar -zxvf nexus-2.11.1-01-bundle.tar.gz`

解压出两个文件，程序目录 `nexus-2.11.1`和 仓库目录 `sonatype-work`

> **配置程序目录**

- 移动程序目录到/usr/local

`mv nexus-2.11.1 /usr/local`

- 修改程序目录的名称为nexus2.11.1

`mv nexus-2.11.1/ nexus2.11.1`

- 编辑系统配置文件

`vim /etc/profile`

文件末尾追加下列信息

<pre>
# Nexus
NEXUS_HOME=/usr/program/nexus2.11.4
export NEXUS_HOME
RUN_AS_USER=root
export RUN_AS_USER
</pre>

- 刷新配置
`source /etc/profile`

> **配置仓库目录**

由于目录 `sonatype-work` 以后是做仓库用的，会存储很多 `jar`，所以这个目录一定要放在磁盘空间大的区内，个人习惯直接存放在`/opt`目录下。

- 将文件移动到`/opt`目录

`mv /opt/setup/maven/sonatype-work/ /opt/`

- 设置配置文件

`vim /usr/local/nexus2.11.1/conf/nexus.properties`

将文件中的`nexus-work=${bundleBasedir}/../sonatype-work/nexus`，修改为`nexus-work=/opt/sonatype-work/nexus`

- 开放iptables端口

编辑 `vim /etc/sysconfig/iptables` 文件，将文件中开放`8081`端口。

添加规则 ： `-A INPUT -p tcp -m state --state NEW -m tcp --dport 8081 -j ACCEPT`

重启 iptables ： `service iptables restart`

> **测试Nexus**

- 启动nexus

`/usr/local/nexus2.11.1/bin/nexus start`

- 查看启动日志

`tail -200f /usr/local/nexus2.11.1/logs/wrapper.log`

- 关闭 Nexus

`/usr/local/nexus2.11.1/bin/nexus stop`

- 访问

`http://ip:8081/nexus`

## Author
- [pengcheng3211@gmail.com](https://github.com/pengcgithub)