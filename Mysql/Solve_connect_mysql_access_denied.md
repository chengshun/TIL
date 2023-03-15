# 远程连接Mysql，并解决`Access denied`问题

## 远程连接命令
`mysql -u [username] -p [password] -h [host=127.0.0.1]`

使用docker的项目中需要将host替换为docker-compose中定义的mysql service name

## 创建Mysql新用户

`CREATE USER 'username'@'host' IDENTIFIED BY 'password';`

host指定用户在哪个主机上可以登录，`localhost`表示本机，`%`表示远程

## 给新用户授权
`GRANT all privileges on `databasename`.* to '`newUsername`'@'%';`

All表示所有权限（Select、Insert、Update）

`flush privileges;`

刷新权限

## 授权过程的报错

`ERROR 1410 (42000): You are not allowed to create a user with GRANT`

解决办法：需要修改mysql.user表里用户的host为’%‘

`update mysql.user set host='%' where user='`newUsername`';`

## 总结
mysql远程连接很简单，日常中经常会遇到，但是一时想不起来怎么处理，在此记录一下。

