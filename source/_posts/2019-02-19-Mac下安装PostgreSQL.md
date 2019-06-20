---
title: Mac 下安装PostgreSQL
layout: post
date: 2019-02-19
---


# Mac首次安装postgresql

1 参考这篇博文 [Mac安装PostgreSQL](https://www.jianshu.com/p/10ced5145d39)

1）这里使用homebrew安装

```
   brew install postgresql 
```

2）然后初始化db

```
  initdb /usr/local/var/postgres
```

如果有旧版本的代码不兼容，报错
> 2019-02-19 14:36:27.575 CST [94795] FATAL:  database files are incompatible with server
2019-02-19 14:36:27.575 CST [94795] DETAIL:  The data directory was initialized by PostgreSQL version 9.6, which is not compatible with this version 11.1.

解决办法参考 [stackoverflow](https://stackoverflow.com/questions/17822974/postgres-fatal-database-files-are-incompatible-with-server)

```
  rm -rf /usr/local/var/postgres && initdb /usr/local/var/postgres -E utf8  
```

3）启动服务：

```
  pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
```

设置开机启动

```
 to-do
```

4) 创建数据库

mac安装postgresql后不会创建用户名数据库，执行命令：
之后需要做以下几件事：


1 创建postgres用户

```
 CREATE USER postgres WITH PASSWORD 'password';
```


2 删除默认生成的postgres数据库

```
DROP DATABASE postgres;
```


3 创建属于postgres用户的postgres数据库

```
 CREATE DATABASE postgres OWNER postgres;
```


4 将数据库所有权限赋予postgres用户

 ```
 GRANT ALL PRIVILEGES ON DATABASE postgres to postgres;
```


5 给postgres用户添加创建数据库的属性

 ```
 ALTER ROLE postgres CREATEDB;
```


这样就可以使用postgres作为数据库的登录用户了，并可以使用该用户管理数据
