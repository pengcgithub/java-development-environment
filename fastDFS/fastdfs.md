# FastDFS

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

## 安装

- 软件准备
	- FastDFS_v5.05.tar.gz
	- fastdfs-nginx-module_v1.16.tar.gz
	- libfastcommon-1.0.7.tar.gz
	- nginx-1.12.2.tar.gz

### 安装libfastcommon
*由于libfastcommon上传为zip包，所以需要先安装 `yum install -y unzip zip`，如果上传的为.tar.gz则不需要安装。*

- 创建安装目录：`mkdir -p /usr/local/fast/`
- 解压：`unzip libfastcommon-master.zip -d /usr/local/fast/ `
- 移动解压文件：`mv /opt/setups/libfastcommon-master /usr/local/fast`
- 进入目录：`cd /usr/local/fast/libfastcommon-master`
- 编译：`make`
- 安装：`make install`
- 设置几个软链接：由于libfastcommon默认安装在`/usr/lib64/`路径下，又因FastDFS主程序设置的目录在`/usr/local/lib/`目录下，所以需要创建`/usr/lib64/`下的一些核心执行程序的软连接文件
	- `ln -s /usr/lib64/libfastcommon.so /usr/local/lib/libfastcommon.so`
	- `ln -s /usr/lib64/libfastcommon.so /usr/lib/libfastcommon.so`
	- `ln -s /usr/lib64/libfdfsclient.so /usr/local/lib/libfdfsclient.so`
	- `ln -s /usr/lib64/libfdfsclient.so /usr/lib/libfdfsclient.so`

### 安装FastDFS_v5.08

#### 1、安装

- 解压：`tar zxvf FastDFS_v5.05.tar.gz`
- 移动解压文件：`mv /opt/setups/FastDFS_v5.05 /usr/local/fast`
- 进入FastDFS：`cd /usr/local/fast/FastDFS_v5.05`
- 编译：`./make.sh`
- 安装：`./make.sh install`
- 安装结果
<pre>
/usr/bin 存放有编译出来的文件
/etc/fdfs 存放有配置文件
</pre>
- 默认安装的方式安装,安装后的相应文件与目录

<pre>
# 脚本服务
/etc/init.d/fdfs_storaged
/etc/init.d/fdfs_tracker

# 配置文件
/etc/fdfs/client.conf.sample
/etc/fdfs/storage.conf.sample
/etc/fdfs/tracker.conf.sample

# 命令工具在/usr/bin/目录下的文件：ls | grep fdfs
fdfs_appender_test
fdfs_appender_test1
fdfs_append_file
fdfs_crc32
fdfs_delete_file
fdfs_download_file
fdfs_file_info
fdfs_monitor
fdfs_storaged
fdfs_test
fdfs_test1
fdfs_trackerd
fdfs_upload_appender
fdfs_upload_file
stop.sh
restart.sh
</pre>

- 修改 FastDFS 服务脚本：因为 FastDFS 服务脚本设置的 bin 目录是`/usr/local/bin`，但实际命令安装在`/usr/bin`，因此需要修改 FastDFS 服务脚本中相应的命令路径，也就是把`/etc/init.d/fdfs_storaged`和`/etc/init.d/fdfs_tracker` 两个脚本中的`/usr/local/bin` 修改成`/usr/bin`

<pre>
# vim fdfs_trackerd
使用查找替换命令进统一修改:%s+/usr/local/bin+/usr/bin
# vim fdfs_storaged
使用查找替换命令进统一修改:%s+/usr/local/bin+/usr/bin
</pre>


#### 2、配置 tracker 服务

- 复制一份配置文件：`cp /etc/fdfs/tracker.conf.sample /etc/fdfs/tracker.conf`
- 编辑tracker.conf：`vim /etc/fdfs/tracker.conf`
<pre>
disabled=false
bind_addr=
port=22122
connect_timeout=30
network_timeout=60
# 下面这个路径是保存 store data 和 log 的地方，需要我们改下，指向我们一个存在的目录
# 创建目录：mkdir -p /fastdfs/tracker/data-and-log
base_path=/fastdfs/tracker
max_connections=256
accept_threads=1
work_threads=4
store_lookup=2
store_group=group2
store_server=0
store_path=0
download_server=0
reserved_storage_space = 10%
log_level=info
run_by_group=
run_by_user=
allow_hosts=*
sync_log_buff_interval = 10
check_active_interval = 120
thread_stack_size = 64KB
storage_ip_changed_auto_adjust = true
storage_sync_file_max_delay = 86400
storage_sync_file_max_time = 300
use_trunk_file = false 
slot_min_size = 256
slot_max_size = 16MB
trunk_file_size = 64MB
trunk_create_file_advance = false
trunk_create_file_time_base = 02:00
trunk_create_file_interval = 86400
trunk_create_file_space_threshold = 20G
trunk_init_check_occupying = false
trunk_init_reload_from_binlog = false
trunk_compress_binlog_min_interval = 0
use_storage_id = false
storage_ids_filename = storage_ids.conf
id_type_in_filename = ip
store_slave_file_use_link = false
rotate_error_log = false
error_log_rotate_time=00:00
rotate_error_log_size = 0
log_file_keep_days = 0
use_connection_pool = false
connection_pool_max_idle_time = 3600
http.server_port=8080
http.check_alive_interval=30
http.check_alive_type=tcp
http.check_alive_uri=/status.html
</pre>

