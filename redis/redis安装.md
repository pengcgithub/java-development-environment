# Redis

我个人习惯 /opt 目录下创建一个目录 setups 用来存放各种软件安装包；在 /usr 目录下创建一个 local 用来存放各种解压后的软件包，下面的讲解也都是基于此习惯；

## Redis 安装

- 安装依赖包 ： `yum install -y gcc-c++ tcl`

- Redis 下载 ： `wget http://download.redis.io/releases/redis-3.0.7.tar.gz`

- 解压 ： `tar zxvf redis-3.0.7.tar.gz`

- 移动到我个人安装目录 ： `mv redis-3.0.7 /usr/local`

- 进入redis目录 ： `cd /usr/local/redis-3.0.7`

- 编译 ： `make`

- 编译安装 ： `make install`

默认安装路径 /usr/local/bin/ 也可自定义安装路径（make PREFIX=/usr/local/redis install）

- 复制配置文件 ： `cp /usr/local/redis-3.0.7/redis.conf /usr/local/redis/etc/`

- 删除压缩文件 ： `rm -rf /usr/local/redis-3.0.7`

- 修改配置 ： `vim /usr/local/redis/etc/redis.conf`

把旧值 daemonize no，改为新值 daemonize yes

- 启动 ： `/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf`

- 关闭 ： `redis-cli -h 127.0.0.1 -p 6379 shutdown` or `pkill redis-server` or `kill 进程ID`

- 查看是否启动 ： `ps -ef | grep redis`

- 查看端口 ： `netstat -tunpl | grep 6379`

- 连接 ： `redis-cli -h localhost -p 6379 `陪亲戚看病


