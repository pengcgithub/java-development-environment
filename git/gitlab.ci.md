# gitLab CI/CD

gitlab实现项目自动发布的功能。

## 安装gitlab-runner

- 添加gitlab-runner官方仓库：`curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash`
- 安装gitlab-runner:`sudo yum install gitlab-runner`
- 或则，指定安装版本：
	- 查找版本：`yum list gitlab-runner --showduplicates | sort -r`
	- 安装：`sudo yum install gitlab-runner-$version`

## 配置gitlab-runner

- 注册gitlab-runner:`sudo gitlab-runner register`，按照以下提示依次执行。

<pre>
[root@localhost ~]# gitlab-ci-multi-runner register
Running in system-mode.                            
 ## gitlab服务器的域名或IP+/ci                                                  
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://gitlab.wawaeg.com/

## runnerToken，gitlab服务器上获取
Please enter the gitlab-ci token for this runner:
z8ZkxyNJZQ5ZnUw2Vxxx

## 描述
Please enter the gitlab-ci description for this runner:
[localhost.localdomain]: 描述信息

##下面的tag输入之后到底有什么用？？
Please enter the gitlab-ci tags for this runner (comma separated):
wawa-dev
Registering runner... succeeded                     runner=3kjzGzK4

## 执行容器，我用的shell
Please enter the executor: virtualbox, docker-ssh+machine, docker, docker-ssh, parallels, shell, ssh, docker+machine, kubernetes:
shell

Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
</pre>

- 查看注册信息：`sudo vim /etc/gitlab-runner/config.toml`

- 设置文件夹操作权限：`sudo chown -R gitlab-runner:gitlab-runner /usr/local/git-tomcat/`

## 参考资料

[gitLab CI/CD](https://github.com/Tupurp/GitLab-Tutorial/wiki/GitLab-runner%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8EGitLab-CI%E7%9A%84%E5%88%9D%E6%AD%A5%E4%BD%BF%E7%94%A8.md)