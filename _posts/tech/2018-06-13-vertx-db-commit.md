---
layout: post
title: Vertx 数据库事务处理
category: 技术
tags: vertx 事务
keywords: vertx commit rollback 事务
---
## 传统事务
```
try {
    开启事务
    。。。
    提交事务
} catch (Exception e) {
    回滚事务
}
```
## Vertx事务1
```
jdbcClientlient.getConnection(connHandler(resultHandler, connection -> {
    String sql1 = "";
    String sql2 = "";
    Future.<Void>future(f -> connection.setAutocommit(false, f))
    .compose(f1 -> Future.<UpdateResult>future(f -> connection.update(sql1, f)))
    .compose(f2 -> Future.<UpdateResult>future(f3 -> connection.update(sql2, f3)))
    .setHandler( res -> {
        if (res.failed()) {
            connection.rollback(roll -> {
                if (roll.succeeded()) {
                    //rollback ok
                } else {
                    //rollback error
                }
                connection.close();
            });
        } else {
            connection.commit(commit -> {
                if (commit.succeeded()) {
                    //commit ok
                } else {
                    //commit error
                }
                connection.close();
            });
        }
    });
});
```
## Vertx事务2
```
jdbcClientlient.getConnection(connHandler(resultHandler, connection -> {
    String sql1 = "";
    String sql2 = "";
    List<Future> futures = new ArrayList<>();
    Future<Void> future = Future.future();
    Future<UpdateResult> future1 = Future.future();
    Future<UpdateResult> future2 = Future.future();
    connection.setAutoCommit(false, future);
    futures.add(future);
    connection.update(sql1, future1);
    futures.add(future1);
    connection.update(sql2, future2);
    futures.add(future2);

    CompositeFuture.all(futures).setHandler(res -> {
        if (res.succeeded()) {
            connection.commit(commit -> {
                if (commit.succeeded()) {
                    //commit ok
                } else {
                    //commit error
                }
                connection.close();
        } else {
            connection.rollback(roll -> {
                if (roll.succeeded()) {
                    //rollback ok
                } else {
                    //rollback error
                }
                connection.close();
            });
        }
    });
});
```
