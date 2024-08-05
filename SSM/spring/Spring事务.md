# Spring之事务
### 背景
#### Spring本身并没有事务的实现，依赖各个orm实现类，进而实现事务，例如事务的挂起，org.springframework.transaction.support.AbstractPlatformTransactionManager#doSuspend，其实现方式有jdbc、hibernate5、amqp.rabbit.mq ... 本质上，Spring的事务是对数据库事务对支持。
### 纯JDBC操作数据库 
```
    1. 获取连接 Connection conn = DriverManager.getConnection();
    2. 开启事务 conn.setAutoCommit(true/false);
    3. 执行业务 CRUD
    4. 提交/回滚事务 conn.commit() / conn.rollback();
    5. 关闭连接 conn.close();
```
### 有了Spring事务之后
