---
layout: post
title: 微服务-蓝图测试环境搭建 
category: 技术
tags: 微服务 蓝图 vertx
keywords: vertx 微服务 蓝图
---

运行mysql容器
```
docker run -d --name mysql --restart always -p 3306:3306 -e MYSQL_ROOT_PASSWORD=666666 -e MYSQL_DATABASE=vertx -e MYSQL_USER=mark -e MYSQL_PASSWORD=123456 mysql:latest
```

运行mongo容器
```
docker run -d --name mongo --restart always -p 27017:27017 mongo
```

运行redis容器
```
docker run -d --name redis --restart always -p 6379:6379 redis
```

运行elasticsearch容器
```
docker run -d --name elasticsearch --restart always -p 9200:9200 -p 9300:9300 -e ES_JAVA_OPTS="-Xms1g -Xmx1g" elasticsearch elasticsearch
```

运行logstash容器
```
docker run -d --name logstash --restart always -p 12201:12201/udp --link elasticsearch:elasticsearch logstash -e 'input { gelf { host => "0.0.0.0" port => 12201 } } output { elasticsearch { hosts => ["elasticsearch"] } }'
```

运行kibana容器
```
docker run -d --name kibana --restart always -p 5601:5601 -e ELASTICSEARCH_URL=http://elasticsearch:9200 --link elasticsearch:elasticsearch --link logstash:logstash kibana
```

运行cache容器
```
docker run -d --name cache --restart always --privileged -v /root/cqzk-msvc/image:/opt/verticles --link redis:redis --link logstash:logstash cqzk/cache
```

运行account容器
```
docker run -d --name account --restart always -p 8081:8081 --privileged -v /root/cqzk-msvc/image:/opt/verticles --link mysql:mysql --link logstash:logstash cqzk/account
```

运行gateway容器
```
docker run -d --name gateway --restart always -p 8787:8787 --privileged -v /root/cqzk-msvc/image:/opt/verticles -e JAVA_OPTS="-Xms128m -Xmx256m" --link redis:redis --link logstash:logstash cqzk/gateway
```