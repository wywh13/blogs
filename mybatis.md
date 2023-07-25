# mbatis具体执行流程：

<img src="https://img2020.cnblogs.com/blog/848880/202007/848880-20200704003555956-24153981.png" alt="img" style="zoom:50%;" />



# mybatis的几大组件：

### 2.1 SqlSessionFactoryBuilder

　　从名称可以看出来使用的建造者设计模式（Builder），用于构建SqlSessionFactory对象

　　1.解析mybatis的xml配置文件，然后创建Configuration对象（对应<configuration>标签）；

　　2.根据创建的Configuration对象，创建SqlSessionFactory（默认使用DefaultSqlSessionFactory）；

 

### 2.2 SqlSessionFactory

　　从名称上可以看出使用的工厂模式（Factory），用于创建并初始化SqlSession对象（数据库会话）

　　1.调用openSession方法，创建SqlSession对象，可以将SqlSession理解为数据库连接（会话）；

　　2.openSession方法有多个重载，可以创建SqlSession关闭自动提交、指定ExecutorType、指定数据库事务隔离级别….

```java
package org.apache.ibatis.session;
import java.sql.Connection;
 
public interface SqlSessionFactory {
    /**
     * 使用默认配置
     * 1.默认不开启自动提交
     * 2.执行器Executor默认使用SIMPLE
     * 3.使用数据库默认的事务隔离级别
     */
    SqlSession openSession();
 
    /**
     * 指定是否开启自动提交
     * @param autoCommit 是否开启自动提交
     */
    SqlSession openSession(boolean autoCommit);
 
    /**
     * 根据已有的数据库连接创建会话（事务）
     * @param connection 数据库连接
     */
    SqlSession openSession(Connection connection);
 
    /**
     * 创建连接时，指定数据库事务隔离级别
     * @param level 事务隔离界别
     */
    SqlSession openSession(TransactionIsolationLevel level);
 
    /**
     * 创建连接时，指定执行器类型
     * @param execType 执行器
     */
    SqlSession openSession(ExecutorType execType);
 
    SqlSession openSession(ExecutorType execType, boolean autoCommit);
    SqlSession openSession(ExecutorType execType, TransactionIsolationLevel level);
    SqlSession openSession(ExecutorType execType, Connection connection);
 
    /**
     * 获取Configuration对象，也就是解析xml配置文件中的<configuration>标签后的数据
     */
    Configuration getConfiguration();
}
```

　　

### 2.3 SqlSession

　　如果了解web开发，就应该知道cookie和session吧，SqlSession的session和web开发中的session概念类似。

　　session，译为“会话、会议”，数据的有效时间范围是在会话期间（会议期间），会话（会议）结束后，数据就清除了。

　　也可以将SqlSession理解为一个数据库连接（但也不完全正确，因为创建SqlSession之后，如果不执行sql，那么这个连接是无意义的，所以数据库连接在执行sql的时候才建立的）。

　　SqlSession是一个接口，定义了很多操作数据库的方法声明：

```java
public interface SqlSession extends Closeable {
    /* 获取数据库连接 */
    Connection getConnection();
 
    /* 数据库的增删改查 */
    <T> T selectOne(String statement);
    <E> List<E> selectList(String statement);
    int update(String statement, Object parameter);
    int delete(String statement, Object parameter);
 
    /* 事务回滚与提交 */
    void rollback();
    void commit();
 
    /* 清除一级缓存 */
    void clearCache();
 
    // 此处省略了很多方法
}
```

　　SqlSession只是定义了执行sql的一些方法，而具体的实现由子类来完成，比如SqlSession有一个接口实现类DefaultSqlSession。

　　MyBatis中通过Executor来执行sql的，在创建SqlSession的时候（openSession），同时会创建一个Executor对象，如下：

```java

@Override
public SqlSession openSession() {
    return openSessionFromDataSource(configuration.getDefaultExecutorType(), null, false);
}
 
private SqlSession openSessionFromDataSource(ExecutorType execType, TransactionIsolationLevel level, boolean autoCommit) {
    Transaction tx = null;
    try {
        final Environment environment = configuration.getEnvironment();
        final TransactionFactory transactionFactory = getTransactionFactoryFromEnvironment(environment);
        tx = transactionFactory.newTransaction(environment.getDataSource(), level, autoCommit);
 
        // 利用传入的参数，创建executor对象
        final Executor executor = configuration.newExecutor(tx, execType);
        return new DefaultSqlSession(configuration, executor, autoCommit);
    } catch (Exception e) {
        closeTransaction(tx); // may have fetched a connection so lets call close()
        throw ExceptionFactory.wrapException("Error opening session.  Cause: " + e, e);
    } finally {
        ErrorContext.instance().reset();
    }
}
```

 

### 2.4 Executor

　　Executor（人称“执行器”）是一个接口，定义了对JDBC的封装；

　　MyBatis中提供了多种执行器，如下：

