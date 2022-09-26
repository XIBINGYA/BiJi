# MybatisPlus

* MyBatisPlus(简称MP)是基于MyBatis框架基础上开发的增强型工具，旨在简化开发，提高效率

```xml
<dependency>
   <groupId>com.baomidou</groupId>
   <artifactId>mybatis-plus-boot-starter</artifactId>
   <version>3.4.1</version>
</dependency>

<dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>druid</artifactId>
   <version>1.1.16</version>
</dependency>        

```

```java

@Mapper
public interface UserMapper extends BaseMapper<User> {

}
```

## MybatisPlus 简介(看看就行,简化开发)

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

## 条件查询--设置查询条件

* 格式一:常规格式

  ```java
  QuerWrapper<User> qw = new QueryWrapper<User>();
  
  // 查询年龄大于等于18岁，小于65岁的用户
  qw.lt("age", 65);
  qw.ge("age", 18);
  
  List<User> userList = userMapper.selectList(qw);
  System.out.println(userList);
  
  ```

  

* 格式二：链式编程格式**（lt小于，ge大于）**

  ```java
  QueryWrapper<User> qw = new QueryWrapper<User>();
  // 查询年龄大于等于18岁，小于65岁的用户
  qw.lt("age",65).ge("age",18);
  List<User> userList = userMapper.selectList(qw);
  System.out.println(userList);
  ```

  



* 格式三:lambda格式(**推荐**)

```java
LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();

//查询年龄大于等于18岁，小于65岁的用户
lqw.lt(User::getAge,65).ge(User::getAge,18);

List<User> userList =  userMapper.selectList(lqw);
System.out.println(userList);
```





## 条件查询-----组合查询条件

* 并且(and)

  ```java
  LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
  
  //查询年龄大于等于18岁，小于65岁的用户
  lqw.lt(User::getAge,65).ge(User::getAge,18);
  
  List<User> userList =  userMapper.selectList(lqw);
  System.out.println(userList);
  ```

* 或者(or)

  ```java
  LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
  
  //查询年龄大于等于18岁，小于65岁的用户
  lqw.lt(User::getAge,65).**or()**.ge(User::getAge,18);
  
  List<User> userList =  userMapper.selectList(lqw);
  System.out.println(userList);
  ```

  

## 条件查询---null值处理

```java
LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
lqw.lt(null!=uq.getAge(),User::getAge, uq.getAge());
lqw.gt(null!=uq.getAge2(),User::getAge, uq.getAge2());
```



# DQL编程控制



## 查询投影

**(查询出来看到的结果)**

* 查询结果包含模型类中部分属性

```java
// 只查询id，name,age 三个属性
LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
lqw.select(User::getId,User::getName,User::getAge);
List<User> userList = userMapper.selectList(lqw);
System.out.println(userList);
```

* 查询结果包含模型类中未定义的属性

```java
QueryWrapper<User> qm = new QueryWrapper<User>();
qm.select("count(*) as nums,gender");
qm.groupBy("gender");
List<Map<String,Object>> maps = userMapper.selectMaps(qm);
System.out.println(maps);
```

## 查询条件

* 用户登录

  ```java
  LambdaQueryWrapper<User> lqw=  new LambdaQueryWrapper<>();
  
  lqw.eq(User::getName, "Tom").eq(User::getPassword, "tom");
  List<User> user = userMapper.selectList(lqw);
  System.out.println(user);
  ```

  

* 范围的匹配

```java
LambdaQueryWrapper<User> lqw=  new LambdaQueryWrapper<>();

lqw.between(User::getAge, 10, 30);
List<User> user = userMapper.selectList(lqw);
System.out.println(user);
```

* 模糊匹配

```java
LambdaQueryWrapper<User> lqw=  new LambdaQueryWrapper<>();

lqw.like(User::getName, "J");
List<User> user = userMapper.selectList(lqw);
System.out.println(user);
```

* 使用指南 https://baomidou.com/pages/10c804/#notlike

## 字段映射与表名映射

* 名称：@TableField

* 类型：属性注解

* 位置：模型类属性定义上方

* 作用：设置当前属性对应的数据库表中的字段关系

* 范例：
  ```java
  public class User{
      @TableField(value="pwd",select = false)
      private String password;
  	
  	@TableFild(exist=false)
  	private Integer online;
  }
  
  
  ```
* 相关属性 

  * value（默认）：设置数据库表字段名称
  * exist：设置属性在数据库字段中是否存在，默认为true。此属性无法与value合并使用
  * select：设置属性是否参与查询，此属性与select()映射配置不冲突

------------------

* 名称：@TableName

* 类型：类注解

* 位置：模型类定义上方

* 作用：设置当前类对应与数据库表关系

* 范例：
  ```
  @TableName("tbl_user")
  public class User{
  	private Long id;
  }
  
  ```
* 相关属性 
  
  * value: 设置数据库表名称

## id生产策略控制

* 不同的表应用不同的id生成策略
* 日志：自增(1,2,3,4，…)
* 购物订单：特殊规则(FQ23948AK3843)
* 外卖单：关联地区日期等信息(1004202003143491)
* 关系表：可省略id
* 。。。。。

---

* 名称：@TableId

* 类型：**属性注解**

* 位置：模型类中用于表示主键的属性定义上方

* 作用：设置当前类中主键属性的生成策略

* 范例：
  ```java
  public class User{
      @TableId(type IdType.AUTO)
      private Long id;
  }
  ```
* 相关属性
  
  * value:设置数据库主键名称
  * type:设置主键属性的生成策略，值参照IdType枚举值

## 逻辑删除

* 删除操作业务问题：业务数据从数据库中丢弃
* 逻辑删除：为数据设置是否可用状态字段，删除时设置状态字段为不可用状态，数据保留在数据库中

![image-20220926145832992](.\MybatisPlus.assets\image-20220926145832992.png)

![image-20220926151646607](.\MybatisPlus.assets\image-20220926151646607.png)

* 2.实体类中添加对应字段，并设定当前字段为逻辑删除标记字段

```java
public class User {
	private Long id;
	@TableLogic
	private Integer deleted;
}
```

* 3.配置逻辑删除(与上一个操作功能一样，但是这个通用**推荐使用**)

```yml
mybatis-plus:
	global-config:
		db-config:
            logic-delete-field:deleted
            logic-not-delete-value:0
            logic-delete-value:1
```





## 乐观锁

![image-20220926153502544](.\MybatisPlus.assets\image-20220926153502544.png)

![image-20220926153517700](.\MybatisPlus.assets\image-20220926153517700.png)

![image-20220926153540611](.\MybatisPlus.assets\image-20220926153540611.png)

![image-20220926153551382](.\MybatisPlus.assets\image-20220926153551382.png)

## 代码生成器

* 模板：MyBatisPlus提供
* 数据库相关配置：读取数据库获取信息
* 开发者自定义配置：手工配置
