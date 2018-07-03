#  firewall 切换至 iptables

## firewall 基本使用

- 启动：`systemctl start firewalld`
![](https://i.imgur.com/cH4gX6Y.png)

- 停止：`systemctl disable firewalld`
![](https://i.imgur.com/HUz6Srz.png)

- 禁用：`systemctl stop firewalld`
- 查看状态：`systemctl status firewalld`
- 查看所有打开的端口： `firewall-cmd --zone=dmz --list-ports`
- 增加端口： `firewall-cmd --zone=dmz --add-port=8080/tcp`

## 切换为iptables防火墙

切换到iptables首先应该关掉默认的firewalld，然后安装iptables服务。

- 关闭firewall

`service firewalld stop`

`systemctl disable firewalld.service`  #禁止firewall开机启动

- 安装iptables防火墙

`yum install iptables-services`

- 编辑iptables防火墙配置

`vim /etc/sysconfig/iptables`

例如开放一个80端口，需要在iptables文件中添加:
`-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT`

- 开启iptables防火墙

`service iptables start`

- 查看iptable防火墙状态

`service iptables status`

- 设置iptables防火墙开机启动或者关闭

`systemctl enable iptables.service`

`systemctl disable iptables.service`


## 参考资料

[Centos 7防火墙firewalld开放80端口](https://my.oschina.net/sokes/blog/826705)

[ centOS7中关闭firewall，并使用iptables管理防火墙](http://blog.csdn.net/misakaqunianxiatian/article/details/52344415)

[CentOS 7中firewall防火墙详解和配置以及切换为iptables防火墙](http://blog.csdn.net/xlgen157387/article/details/52672988)

## Author
- [pengcheng3211@gmail.com](https://github.com/pengcgithub)