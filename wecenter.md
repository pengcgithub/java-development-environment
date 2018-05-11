# wecenter

## 简介

WeCenter 是一款建立知识社区的开源程序 专注于企业和行业知识的整理、归类、检索、分享

[官网地址](http://www.wecenter.com/)

## 安装步骤

### 安装LAMP基础环境

- 安装Apache、PHP以及PHP扩展：`yum -y install php php-mysql httpd php-gd*`
- 安装freetype：`yum -y install freetype freetype-devel`
- 安装ImageMagick性能更佳：`yum install -y ImageMagick ImageMagick-devel`
- 安装mcrypt支持：`yum install -y libmcrypt libmcrypt-devel mcrypt mhash php-mcrypt`
- 更改目录权限：`chown -R apache.apache /var/www/html/`

### 数据库配置

- [MYSQL数据库安装](https://github.com/pengcgithub/java-development-environment/blob/master/mysql/mysql%E5%AE%89%E8%A3%85.md)
- 创建wecenter数据库：`create database wecenter charset=utf8`

### wecenter安装

- wecenter部署文件下载：`http://wenda.wecenter.com/page/download`
- 上传zip部署文件至`/opt/setups`目录
- 解压：`uzip WeCenter_v3.1.9.zip`
- 进入WeCenter目录：`cd /opt/setups/WeCenter`
- 移动UPLOAD至`/var/www/html`：`mv UPLOAD /var/www/html`
- UPLOAD重命名为wecenter：`mv /var/www/html/UPLOAD wecenter`
- 启动Apache服务：`systemctl start httpd`
- 访问install页面，安装wecenter：`http://IP:80/wecenter/install/`

![](https://i.imgur.com/ebVNgvX.png)

- 配置wecenter数据库

![](https://i.imgur.com/yFZV3gn.png)

- 安装成功

![](https://i.imgur.com/y3NdJAo.png)

- 访问：`http://IP:端口/`

## 基础配置

### 配置邮件

- 进入*邮件设置*菜单：*全局设置 > 邮件设置*
- 配置邮件发送
![](https://i.imgur.com/sw1dYA4.png)
- 验证是否配置成功：进入 *工具 > 系统维护 > 邮件系统测试*
- 测试成功

![](https://i.imgur.com/1Nr2zZi.png)

**如果出现错误情况基本上都是邮件配置原因，或则是没有开启SMTP授权等**


## 问题

- 管理员进入后台管理端报错
	- [https://blog.csdn.net/reblue520/article/details/52327280](https://blog.csdn.net/reblue520/article/details/52327280)

## 参考资料

- [https://blog.csdn.net/reblue520/article/details/52327280](https://blog.csdn.net/reblue520/article/details/52327280)
- [灜沣社区](http://47.94.197.87:35080/)