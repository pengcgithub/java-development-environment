
- sudo vim /etc/gitlab-runner/config.toml

- sudo chown -R gitlab-runner:gitlab-runner /usr/local/git-tomcat/

<pre>
[root@localhost ~]# gitlab-ci-multi-runner register
Running in system-mode.                            
 ## gitlab服务器的域名或IP+/ci                                                  
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://192.168.92.128/ci
## runnerToken，gitlab服务器上获取
Please enter the gitlab-ci token for this runner:
3kjzGzK4PZDA73HYPHrP
## 描述
Please enter the gitlab-ci description for this runner:
[localhost.localdomain]: jdk8·test
##下面的tag输入之后到底有什么用？？
Please enter the gitlab-ci tags for this runner (comma separated):
js
Registering runner... succeeded                     runner=3kjzGzK4
## 执行容器，我用的shell
Please enter the executor: virtualbox, docker-ssh+machine, docker, docker-ssh, parallels, shell, ssh, docker+machine, kubernetes:
shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!

</pre>