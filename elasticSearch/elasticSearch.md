# elasticSearch

## 简介

elasticSearch

## 步骤

- 解压：`tar -zxvr elasticsearch-6.2.4.tar.gz elasticsearch6.2.4`
- 移到安装目录：`mv elasticsearch6.2.4 /usr/local`
- 创建es用户
	- 创建用户：`adduser es`
	- 设置密码：`passwd es`
	- 设置elasticsearch用户访问权限：`chown -R es:es /usr/local/elasticsearch6.2.4`
- 配置参数:
	- elasticsearch.yml
	
	<pre>
	node.name: node-1
	cluster.name: my-application
	network.host: 0.0.0.0
	http.port: 8080
	</pre>
	
	- jvm.options 设置启动内存
	
	<pre>
	-Xms100m
	-Xmx100m
	</pre>
- 进入bin：`cd /usr/local/elasticsearch6.2.4/bin`  
- 启动es：`./elasticsearch`或`./elasticsearch &` 
- 验证是否成功：`curl 47.98.188.53:8080`

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