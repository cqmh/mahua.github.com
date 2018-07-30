---
layout: post
title: Docker-Oracle数据库搭建
category: 技术
tags: docker oracle
keywords: docker oracle
---
参考：[https://github.com/oracle/docker-images/tree/master/OracleDatabase](https://github.com/oracle/docker-images/tree/master/OracleDatabase)
##下载oracle镜像
```
docker pull oracle/database:12.1.0.2-ee
```

##自建oracle基础镜像
编写Dockerfile，主要引入上海时区及设置工作目录
```
FROM oracle/database:12.1.0.2-ee

USER root
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime

USER oracle
WORKDIR /home/oracle
```
通过Dockerfile构建
```
docker build -t oracle/base:12 .
```
##启动容器
```
docker run -d --restart always --privileged --name orcl
  -p 1521:1521
  -e ORACLE_SID=orcl
  -e ORACLE_PDB=pdborcl
  -e ORACLE_PWD=password
  -e ORACLE_CHARACTERSET=ZHS16GBK
  -v /root/oracle/data:/opt/oracle/oradata
  oracle/base:12
```
##其它
在数据库服务器上的oracle/network/admin/sqlnet.ora文件添加一行SQLNET.ALLOWED_LOGON_VERSION=8，重启数据库，重新连接数据库，可以成功连接