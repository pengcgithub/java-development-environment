# mysql 权限

## 简介

`'jack'@'%'` 可以任意ip连接的用户

`'jack'@'localhost'`只允许localhost连接的用户

## 基本语法

> 基本操作

- 查看用户： select * from mysql.user;

- 删除用户： drop user 'jack'@'localhost';

- 查看当前登录用户权限： show grants;

- 查看权限： show grants for '用户名'@'localhost'

- 查看权限： show grants for 'jack'@'%';

- 移除授权： revoke delete on *.* from 'jack'@'localhost';

- 账户重命名： rename user 'jack'@'%' to 'jim'@'%';

- 修改密码

	- SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456');

	- update user set PASSWORD = PASSWORD('1234abcd') where user = 'root';

- 刷新： flush privileges;

> 创建test用户，并赋予所有用户权限

GRANT SELECT,INSERT,DELETE,UPDATE ON samp_db.* TO 'user'@'%' IDENTIFIEDBY "pass"

- 创建test账号
	- insert into mysql.user(Host,User,Password) values("localhost","test",password("123456"));
- 赋予权限
	- grant 权限 on 数据库.* to 用户名@登录主机 identified by "密码";
	- eg:grant all privileges on *.* to test@localhost identified by "123456";
	- eg:grant all privileges on *.* to test@% identified by "123456";

## 实践案例

> 创建jsyfadmin账号

- insert into mysql.user(Host,User,Password) values("localhost","jsyfadmin",password("myLoveJSYF365.com"));
- flush privileges;
- grant all privileges on *.* to jsyfadmin@localhost identified by "myLoveJSYF365.com";
- grant all privileges on *.* to 'jsyfadmin'@'%' identified by "myLoveJSYF365.com";

> 创建jsyfstore账号

- insert into mysql.user(Host,User,Password) values("localhost","jsyfstore",password("myLoveJSYF.com"));
- flush privileges;
- grant all privileges on store.* to 'jsyfstore'@'%' identified by "myLoveJSYF.com";

> 修改root密码

- SET PASSWORD FOR 'root'@'%' = PASSWORD('myLoveJSYF');
- SET PASSWORD FOR 'root'@'localhost' = PASSWORD('myLoveJSYF');
- flush privileges;

## 参考资料

[Mysql 远程登录1045失败解决办法](http://blog.csdn.net/yang382197207/article/details/18217429)
