# dubbo

## 环境
- JDK 1.8
- Tomcat 8
- dubbo-admin-2.0.0

## 编译dubbo-admin

- 克隆`incubator-dubbo-ops`项目：`git clone https://github.com/apache/incubator-dubbo-ops.git`
- 进入项目：`cd incubator-dubbo-ops`
- maven打包：`mvn package`
- 编译打包文件
	- dubbo-admin-2.0.0.war
	- `dubbo-monitor-simple-2.0.0-assembly.tar.gz`：Unzip it you will find the shell scripts for starting or stopping monitor.
	- `dubbo-registry-simple-2.0.0-assembly.tar.gz`：Unzip it you will find the shell scripts for starting or stopping registry.

## 安装
- 获取`dubbo-admin-2.0.0.war`架包直接部署到Tomcat容器
- 修改zookeeper地址以及默认用户的登陆密码：`vim /usr/local/tomcat-dubbo/webapps/dubbo-admin/WEB-INF/dubbo.properties`
<pre>
dubbo.registry.address=zookeeper://127.0.0.1:2181
dubbo.admin.root.password=root
dubbo.admin.guest.password=guest
</pre>
- zookeeper集群
<pre>
dubbo.registry.address=zookeeper://192.168.1.121:2181?backup=192.168.1.111:2181,192.168.1.112:2181
dubbo.admin.root.password=root
dubbo.admin.guest.password=guest
</pre>

# 问题

- Dubbo-Admin 需要修改部分代码，让它支持 JDK 8，具体看文章：[https://github.com/alibaba/dubbo/issues/50](https://github.com/alibaba/dubbo/issues/50)

## Author
- [pengcheng3211@gmail.com](https://github.com/pengcgithub)