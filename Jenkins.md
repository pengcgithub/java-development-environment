# Jenkins 安装和配置

## 简介

- 官网：[http://jenkins-ci.org/](http://jenkins-ci.org/)
- 官网帮助中心：[https://wiki.jenkins-ci.org/display/JENKINS/Use+Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Use+Jenkins)
- 官网使用 Tomcat 部署方式指导：[https://wiki.jenkins-ci.org/display/JENKINS/Tomcat](https://wiki.jenkins-ci.org/display/JENKINS/Tomcat)

## 安装步骤

- Jenkins下载：`wget http://mirrors.jenkins-ci.org/war/latest/jenkins.war`
- 启动Jenkins：`java -jar jenkins.war --httpPort=8080`
- 浏览器访问：`localhost:8080`
- Getting Started(此处的password，可以去`/data/work/jenkins`目录下获取)
![](https://upload-images.jianshu.io/upload_images/75842-ddccc7b8ceeb89ea.png)
- 安装推荐的插件
![](https://upload-images.jianshu.io/upload_images/75842-4c6bc66ebc5e080a.png)
- 进入jenkins首页
![](https://upload-images.jianshu.io/upload_images/75842-c6bf76a30da49a13.png)

## 推荐安装方式

[docker.jenkins](./docker/docker.jenkins.md)

## 部署war至tomcat

### 项目简介

A项目，用于maven构建完成。通过a-parent项目管理下面的子项目，如下：a-common、a-service、a-web、a-admin，a-web和a-admin之间没有依赖关系，但是这两个项目均和a-common以及a-service有依赖关系

### 插件

- GitLab
- Gitlab Authentication
- Gitlab Merge Request Builder
- Generic Webhook Trigger 实现自动触发构建功能
- Git Parameter 实现多分支打包

### 构建项目

##### General

![](https://i.imgur.com/LCdxt6F.png)

##### 源码管理

![](https://i.imgur.com/ldQkKHf.png)

- Repository URL：gitlab 源码地址
- Credentials：添加凭证，只需要gitlab的账号密码就行（username,password）
- Branch Specifier (blank for 'any')：分支

##### 构建触发器

##### 构建环境

##### Pre Steps

##### Build

![](https://i.imgur.com/SQ67r0w.png)

- Root POM：build的pom文件
- Goals and options：maven脚本

##### Post Steps

##### 构建设置

##### 构建后操作

![](https://i.imgur.com/bNEUk8d.png)

- WAR/EAR files：mvn之后的war地址
- Context path：上下文名称
- Containers：配置容器，添加tomcat的地址，以及tomcat中user.xml中配置的用户名、密码。

## 参考

- [https://blog.csdn.net/wenyingzhi/article/details/80883832](https://blog.csdn.net/wenyingzhi/article/details/80883832)
- [https://blog.csdn.net/u012076316/article/details/52056107](https://blog.csdn.net/u012076316/article/details/52056107)