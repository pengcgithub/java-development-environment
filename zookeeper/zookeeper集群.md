# zookeeper集群

zookeeper集群在zookeeper单机安装版本的基础之上，如果不清楚如何安装单机版本，可参考 [zookeeper安装](./zookeeper.md)。很多种情况下，我们没有很多机器可以实现多机器集群的效果，但是我们依旧可以通过单机集群的方式实现相同的效果，并且他们配置的方式也相同。

## 单机集群

与[zookeeper安装](./zookeeper.md)中的路径不同，为了方便安装单机集群，重新定义一套路径为了方便环境的安装。
- zookeeper安装路径：`/usr/local/zookeeper/`，该路径下会存放多个zookeeper节点，例如`/usr/local/zookeeper/node1`。
- data和dataLog：`/usr/local/zookeeper/node1/data`和`/usr/local/zookeeper/node1/dataLog`

此教程默认安装3个集群节点，分别为node1、node2、node3，这三个节点可以按照[zookeeper安装](./zookeeper.md)示例搭建完成，但是需要保持上面的路径规则即可。搭建完成之后这三个节点均可以单独启动，并且相互之后没有影响，接下来就需要将这三个节点关联起来。

- 首先 需要在三个节点的`/usr/local/zookeeper/node1/data`目录内都创建一个名为myid的文件，并且编辑`myid`的内容为`1`（备注：内容数字根据节点来，如果为node1填写`1`，依次node2填写`2`）。
- 然后 修改每个zookeeper节点中的`zoo.cfg`文件，将以下内容设置为一致即可。
<pre>
dataDir=/usr/local/zookeeper/node1/data
dataLogDir=/usr/local/zookeeper/node1/dataLog
server.1=hserver1:3211:3212
server.2=hserver2:3221:3222
server.3=hserver3:3231:3232
# server.4=192.168.0.1:3241:3242
</pre>

- 最后 启动zookeeper节点即可。（**节点启动顺序不分先后，只需要启动成功即可。**）

`/usr/local/zookeeper/node1/bin/zkServer.sh start`

- 查看状态

`/usr/local/zookeeper/node1/bin/zkServer.sh status`

通过查询状态可以看出，其中有一个为`leader`，其余是`follower`。

![leader](https://i.imgur.com/mWlx6bs.png)

![follower](https://i.imgur.com/EFNHQeI.png)


## 多机集群

多机集群的配置和单机的配置几乎一致，唯一不同就是将原先多个zookeeper节点，分别放置到不同机器上。

## author

- [[pengcheng3211@163.com](https://github.com/pengcgithub)