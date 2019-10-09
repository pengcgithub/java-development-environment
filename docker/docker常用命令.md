# docker常用命令

- `docker images`：显示本地所有的镜像地址
- `docker ps`：列举出当前正在运行的容器
- `docker search`
- `dcoker pull`
- `docker container prune`：清理所有处于中止状态的容器
- `docker inspect name`：查看docker容器的配置信息

### 运行

`docker run -it -d --name [容器名称] -v /var/lib/docker/volumes/redis:/data redis:4-alpine`
/var/lib/docker/volumes/redis表示本地存储路径，/data表示容器挂载路径

### 删除

- `docker stop $(docker ps -aq)`：停止所有的container（容器）
- `docker rm $(docker ps -aq)`：删除所有的container（容器）
- `docker rmi <image id>`：删除images（镜像），通过image的id来指定删除
- `docker rmi $(docker images -q)`：删除全部image（镜像）
- `docker rmi -f ${docker images -q}`：强制删除全部image（镜像）
- `docker container prune`：删除所有停止的容器
- `docker image prune -f -a`或则`docker image prune --force --all`：删除所有不使用的镜像

### 版本说明

docker image
docker containers

[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/centos/7/extras/x86_64/)

docker run --name kvstore -d redis:4-alpine
docker exce -it kvstore /bin/sh

netstat -tnl

### 镜像管理基础

![](https://i.imgur.com/TGfcxFV.png)

docker registry

- 启动容器时，docker registry会试图从本地获取相关的镜像；本地不存在时，其将从registry中下载镜像并保存在本地；

![](https://i.imgur.com/dREfKof.png)

docker tag [image_id/repostitory] taget_image[:tag]

docker image rm [repostitory]

docker login -u pcwww

docker exec -it ac6eb263bbe5 /bin/sh

docker run --name zookeeper01 -d zookeeper