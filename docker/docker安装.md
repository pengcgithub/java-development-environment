# Docker安装

## 安装

- Docker CE 版本

- Docker安装

	- 安装一些必要的系统工具：`sudo yum install -y yum-utils device-mapper-persistent-data lvm2`
	- 添加软件源信息：`sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`
	- 更新yum缓存：`sudo yum makecache fast`
	- 安装 Docker-CE：`sudo yum install -y docker-ce`

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

## 卸载docker

<pre>
yum remove docker \
  docker-client \
  docker-client-latest \
  docker-common \
  docker-latest \
  docker-latest-logrotate \
  docker-logrotate \
  docker-selinux \
  docker-engine-selinux \
  docker-engine

rm -rf /etc/systemd/system/docker.service.d

rm -rf /var/lib/docker

rm -rf /var/run/docker
</pre>

<pre>
# 查找docker文件
yum list installed | grep docker

yum -y remove docker-engine.x86_64
</pre>

## 问题

- docker安装添加软件源，报错 `File "/bin/yum-config-manager", line 133 except yum.Errors.RepoError, e:`
	- [https://blog.csdn.net/zhoudan232/article/details/79819394](https://blog.csdn.net/zhoudan232/article/details/79819394)

## 参考资料

- [http://www.runoob.com/docker/centos-docker-install.html](http://www.runoob.com/docker/centos-docker-install.html)
- [https://blog.csdn.net/qq_36421955/article/details/87802942](https://blog.csdn.net/qq_36421955/article/details/87802942)
- [https://www.cnblogs.com/wonder4/p/9267509.html](https://www.cnblogs.com/wonder4/p/9267509.html)