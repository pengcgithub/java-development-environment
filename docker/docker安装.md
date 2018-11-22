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