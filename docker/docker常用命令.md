# docker常用命令

- `docker images`：显示本地所有的镜像地址
- `docker ps`：列举出当前正在运行的容器
- `docker search`
- `dcoker pull`
- `docker container prune`：清理所有处于中止状态的容器
- `docker inspect name`：查看docker容器的配置信息

#### 运行

`docker run -it -d --name [容器名称] -v /var/lib/docker/volumes/redis:/data redis:4-alpine`
/var/lib/docker/volumes/redis表示本地存储路径，/data表示容器挂载路径

#### 删除

`docker stop $(docker ps -aq)`：停止所有的container（容器）
`docker rm $(docker ps -aq)`：删除所有的container（容器）
`docker rmi <image id>`：删除images（镜像），通过image的id来指定删除
`docker rmi $(docker images -q)`：删除全部image（镜像）
`docker rmi -f ${docker images -q}`：强制删除全部image（镜像）
`docker container prune`：删除所有停止的容器
`docker image prune -f -a`或则`docker image prune --force --all`：删除所有不使用的镜像
