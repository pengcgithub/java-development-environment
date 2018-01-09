# docker

## 安装

- Docker CE 版本

- Docker安装

	- sudo yum install -y yum-utils
	- sudo yum-config-manager add-repo https://download.docker.com/linux/centos/docker-ce.repo
	- sudo yum makecache fast
	- sudo yum install -y docker-ce

- 启动 Docker：`systemctl start docker.service`

- 停止 Docker：`systemctl stop docker.service`

- 查看状态：`systemctl status docker.service`

## Docker基本命令

> 本地镜像管理

- `docker images`：显示本地所有的镜像地址

> 容器操作

- `docker ps`：列举出当前正在运行的容器