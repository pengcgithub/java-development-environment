# docker.tomcat

docker环境构建tomcat镜像，通过直接pull或则手动构建两种方式。利用构建完成的镜像，完成war的部署发布。

## pull官方images

- 查找镜像：`docker search tomcat`
- 拉取镜像：`docker pull tomcat:8.5-jre8`，如果不指定tag，那么会拉取最新的镜像。
- 查看镜像：`docker images`

## 手动制作tomcat镜像

- 基础文件准备
	- jdk1.8
	- tomcat8
- 编写Dockerfile
<pre>
FROM centos:7.2.1511
MAINTAINER pengc <pengcheng3211@163.com>
# 添加jdk和tomcat
ADD jdk1.8 /home/JDK/jdk8
ADD tomcat8 /home/tomcat/tomcat8
# 环境变量
ENV JAVA_HOME=/home/JDK/jdk8 \
    PATH=$JAVA_HOME/bin:$PATH
# 公开端口
EXPOSE 8080
# 设置启动命令
CMD ["/home/tomcat/tomcat8/bin/catalina.sh", "run"]
</pre>
- 将基础文件和Dockerfile文件放在同一个文件夹下，执行编译操作：`docker build -t tomcat:v1.0 .`
	- --tag, -t: 镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签。

## 运行tomcat

- 启动tomcat：`docker run -d -p 8080:8080 --name tomcat tomcat:v1.0`
- 查看运行中的容器：`docker ps`
- 进入docker容器：`docker exec -it ae8e426a0fb1 /bin/bash`

## war部署

### war挂载启动

- war文件挂载到tomcat：`docker run -d -p 8080:8080 -v /data/work/tomcat/wapei-admin.war:/home/tomcat/tomcat8/webapps/wapei-admin.war --name wapei-admin tomcat:v1.0`

### war编译为images

- 编译项目镜像
<pre>
FROM tomcat:8.5-jre8
MAINTAINER pengc <pengcheng3211@163.com>
# 添加war到tomcat目录
ADD wapei-admin.war /usr/local/tomcat/webapps/wapei-admin.war
# 公开端口
EXPOSE 8080
# 设置启动命令
CMD ["catalina.sh", "run"]
</pre>

- 启动images：`docker run -d -p 8080:8080 --name wapei-admin tomcat:v1.0`