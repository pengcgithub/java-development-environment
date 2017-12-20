# 配置JDK8

## 参数准备

- [JDK8 download](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

## 安装步骤

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

> 检查JDK命令： 

- `java -version`
- 显示以下信息则表示有JDK
<pre>
java version "1.8.0_144"
Java(TM) SE Runtime Environment (build 1.8.0_144-b01)
Java HotSpot(TM) Server VM (build 25.144-b01, mixed mode)
</pre>


> 解压安装包：

- `tar -zxvr jdk-8u72-linux-x64.tar.gz`

> 移动解压包到`/usr/local`目录下：

- `mv jdk1.8.0_72 /usr/local`

> 配置环境变量：

- 编辑配置文件：`vim /etc/profile`
- 在文件末尾添加如下内容：
    
<pre>
# jdk
JAVA_HOME=/usr/local/jdk1.8.0_72
JRE_HOME=$JAVA_HOME/jre
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME
export JRE_HOME
export PATH
export CLASSPATH
</pre>

- 执行命令，刷新该配置（必备操作）：`source /etc/profile`
- 检查是否使用了最新的 JDK：java -version

## 问题

- jdk安装完毕之后，使用`java -version`出现 ` bash: /usr/local/jdk1.8.0_25/bin/java: /lib/ld-linux.so.2: bad ELF interpreter: No such file or directory` 错误。

解决：安装glibc即可，`yum install glibc.i686`

## Author
- [pengcheng3211@gmail.com](https://github.com/pengcgithub)
	
	