　　![img](https://img2020.cnblogs.com/blog/848880/202007/848880-20200703214658494-1409345354.png)

　　上面的图中，虽然列出了5个Executor（BaseExecutor是抽象类），其实Executor只有三种：

```java
public enum ExecutorType {
    SIMPLE, // 简单
    REUSE,  // 复用
    BATCH;  // 批量
}
```

　　CacheExecutor其实是一个Executor代理类，包含一个delegate，需要创建时手动传入（要入simple、reuse、batch三者之一）；

　　ClosedExecutor，所有接口都会抛出异常，表示一个已经关闭的Executor；

　　创建SqlSession时，默认使用的是SimpleExecutor；

　　下面是创建Executor的代码：org.apache.ibatis.session.Configuration#newExecutor()

　　![img](https://img2020.cnblogs.com/blog/848880/202007/848880-20200703215856609-1524620608.png)

　　上面说了，Executor是对JDBC的封装。当我们使用JDBC来执行sql时，一般会先预处理sql，也就是conn.prepareStatement(sql)，获取返回的PreparedStatement对象（实现了Statement接口），再调用statement的executeXxx()来执行sql。

　　也就是说，Executor在执行sql的时候也是需要创建Statement对象的，下面以SimpleExecutor为例：

　　![img](https://img2020.cnblogs.com/blog/848880/202007/848880-20200703222012330-1772441820.png)

 

### 2.5 StatementHandler

　　在JDBC中，是调用Statement.executeXxx()来执行sql；

　　在MyBatis，也是调用Statement.executeXxx()来执行sql，此时就不得不提StatementHandler，可以将其理解为一个工人，他的工作包括

　　1.对sql进行预处理;

　　2.调用statement.executeXxx()执行sql；

　　3.将数据库返回的结果集进行对象转换（ORM）；

```java
public interface StatementHandler {
    /**
     * 获取预处理对象
     */
    Statement prepare(Connection connection, Integer transactionTimeout) throws SQLException;
 
    /**
     * 进行预处理
     */
    void parameterize(Statement statement) throws SQLException;
 
    /**
     * 批量sql（内部调用statement.addBatch）
     */
    void batch(Statement statement) throws SQLException;
 
    /**
     * 执行更新操作
     * @return 修改的记录数
     */
    int update(Statement statement) throws SQLException;
 
    /**
     * 执行查询操作
     * @param statement     sql生成的statement
     * @param resultHandler 结果集的处理逻辑
     * @return 查询结果
     */
    <E> List<E> query(Statement statement, ResultHandler resultHandler) throws SQLException;
     
    <E> Cursor<E> queryCursor(Statement statement) throws SQLException;
    BoundSql getBoundSql();
    ParameterHandler getParameterHandler();
}
```

　　StatementHandler的相关子类如下图所示：

　　![img](https://img2020.cnblogs.com/blog/848880/202007/848880-20200703223645348-1550489349.png)

 

　　以BaseStatementHandler为例：

　　![img](https://img2020.cnblogs.com/blog/848880/202007/848880-20200703224532989-148866422.png)

 

 

### 2.6 ParameterHandler

　　ParameterHandler的功能就是sql预处理后，进行设置参数：

```java
public interface ParameterHandler {
 
    Object getParameterObject();
 
    void setParameters(PreparedStatement ps) throws SQLException;
}
```

　　ParamterHandler有一个DefaultParameterHandler，下面是其重写setParameters的代码：

　　![img](https://img2020.cnblogs.com/blog/848880/202007/848880-20200703231208648-819980821.png)

  

### 2.7 ResultSetHandler

　　当执行statement.execute()后，就可以通过statement.getResultSet()来获取结果集，获取到结果集之后，MyBatis会使用ResultSetHandler来将结果集的数据转换为Java对象（ORM映射）。

```java
public interface ResultSetHandler {
    /**
     * 从statement中获取结果集，并将结果集的数据库属性字段映射到Java对象属性
     * @param stmt 已经execute的statement，调用state.getResultSet()获取结果集
     * @return 转换后的数据
     */
    <E> List<E> handleResultSets(Statement stmt) throws SQLException;
     
    <E> Cursor<E> handleCursorResultSets(Statement stmt) throws SQLException;
    void handleOutputParameters(CallableStatement cs) throws SQLException;
}
```

　　ResultSetHandler有一个实现类，DefaultResultHandler，其重写handlerResultSets方法，如下：

　　![img](https://img2020.cnblogs.com/blog/848880/202007/848880-20200703230122851-1699809333.png) 

###  2.8 TypeHandler

　　TypeHandler主要用在两个地方：

　　1.参数绑定，发生在ParameterHandler.setParamenters()中。

　　MyBatis中，可以使用<resultMap>来定义结果的映射关系，包括每一个字段的类型，比如下面这样：

```java
<resultMap id="baseMap" type="cn.ganlixin.model.User">
    <id column="uid" property="id" jdbcType="INTEGER" />
    <result column="uname" property="name" jdbcType="VARCHAR" />
</resultMap>
```

　　TypeHandler，可以对某个字段按照xml中配置的类型进行设置值，比如设置sql的uid参数时，类型为INTEGER（jdbcType）。

　　2.获取结果集中的字段值，发生在ResultSetHandler处理结果集的过程中。

　　TypeHandler的定义如下：

```java
public interface TypeHandler<T> {
 
    /**
     * 设置预处理参数
     *
     * @param ps        预处理statement
     * @param i         参数位置
     * @param parameter 参数值
     * @param jdbcType  参数的jdbc类型
     */
    void setParameter(PreparedStatement ps, int i, T parameter, JdbcType jdbcType) throws SQLException;
 
    /**
     * 获取一条结果集的某个字段值
     *
     * @param rs         一条结果集
     * @param columnName 列名（字段名称）
     * @return 字段值
     */
    T getResult(ResultSet rs, String columnName) throws SQLException;
 
    /**
     * 获取一条结果集的某个字段值（按照字段的顺序获取）
     *
     * @param rs          一条结果集
     * @param columnIndex 字段列的顺序
     * @return 字段值
     */
    T getResult(ResultSet rs, int columnIndex) throws SQLException;
 
    T getResult(CallableStatement cs, int columnIndex) throws SQLException;
}
```

参考地址：https://www.cnblogs.com/-beyond/p/13232624.html