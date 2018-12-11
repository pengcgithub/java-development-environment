# Docker安装

## 安装

- Docker CE 版本

- Docker安装

	- sudo yum install -y yum-utils
	- sudo yum-config-manager add-repo https://download.docker.com/linux/centos/docker-ce.repo
	- sudo yum makecache fast
	- sudo yum install -y docker-ce

- 启动docker：`systemctl start docker.service`

- 停止docker：`systemctl stop docker.service`

- 查看状态：`systemctl status docker.service`

## 设置镜像加速

目前可加速的镜像有 docker cn、阿里云加速器、中国科技大学三类。

- 创建配置文件：`touch /etc/docker/daemon.json`
- 修改daemon.json：
<pre>
{
"registry-mirrors"：["https://registry.docker-cn.com"]
}
</pre>

## 版本说明

docker image
docker containers

[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/centos/7/extras/x86_64/)

docker run --name kvstore -d redis:4-alpine
docker exce -it kvstore /bin/sh

netstat -tnl

## 镜像管理基础

![](https://i.imgur.com/TGfcxFV.png)

docker registry

- 启动容器时，docker registry会试图从本地获取相关的镜像；本地不存在时，其将从registry中下载镜像并保存在本地；

![](https://i.imgur.com/dREfKof.png)

docker tag [image_id/repostitory] taget_image[:tag]

docker image rm [repostitory]

docker login -u pcwww

