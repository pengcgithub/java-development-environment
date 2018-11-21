# command

- 检查端口被哪个进程占用：`netstat -lnp | grep 88`
- 查看进程的详细信息：`ps 进程号`
- 列出所有正在使用的端口以及关联进程：`netstat -nap`或则`netstat -ntlp`
- 显示所有运行中的内存：`ps aux | less`
	- 查看非root运行的进程：`ps -U root -u root -N`
	- 查看用户vivek运行的进程：`ps -u vivek`

## Author
- [pengcheng3211@gmail.com](https://github.com/pengcgithub)