- 启动 tracker 服务：
	- `/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf`
	- `/etc/init.d/fdfs_trackerd start`
- 停止 tracker 服务：
	- `/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf stop` 
	- `/etc/init.d/fdfs_trackerd stop`
- 重启 tracker 服务：
	- `/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf restart`
	- `/etc/init.d/fdfs_trackerd restart`

#### 3、storage （存储节点）服务部署

*如果 storage 单独安装的话，那上面安装的步骤都要在走一遍，只是到了编辑配置文件的时候，编辑的是 storage.conf 而已*

- 复制一份配置文件：cp /etc/fdfs/storage.conf.sample /etc/fdfs/storage.conf
- 编辑：vim /etc/fdfs/storage.conf

<pre>
disabled=false
group_name=group1
bind_addr=
client_bind=true
port=23000
connect_timeout=30
network_timeout=60
heart_beat_interval=30
stat_report_interval=60
# 下面这个路径是保存 store data 和 log 的地方，需要我们改下，指向我们一个存在的目录
# 创建目录：mkdir -p /fastdfs/storage
base_path=/fastdfs/storage
max_connections=256
buff_size = 256KB
accept_threads=1
work_threads=4
disk_rw_separated = true
disk_reader_threads = 1
disk_writer_threads = 1
sync_wait_msec=50
sync_interval=0
sync_start_time=00:00
sync_end_time=23:59
write_mark_file_freq=500
store_path_count=1
# 图片实际存放路径，如果有多个，这里可以有多行：
# store_path0=/opt/fastdfs/storage/images-data0
# store_path1=/opt/fastdfs/storage/images-data1
# store_path2=/opt/fastdfs/storage/images-data2
# 创建目录：mkdir -p /opt/fastdfs/storage/images-data
store_path0=/fastdfs/storage
subdir_count_per_path=256
# 指定 tracker 服务器的 IP 和端口
tracker_server=106.15.183.40:22122
log_level=info
run_by_group=
run_by_user=
allow_hosts=*
file_distribute_path_mode=0
file_distribute_rotate_count=100
fsync_after_written_bytes=0
sync_log_buff_interval=10
sync_binlog_buff_interval=10
sync_stat_file_interval=300
thread_stack_size=512KB
upload_priority=10
if_alias_prefix=
check_file_duplicate=0
file_signature_method=hash
key_namespace=FastDFS
keep_alive=0
use_access_log = false
rotate_access_log = false
access_log_rotate_time=00:00
rotate_error_log = false
error_log_rotate_time=00:00
rotate_access_log_size = 0
rotate_error_log_size = 0
log_file_keep_days = 0
file_sync_skip_invalid_record=false
use_connection_pool = false
connection_pool_max_idle_time = 3600
http.domain_name=
http.server_port=8888
</pre>

- 启动 storage 服务（首次启动会很慢，因为它在创建预设存储文件的目录）：
	- `/usr/bin/fdfs_storaged /etc/fdfs/storage.conf`
	- `/etc/init.d/fdfs_storaged start`

- 停止 storage 服务：
	- `/usr/bin/fdfs_storaged /etc/fdfs/storage.conf stop`
	- `/etc/init.d/fdfs_storaged stop`

- 重启 storage 服务：
	- `/usr/bin/fdfs_storaged /etc/fdfs/storage.conf restart`
	- `/etc/init.d/fdfs_storaged restart`
 

#### 4、测试是否部署成功

*利用自带的 client 进行测试*

- 复制一份配置文件：`cp /etc/fdfs/client.conf.sample /etc/fdfs/client.conf`
- 编辑：`vim /etc/fdfs/client.conf`
<pre>
connect_timeout=30
network_timeout=60
# 下面这个路径是保存 store log 的地方，需要我们改下，指向我们一个存在的目录
# 创建目录：mkdir -p /fastdfs/client
base_path=/fastdfs/client
# 指定 tracker 服务器的 IP 和端口
tracker_server=106.15.183.40:22122
log_level=info
use_connection_pool = false
connection_pool_max_idle_time = 3600
load_fdfs_parameters_from_tracker=false
use_storage_id = false
storage_ids_filename = storage_ids.conf
http.tracker_server_port=80
</pre>

- 执行测试命令：`/usr/bin/fdfs_test /etc/fdfs/client.conf upload /opt/test.jpg`
- 返回ID号：`group1/M00/00/00/rBPbMlq3V0KALRgmADcMnleurQk583.png` 
- 访问：`http://106.15.183.40:8888/group1/M00/00/00/rBPbMlq3V0KALRgmADcMnleurQk583.png`，无法展示图片，因为我们还没装 FastDFS 的 Nginx 模块。