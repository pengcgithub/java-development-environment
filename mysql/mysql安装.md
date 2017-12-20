# mysql 安装配置

## 安装步骤

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

**我们这次安装以 5.6.35 为实例**

- 进入下载目录：`cd /opt/setups`

- 解压压缩包：`tar zxvf mysql-5.6.35.tar.gz`

- 移到解压包：`mv /opt/setups/mysql-5.6.35 /usr/local/`

- 安装依赖包、编译包：`yum install -y make gcc-c++ cmake bison-devel ncurses-devel`

- 进入解压目录：`cd /usr/local/mysql-5.6.35/`

- 生成安装目录：`mkdir -p /usr/local/mysql/data`

- 生成配置（使用 InnoDB）：`cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/usr/local/mysql/data -DMYSQL_UNIX_ADDR=/tmp/mysql.sock -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS:STRING=utf8 -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DENABLED_LOCAL_INFILE=1`

- 编译：make，这个过程比较漫长，一般都在 30 分钟左右，具体还得看机子配置，如果最后结果有 error，建议删除整个 mysql 目录后重新解压一个出来继续处理

- 安装：`make install`

- 配置开机启动：
	- `cp /usr/local/mysql-5.6.35/support-files/mysql.server /etc/init.d/mysql`
	- `chmod 755 /etc/init.d/mysql`
	- `chkconfig mysql on`

<pre>
CMAKE参数说明：
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql //默认安装目录
-DINSTALL_DATADIR=/usr/local/mysql/data //数据库存放目录
-DDEFAULT_CHARSET=utf8 　　　 //使用utf8字符
-DDEFAULT_COLLATION=utf8_general_ci //校验字符
-DEXTRA_CHARSETS=all 　　//安装所有扩展字符集
-DENABLED_LOCAL_INFILE=1 　　//允许从本地导入数据
-DMYSQL_USER=mysql
-DMYSQL_TCP_PORT=3306
</pre>

- 复制一份配置文件： `cp /usr/local/mysql-5.6.35/support-files/my-default.cnf /etc/my.cnf`

- 删除安装的目录：`rm -rf /usr/local/mysql-5.6.35/`

- 添加组和用户及安装目录权限
	- groupadd mysql #添加组
	- useradd -g mysql mysql -s /bin/false #创建用户mysql并加入到mysql组，不允许mysql用户直接登录系统
	- chown -R mysql:mysql /usr/local/mysql/data #设置MySQL数据库目录权限

- 初始化数据库：`/usr/local/mysql/scripts/mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --skip-name-resolve --user=mysql`

[初始化mysql数据库提示缺少Data:dumper模块解决方法](http://blog.51cto.com/liuzhenlife/1892070)

- 开放防火墙端口：
	- `iptables -I INPUT -p tcp -m tcp --dport 3306 -j ACCEPT`
	- `service iptables save`
	- `service iptables restart`

- 禁用 selinux
	- 编辑配置文件：`vim /etc/selinux/config`
	- 把 SELINUX=enforcing 改为 SELINUX=disabled

## MySQL 配置

- 找一下当前系统中有多少个 my.cnf 文件：find / -name "my.cnf"，我查到的结果

<pre>
/etc/my.cnf
/usr/local/mysql/my.cnf
/usr/local/mysql/mysql-test/suite/ndb/my.cnf
/usr/local/mysql/mysql-test/suite/ndb_big/my.cnf
.............
/usr/local/mysql/mysql-test/suite/ndb_rpl/my.cnf
</pre>

- 保留 /etc/my.cnf 和 /usr/local/mysql/mysql-test/ 目录下配置文件，其他删除掉。

## 修改 root 账号密码

- 启动 Mysql 服务器：`service mysql start`

- 查看是否已经启动了：`ps aux | grep mysql`

- 终端下执行：/usr/local/mysql/bin/mysql -uroot

	- 现在进入了 mysql 命令行管理界面，输入：`SET PASSWORD = PASSWORD('123456');FLUSH PRIVILEGES;`

- 修改密码后，终端下执行：`mysql -uroot -p`

	- 根据提示，输入密码进入 mysql 命令行状态。

## Mycli 客户端

- 安装pip : `yum -y install python-pip`

- 更新pip : `pip install --upgrade pip`

- 安装mycli ： `pip install mycli`

- 连接mysql ： `mycli -u root -ppassword`

[http://www.my-superspace.com/thread-3427-1-1.html](http://www.my-superspace.com/thread-3427-1-1.html)

## 删除mysql

- `yum remove  mysql mysql-server mysql-libs mysql-server`
- `find / -name mysql` 将找到的相关东西delete掉
- `rpm -qa | grep mysql` (查询出来的东东yum remove掉)

[http://blog.csdn.net/hi_1234567/article/details/9176715](http://blog.csdn.net/hi_1234567/article/details/9176715)


## 问题

- Host '192.168.1.133' is not allowed to connect to this MySQL server

	- 进入MYSQL命令行：`mysql -h localhost -u root`
	- 赋予任何主机访问数据的权限：`GRANT ALL PRIVILEGES ON *.* TO  'root'@'%' WITH GRANT OPTION` 
	- 修改生效：`FLUSH PRIVILEGES`

- Access denied for user 'root'@'localhost' (using password: YES)

	- 停止MYSQL服务：`service mysql stop`
	- 终端中执行（前面添加的 Linux 用户 mysql 必须有存在）：`/usr/local/mysql/bin/mysqld --skip-grant-tables --user=mysql`
		- 此时 MySQL 服务会一直处于监听状态，你需要另起一个终端窗口来执行接下来的操作
		- 在终端中执行：mysql -u root mysql
		- 把密码改为：123456，进入 MySQL 命令后执行：UPDATE user SET Password=PASSWORD('123456') where USER='root';FLUSH PRIVILEGES;
		- 然后重启 MySQL 12/13/2017 4:46:50 PM 服务：service mysql restart

- [ERROR] Fatal error: Can't open and lock privilege tables: Table 'mysql.user' doesn't exist

	- my.cnf中的datadir地址路径与编译安装时指定的路径不一致
	- 参考文：http://blog.csdn.net/leshami/article/details/41801395

<pre>
[root@HKBO scripts]# service mysqld start
Starting MySQL..The server quit without updating PID file (/var/lib/mysql/HKBO.pid).[FAILED]
</pre>

- mysqld_safe error: log-error set to '/usr/local/mysql/log/alert.log', however file don't exists. Create writable for user 'mysql'.

	- 给路径赋予读写权限：sudo chown -R mysql:mysql mysql(路径)

## 参考资料

- [http://blog.51cto.com/liuzhenlife/1892310](http://blog.51cto.com/liuzhenlife/1892310)

- [https://blog.imdst.com/mysql-5-6-pei-zhi-you-hua/](https://blog.imdst.com/mysql-5-6-pei-zhi-you-hua/)

- [Host '192.168.1.133' is not allowed to connect to this MySQL server](https://www.cnblogs.com/xyzdw/archive/2011/08/11/2135227.html)

- [https://github.com/zhblue/crud](https://github.com/zhblue/crud)