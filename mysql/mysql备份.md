# mysql 备份与恢复

## 简介

- 清空错误日志： `echo > /usr/local/mysql/log/error.log`

## binlog增量备份

- mysqlbinlog --stop-position=593 mysql-bin.000001 > /opt/593.sql
- mysqlbinlog --start-position=772 mysql-bin.000001 > /opt/772.sql

[http://www.sohu.com/a/134793543_463994](http://www.sohu.com/a/134793543_463994)


## XtraBackup备份

### 优势

- 无需停止数据库进行InnoDB热备
- 增量备份MySQL
- 流压缩到传输到其它服务器
- 能比较容易地创建主从同步
- 备份MySQL时不会增大服务器负载

### 安装

### 使用

- 备份： `innobackupex --host=106.15.92.216 --user=root --password=123456 /data/backup`

### 问题错误

<pre>
error: Failed dependencies:
        perl(DBD::mysql) is needed by percona-xtrabackup-2.2.11-1.el5.x86_64
        perl(Digest::MD5) is needed by percona-xtrabackup-2.2.11-1.el5.x86_64
        rsync is needed by percona-xtrabackup-2.2.11-1.el5.x86_64
</pre>
- yum -y install perl perl-devel libaio libaio-devel perl-Time-HiRes perl-DBD-MySQL
- yum -y install rsync perl l perl-Digest-MD5

<pre>
error: Failed dependencies:
	libev.so.4()(64bit) is needed by percona-xtrabackup-24-2.4.3-1.el7.x86_64
</pre>
- wget  ftp://rpmfind.net/linux/dag/redhat/el6/en/x86_64/dag/RPMS/libev-4.15-1.el6.rf.x86_64.rpm
- rpm -ivh libev-4.15-1.el6.rf.x86_64.rpm

## 参考资料
- [http://blog.csdn.net/wxc20062006/article/details/71629507](http://blog.csdn.net/wxc20062006/article/details/71629507)

- [http://www.cnblogs.com/liushen/p/5715546.html](http://www.cnblogs.com/liushen/p/5715546.html)

