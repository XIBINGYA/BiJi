# JDBC简介

JDBC 概率:

	* JDBC就是使用java语言操作关系型数据库的一套API
	* 全程:(Java DataBase Connectivity) Java 数据库连接

![image-20220605151149404](.\JDBC.assets\image-20220605151149404.png)

JDBC API 详解

* DriverManager(驱动管理类)
* Connection(数据库连接对象)
* Statement(执行SQL语句)
* ResultSet(结果集对象)
* PreparedStatement(预编译SQL语句执行)



## DriverManager(驱动管理类)

* 1.注册驱动(MySql 5之后的驱动包，可以省略注册驱动的步骤)
* 2.获取数据连接(getConnection(String url, String user, String password))
  * 参数
    * url:连接路径
      * 语法：jdbc:mysql://ip地址(域名):端口号/数据库名称?参数键值对1&参数键值对2...
      * 示例：jdbc:mysql://127.0.0.1:3306/db1
      * 细节：
        * 如果连接的是本机ysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:/W数据库名称？参数键值对
        * 配置useSSL=false参数，禁用安全连接方式，解决警告提示
    * user：用户名
    * password：密码

## Connection(数据库连接对象)

* 作用:

  * 1.管理执行SQL的对象

    * 普通执行SQL对象

    * ```
      Statement createStatement();
      ```

    * 预编译SQL的执行SQL对象：防止SQL注入(主要使用这个)

    * ```
      PreparedStatement prepareStatement(sql);
      ```

    * 执行存储过程的对象

    * ```
      CallableStatement prepareCall(sql);
      ```

      

  * 2.管理事务（将SQL语句集中管理）

    * MySQL 事务管理

    * ```
      
      开启事务：BEGIN;/START TRANSACTION;
      提交事务：COMMIT;
      回滚事务：ROLLBACK;
      
      MySQL默认自动提交事务
      
      ```

    * JDBC 事务管理: Connection接口中定义了3个对应的方法

    * ```
      
      开启事务：setAutoCommit(boolean autoCommit): true为自动提交事务;false为手动提交事
      务,即为开启事务
      提交事务：commit()
      回滚事务：rollback();
      
      ```

      

## Statement(执行SQL语句)

作用：

​	1.执行SQL语句

```
int executeUpdate(sql):执行DML，DDL语句
返回值:(1)DML语句影响的行数
	  (2)DDL语句执行后，执行成功也可能返回0	
```

```
ResultSet executeQuery(sql):执行DQL语句
返回值:ResultSet结果集对象
```



## ResultSet(结果集对象)

作用:

 * 1.封装了DQL查询语句的结果

 * ```
   ResultSet stmt.executeQuery(sql):执行DQL语句,返回ResultSet对象
   ```

 * 获取查询结果

 * ```
   boolean next():(1)将光标从当前位置向前移动一行 (2)判断当前行是否为有效行
   返回值:
   	true:有效行，当前行有数据
   	false:无效行，当前行没有数据
   ```

 * ```
   xxx getXxx(参数):获取数据
   
   Xxx:数据类型；如：int getint（参数）；String getString（参数）
   参数：
   	int:列的编号，从1开始
   	String:列的名称
   ```

 * ![image-20220605155654702](.\JDBC.assets\image-20220605155654702.png)

## PreparedStatement(预编译SQL语句执行)

作用:

​	预防SQL注入问题

SQL 注入

​	SQL注入是通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行攻击的方法。

实现步骤:

* 1.获取PreparedStatement对象

* ```java
  // SQL 语句中的参数值，使用？占位符替代
  String sql = "select * from user where username=? and password=?"
  
  // 通过Connection对象获取,并传入对应的sql语句
  PreparedStatement pstmt = conn.prepareStatement(sql);
  
  ```

* 2.设置参数值

* ```
  PreparedStatement对象:setXxx(参数1，参数2):给?赋值
  Xxx:数据类型；如setInt(参数一，参数二)
  参数:
  	参数一:? 的位置编号,从1开始
  	参数二:? 的值
  
  ```

* 执行SQL

* ```java
  executeUpdate(); / executeQuery(); :不需要再传递sql
  ```

  

![image-20220605165347361](.\JDBC.assets\image-20220605165347361.png)



# 数据库连接池

![image-20220606095952808](.\JDBC.assets\image-20220606095952808.png)

## 数据库连接池实现

 * 标准接口:DataSource

    * 官方(Sun)提供的数据库连接池标准接口，由第三方组织实现此接口。

    * 功能：获取连接

    * ```
      Connection getConnection()
      ```

* 常见的数据库连接池

  * DBCP
  * C3P0
  * Druid

* Druid(德鲁伊)

  * Druid连接池是阿里巴巴开源的数据库连接池项目
  * 功能强大，性能优秀，是Java语言最好的数据库连接池之一