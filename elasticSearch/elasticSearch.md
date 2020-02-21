# elasticSearch

## 简介

elasticSearch

## 单机搭建步骤

- 解压：`tar -zxvr elasticsearch-6.2.4.tar.gz elasticsearch6.2.4`
- 移到安装目录：`mv elasticsearch6.2.4 /usr/local`
- 创建es用户
	- 创建用户：`adduser es`
	- 设置密码：`passwd es`
	- 设置elasticsearch用户访问权限：`chown -R es:es /usr/local/elasticsearch6.2.4`
- 配置参数:
	- elasticsearch.yml
	
	<pre>
	#节点名称,其余两个节点分别为node-2 和node-3
	node.name: node-1
	#索引数据的存储路径
	path.data: /usr/local/elk/elasticsearch/data
	#日志文件的存储路径
	path.logs: /usr/local/elk/elasticsearch/logs
	#绑定的ip地址
	network.host: 0.0.0.0
	#设置对外服务的http端口，默认为9200
	http.port: 9200
	</pre>
	
	- jvm.options 设置启动内存，根据实际情况设置
	
	<pre>
	-Xms100m
	-Xmx100m
	</pre>
- 进入bin：`cd /usr/local/elasticsearch6.2.4/bin`  
- 启动es：`./elasticsearch`或`./elasticsearch &` 
- 验证是否成功：`curl 192.168.3.101:9200`

<pre>
{
  "name" : "node-1",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "yR8pJ5vdTnmZdwO6ctujZg",
  "version" : {
    "number" : "6.2.4",
    "build_hash" : "ccec39f",
    "build_date" : "2018-04-12T20:37:28.497551Z",
    "build_snapshot" : false,
    "lucene_version" : "7.2.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
</pre>

## 集群搭建步骤

由于elasticSearch的集群不需要zookeeper进行协调，并且数据的分片副本、负载均衡、集群扩容等复杂的分布式机制全部都隐藏，所以elasticSearch的集群是非常简单的。

- 需求：搭建三个node节点的elasticSearch集群

- 单机版基础上修改elasticsearch.yml文件，增加以下内容：

<pre>
#集群的名称
cluster.name: elasticSearchCluster
#指定该节点是否有资格被选举成为master节点，默认是true，es是默认集群中的第一台机器为master，如果这台机挂了就会重新选举master
node.master: true
#允许该节点存储数据(默认开启)
node.data: true
# 设置节点间交互的tcp端口,默认是9300 
transport.tcp.port: 9300
#Elasticsearch将绑定到可用的环回地址
discovery.zen.ping.unicast.hosts: ["192.168.3.101:9300", "192.168.3.102:9300", "192.168.3.103:9300"]
#如果没有这种设置,遭受网络故障的集群就有可能将集群分成两个独立的集群 - 分裂的大脑 - 这将导致数据丢失
discovery.zen.minimum_master_nodes: 2
</pre>

## 问题

- Error: Cannot allocate memory

参考：[Ubuntu - elasticsearch - Error: Cannot allocate memory](https://stackoverflow.com/questions/34748464/ubuntu-elasticsearch-error-cannot-allocate-memory)

由于elasticsearch默认的启动内存是2G，所以当服务器达不到上述内存就会报错。

- can not run elasticsearch as root

参考：[elasticsearch不能使用root启动问题解决](https://www.cnblogs.com/gcgc/p/10297563.html)

elasticsearch默认root用户不能启动，所以必须建立es相关用户。

- max file descriptors [65535] for elasticsearch process is too low, increase to at least [65536]

参考：[elasticsearch启动常见的几个报错](https://www.jianshu.com/p/2285f1f8ec21)

- max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

参考：[elasticsearch启动常见的几个报错](https://www.jianshu.com/p/2285f1f8ec21)

## 参考资料

- [elasticsearch启动常见的几个报错](https://www.jianshu.com/p/2285f1f8ec21)
- [elasticsearch不能使用root启动问题解决](https://www.cnblogs.com/gcgc/p/10297563.html)
- [Ubuntu - elasticsearch - Error: Cannot allocate memory](https://stackoverflow.com/questions/34748464/ubuntu-elasticsearch-error-cannot-allocate-memory)
- [Linux安装Elasticsearch](https://juejin.im/post/5bc69e2ce51d450e6548ce77)