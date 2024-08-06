# Spring之事务
### 背景
#### 结合Spring，JDBC，MySQL，事务有三个主要的维度，{存在的形式，事务的目的，事务的实现};
### 事务形式
<table>
    <th>事务形式</th>
    <th>Spring</th>
    <th>JDBC</th>
    <th>MySQL</th>
    <tr>
        <td>物理事务</td>
        <td></td>
        <td>connection</td>
        <td>com.mysql.cj.jdbc.Driver.DriverManager.getConnection(url, username, password);</td>
    </tr>
    <tr>
        <td>逻辑事务</td>
        <td>@Transaction</td>
        <td></td>
        <td></td>
    </tr>
</table>

#### 总结：一个物理事务对应一个connection，但一个connection可以包含多个逻辑事务 @Transaction，这取决于Propagation的配置。

### 纯JDBC操作数据库 
```
    1. 获取连接 Connection conn = DriverManager.getConnection();
    2. 开启事务 conn.setAutoCommit(true/false);
    3. 执行业务 CRUD
    4. 提交/回滚事务 conn.commit() / conn.rollback();
    5. 关闭连接 conn.close();
    
    import java.sql.Connection;
    
    Connection = connection = dataSource.getConnection();
    try (connection) {
        connection.setAutoCommit(false);
        
        // execute some SQL statements...
        
        connection.commit();
    } catch (SQLException e) {
        connection.rollBack();
    }
```
#### Spring的事务，基于JDBC的事务api，对除了CRUD之外的步骤，做了封装，通过AOP的形式，为方法提供事务的实现。

### 事务实现
#### Spring本身并没有事务的实现，依赖各个orm实现类，进而实现事务，例如事务的挂起，org.springframework.transaction.support.AbstractPlatformTransactionManager#doSuspend，其实现方式有jdbc、hibernate5、amqp.rabbit.mq ... 本质上，Spring的事务是对数据库事务对支持。

### Spring事务传播行为（管理）
<table>
    <th>事务类型</th>
    <th>事务说明</th>
    <th>JDBC操作</th>
    <tr>
        <td>Propagation.REQUIRE</td>
        <td>方法需要事务，有事务则沿用，没有则新开一个</td>
        <td>getConnection().setAutoCommit(false).commit()</td>
    </tr>
    <tr>
        <td>Propagation.REQUIRE_NEW</td>
        <td>方法需要一个独属于自己的事务</td>
        <td>getConnection().setAutoCommit(false).commit()</td>
    </tr>
    <tr>
        <td>Propagation.SUPPORT</td>
        <td>该方法有事务则沿用，没有在无事务下运行</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Propagation.NOT_SUPPORT</td>
        <td>该方法一定不能在事务下运行，如果有事务，则先将事务挂起，再运行</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Propagation.MANDATORY</td>
        <td>当前方法执行时不会创建一个事务，且必须【已】存在一个事务，否则抛异常</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Propagation.NEVER</td>
        <td>当前方法执行时不会创建一个事务，且如果【已】存在一个事务，则抛异常</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Propagation.NESTED</td>
        <td>如果当前存在事务，则使用保存点技术，异常时回滚到保存点，不影响外部事务，期间共用一个connection</td>
        <td>connection.setSavePoint()</td>
    </tr>
</table>

## 参考资料💾
#### <a href="https://www.marcobehler.com/guides/spring-transaction-management-transactional-in-depth#_introduction">Spring Transaction Management: @Transactional In-Depth</a>
#### <a href="https://blog.csdn.net/aiyaya_66da/article/details/94171771">Spring Transaction REQUIRE_NEW 和 NESTED的区别</a>

