# RocketMQ安装

## 单机安装

- 获取安装包
	- 下载[rocketMQ Binary](http://rocketmq.apache.org/dowloading/releases/)文件
- 移动至`/opt/setups`目录：`mv ~/rocketmq-all-4.4.0-bin-release.zip /opt/setups`
- 解压：`unzip -o rocketmq-all-4.4.0-bin-release.zip`
- 移动至`/usr/local`目录，并修改名称：`mv /opt/setups/rocketmq-all-4.4.0-bin-release /usr/local/rocketmq-4.4.0`
- 进入rocketMQ目录：`cd /usr/local/rocketmq-4.4.0`
- 新建日志目录：`mkdir -p ~/logs/rocketmqlogs`
- 配置JVM内存空间：`vim /usr/local/rocketmq-4.4.0/bin/runserver.sh和runbroker.sh`
	- 修改为 》》 JAVA_OPT="${JAVA_OPT} -server -Xms512m -Xmx512m -Xmn256m -XX:PermSize=128m -XX:MaxPermSize=320m"
- 启动namespace服务：`nohup sh bin/mqnamesrv > ~/logs/rocketmqlogs/namesrv.log 2>&1 &`
- 查看namespace启动日志：`tail -200f ~/logs/rocketmqlogs/namesrv.log`
	- The Name Server boot success. serializeType=JSON，输入以上语句表明启动成功。

- 新建broker.properties，指定访问IP：`echo "brokerIP1=外网IP" > broker.properties`
- 启动broker服务：`nohup sh bin/mqbroker -n localhost:8080 -c ./broker.properties > ~/logs/rocketmqlogs/broker.log 2>&1 &`
	- boot success. serializeType=JSON and name server is localhost:8080，输入以上语句表明启动成功。
- 停止namespace服务：`sh bin/mqshutdown namesrv`
- 停止broker服务：`sh bin/mqshutdown broker`

## 集群安装

<pre>
#所属集群名字
brokerClusterName=rocketmq-cluster
#broker名字，注意此处不同的配置文件填写的不一样
brokerName=broker-a|broker-b
#0 表示 Master，>0 表示 Slave
brokerId=0
#nameServer地址，分号分割
namesrvAddr=192.168.140.128:9876;192.168.140.129:9876
#在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
defaultTopicQueueNums=4
#是否允许 Broker 自动创建Topic，建议线下开启，线上关闭
autoCreateTopicEnable=true
#是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
autoCreateSubscriptionGroup=true
#Broker 对外服务的监听端口
listenPort=10911
#删除文件时间点，默认凌晨 4点
deleteWhen=04
#文件保留时间，默认 48 小时
fileReservedTime=120
#commitLog每个文件的大小默认1G
mapedFileSizeCommitLog=1073741824
#ConsumeQueue每个文件默认存30W条，根据业务情况调整
mapedFileSizeConsumeQueue=300000
#destroyMapedFileIntervalForcibly=120000
#redeleteHangedFileInterval=120000
#检测物理文件磁盘空间
diskMaxUsedSpaceRatio=88
#存储路径
storePathRootDir=/usr/local/rocketmq/store
#commitLog 存储路径
storePathCommitLog=/usr/local/rocketmq/store/commitlog
#消费队列存储路径存储路径
storePathConsumeQueue=/usr/local/rocketmq/store/consumequeue
#消息索引存储路径
storePathIndex=/usr/local/rocketmq/store/index
#checkpoint 文件存储路径
storeCheckpoint=/usr/local/rocketmq/store/checkpoint
#abort 文件存储路径
abortFile=/usr/local/rocketmq/store/abort
#限制的消息大小
maxMessageSize=65536
#flushCommitLogLeastPages=4
#flushConsumeQueueLeastPages=2
#flushCommitLogThoroughInterval=10000
#flushConsumeQueueThoroughInterval=60000
#Broker 的角色
#- ASYNC_MASTER 异步复制Master
#- SYNC_MASTER 同步双写Master
#- SLAVE
brokerRole=ASYNC_MASTER
#刷盘方式
#- ASYNC_FLUSH 异步刷盘
#- SYNC_FLUSH 同步刷盘
flushDiskType=ASYNC_FLUSH
#checkTransactionMessageEnable=false
#发消息线程池数量
#sendMessageThreadPoolNums=128
#拉消息线程池数量
#pullMessageThreadPoolNums=128
</pre>

## RocketMQ Console

## 参考资料

- [rocketMq服务的安装、启动与关闭](https://blog.csdn.net/www_wangzheguilai/article/details/76483705)
- [](https://blog.csdn.net/c_yang13/article/details/76836753)
- [RocketMQ整合Springboot](https://www.jianshu.com/p/c6fd4ab90006)
- [](https://blog.csdn.net/gwd1154978352/article/details/80642850)