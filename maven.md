# Maven 安装与配置

## 基础参数


## 按照步骤

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

- 下载压缩包
`wget http://mirrors.cnnic.cn/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz`

- 解压
`tar zxvf apache-maven-3.3.9-bin.tar.gz`

- 修改目录名，默认的太长了
`mv apache-maven-3.3.9/ maven3.3.9/`

- 移到我个人习惯的安装目录下
`mv maven3.3.9/ /usr/local`

- 配置环境变量 `vim /etc/profile`

文件末尾添加下列内容：
<pre>
# Maven
MAVEN_HOME=/usr/local/maven3.3.9
PATH=$PATH:$MAVEN_HOME/bin
MAVEN_OPTS="-Xms256m -Xmx356m"
export MAVEN_HOME
export PATH
export MAVEN_OPTS
</pre>

- 刷新配置文件
`source /etc/profile`

- 测试是否安装成功
`mvn -version`

## Author
- [pengcheng3211@gmail.com](https://github.com/pengcgithub)
