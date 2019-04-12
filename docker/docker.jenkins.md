# Jenkins

<pre>
mkdir /home/ronit/jenkins_home
chown 1000:1000 /home/ronit/jenkins_home

docker run --privileged --name jenkins-1 -p 8080:8080 -p 50000:50000 -v /home/ronit/jenkins_home:/var/jenkins_home jenkins
</pre>

## 参考资料
- [https://www.jianshu.com/p/3671eb8de971](https://www.jianshu.com/p/3671eb8de971)
- [https://blog.csdn.net/boling_cavalry/article/details/78942408](https://blog.csdn.net/boling_cavalry/article/details/78942408)