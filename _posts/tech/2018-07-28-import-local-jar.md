---
layout: post
title: 本地jar包 
category: 技术
tags: maven jar
keywords: maven jar 本地
---

maven工程使用本地jar包

maven工程添加本地依赖
```
    <dependency>
      <groupId>cn.com.cqzk</groupId>
      <artifactId>aspose-cells</artifactId>
      <version>18.3</version>
      <scope>system</scope>
      <systemPath>${pom.basedir}/lib/aspose-cells-18.3.jar</systemPath>
    </dependency>
```

将本地jar包安装到本地repository中
```
mvn install:install-file -Dfile=ojdbc14-1.0.jar -DgroupId=ojdbc14 -DartifactId=ojdbc14 -Dversion=1.0 -Dpackaging=jar
```

修改依赖
```
    <dependency>
      <groupId>cn.com.cqzk</groupId>
      <artifactId>aspose-cells</artifactId>
      <version>18.3</version>
    </dependency>
```
