# sql server

## pull镜像

[https://hub.docker.com/_/microsoft-mssql-server](https://hub.docker.com/_/microsoft-mssql-server)

本文使用的是`2017-latest`版本的官方镜像

## 步骤

- 拉取镜像：`docker pull mcr.microsoft.com/mssql/server:2017-latest`
- run 镜像：
```
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MSSQL@12345"  -p 1433:1433 --name mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
- 查看images：`docker ps -a`
- 启动sqlServer：`docker start mssql`
- 进入sqlServer
	- 进入bash界面：`docker exec -it mssql "bash"`
	- 进入数据库：` /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P'MSSQL@12345'`
- 关闭sqlServer：`docker stop mssql`