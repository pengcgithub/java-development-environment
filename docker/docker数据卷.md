# 数据卷

一个 data volume 是容器中绕过 Union 文件系统的一个特定的目录。它被设计用来保存数据，而不管容器的生命周期。因此，当你删除一个容器时，Docker 肯定不会自动地删除一个volume。

#### 启动容器挂载本地地址
`docker run -it -d --name [容器名称] -v /var/lib/docker/volumes/redis:/data redis:4-alpine`
/var/lib/docker/volumes/redis表示本地存储路径，/data表示容器挂载路径

#### 进入容器

`docker exec -it [容器名] /bin/sh`：进入容器后台

#### 查询数据卷
`docker volume ls`：查询服务器下所有的数据卷
`docker inspect name`：查看docker容器的配置信息

<pre>
"Mounts": 
[
    {
        "Type": "bind",
        "Source": "/var/lib/docker/volumes/redis",
        "Destination": "/data",
        "Mode": "",
        "RW": true,
        "Propagation": "rprivate"
    }
]
</pre>

#### 删除数据卷
`docker rm -vf name`：删除容器时，删除该容器下的数据卷

#### 复制使用其他数据卷
`docker run -it -d --name [容器名称] --volumes-from [存在的容器名称] redis:4-alpine`

`docker run -it -d --name [容器名称] --network container:[存在的容器名称] --volumes-from [存在的容器名称] redis:4-alpine`
--network container:[容器名称] 表示使用容器的网络
--volumes-from [容器名称] 表示使用容器的数据卷
