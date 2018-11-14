# Nginx

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

**我们这次安装以 1.12.2 为实例**


## Nginx 源码编译安装

- 源码包方式下载：http://nginx.org/en/download.html ，注意该页面的：**Stable version**，这个表示稳定版本

- 安装
	- 安装依赖包：`yum install -y gcc gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel`
	- 预设nginx文件夹：`mkdir -p /usr/local/nginx /var/log/nginx /var/temp/nginx /var/lock/nginx`
	- 进入源码包存放目录： `/opt/setups/nginx`
	- 解压：`tar zxvf nginx-1.12.2.tar.gz`
	- 进入解压后目录：cd nginx-1.12.2/
	- 编译配置
	<pre>
	./configure \
	--prefix=/usr/local/nginx \
	--pid-path=/var/local/nginx/nginx.pid \
	--lock-path=/var/lock/nginx/nginx.lock \
	--error-log-path=/var/log/nginx/error.log \
	--http-log-path=/var/log/nginx/access.log \
	--with-http_gzip_static_module \
	--http-client-body-temp-path=/var/temp/nginx/client \
	--http-proxy-temp-path=/var/temp/nginx/proxy \
	--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
	--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
	--http-scgi-temp-path=/var/temp/nginx/scgi
	</pre>
	- 编译：`make`
	- 安装：`make install`
	- 安装成功效果：`cd /usr/local/nginx; ll`
	<pre>
	drwxr-xr-x. 2 root root 4096 3月  22 16:21 conf
	drwxr-xr-x. 2 root root 4096 3月  22 16:21 html
	drwxr-xr-x. 2 root root 4096 3月  22 16:21 sbin
	</pre>

- 启动 Nginx

	- 启动：`/usr/local/nginx/sbin/nginx`
	- 停止：`/usr/local/nginx/sbin/nginx -s stop`
	- 刷新：`/usr/local/nginx/sbin/nginx -s reload`
	- 检查是否启动成功：
		- 访问：服务器地址（192.168.1.114），如果能看到：Welcome to nginx!，即可表示安装成功
		- 检查 Nginx 进程：`ps aux | grep nginx`
	<pre>
	root      5401     1  0 20:20 ?        00:00:00 nginx: master process /usr/local/nginx/sbin/nginx
	nobody    5402  5401  0 20:20 ?        00:00:00 nginx: worker process
	root      5404  4524  0 20:20 pts/3    00:00:00 grep --color=auto nginx
	</pre>

- 把 Nginx 添加到系统服务中
	- 新建文件：vim /etc/init.d/nginx
	- 添加如下内容：


<pre>
	#!/bin/bash

	#nginx执行程序路径需要修改
	nginxd=/usr/local/nginx/sbin/nginx
	
	# nginx配置文件路径需要修改
	nginx_config=/usr/local/nginx/conf/nginx.conf
	
	# pid 地址需要修改
	nginx_pid=/var/local/nginx/nginx.pid
	
	
	RETVAL=0
	prog="nginx"
	
	# Source function library.
	. /etc/rc.d/init.d/functions
	# Source networking configuration.
	. /etc/sysconfig/network
	# Check that networking is up.
	[ ${NETWORKING} = "no" ] && exit 0
	[ -x $nginxd ] || exit 0
	
	# Start nginx daemons functions.
	start() {
	if [ -e $nginx_pid ];then
	   echo "nginx already running...."
	   exit 1
	fi
	
	echo -n $"Starting $prog: "
	daemon $nginxd -c ${nginx_config}
	RETVAL=$?
	echo
	[ $RETVAL = 0 ] && touch /var/lock/subsys/nginx
	return $RETVAL
	}
	
	# Stop nginx daemons functions.
	# pid 地址需要修改
	stop() {
		echo -n $"Stopping $prog: "
		killproc $nginxd
		RETVAL=$?
		echo
		[ $RETVAL = 0 ] && rm -f /var/lock/subsys/nginx /var/local/nginx/nginx.pid
	}
	
	# reload nginx service functions.
	reload() {
		echo -n $"Reloading $prog: "
		#kill -HUP `cat ${nginx_pid}`
		killproc $nginxd -HUP
		RETVAL=$?
		echo
	}
	
	# See how we were called.
	case "$1" in
		start)
			start
			;;
		stop)
			stop
			;;
		reload)
			reload
			;;
		restart)
			stop
			start
			;;
		status)
			status $prog
			RETVAL=$?
			;;
		*)
	
		echo $"Usage: $prog {start|stop|restart|reload|status|help}"
		exit 1
	
	esac
	exit $RETVAL
</pre>

- 修改权限：`chmod 755 /etc/init.d/nginx`
- 启动服务：`service nginx start`
- 停止服务：`service nginx stop`
- 重启服务：`service nginx restart`

## 参考资料

- [根据原版整理nginx安装配置，推荐原版](https://github.com/pengcgithub/Linux-Tutorial/blob/master/Nginx-Install-And-Settings.md)