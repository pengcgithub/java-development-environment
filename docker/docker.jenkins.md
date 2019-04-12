# Jenkins

## 安装&运行Jenkins镜像
- 搜索jenkins镜像:`docker search jenkins`
	- 选择合适的镜像，我这边选择的是官方镜像。
- 下载Jenkins镜像：`docker pull jenkins`
- 创建本地的Jenkins映射目录：`mkdir /data/work/jenkins`
- 赋予该目录读写权限：`chown 1000:1000 /data/work/jenkins`
- 运行jenkins镜像，同时将文件输入到本地目录：`docker run -p 8080:8080 -p 50000:50000 -v /data/work/jenkins:/var/jenkins_home jenkins`
	- `-p 8080:8080` 映射端口8080
	- `-v /data/work/jenkins:/var/jenkins_home` 数据卷映射，冒号前面为本地路径，冒号后面为容器的Jenkins路径
- 查看是否运行：`docker ps`

## 初始化jenkins
- 访问jenkins：`localhost:8080`
- Getting Started(此处的password，可以去`/data/work/jenkins`目录下获取)
![](https://upload-images.jianshu.io/upload_images/75842-ddccc7b8ceeb89ea.png)
- 安装推荐的插件
![](https://upload-images.jianshu.io/upload_images/75842-4c6bc66ebc5e080a.png)
- 进入jenkins首页
![](https://upload-images.jianshu.io/upload_images/75842-c6bf76a30da49a13.png)

## 参考资料
- [https://www.jianshu.com/p/3671eb8de971](https://www.jianshu.com/p/3671eb8de971)
- [https://blog.csdn.net/boling_cavalry/article/details/78942408](https://blog.csdn.net/boling_cavalry/article/details/78942408)