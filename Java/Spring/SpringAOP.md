# AOP

* AOP(Aspect Oriented Programming)面向切面编程，一种编程范式，指导开发者如何组织程序结构
  * OoP(Object0 riented Programming)面向对象编程

* 作用：在不惊动原始设计的基础上为其进行功能增强
* Spring理念: 无入侵式/无侵入式编程

![image-20220919085859649](.\SpringAOP.assets\image-20220919085859649.png)

* 连接点(]oinPoint):程序执行过程中的任意位置，粒度为执行方法、抛出异常、设置变量等

  * 在SpringAOP中，理解为方法的执行

  

* 切入点(Pointcut):匹配连接点的式子

  * 在SpringAOP中，一个切入点可以只描述一个具体方法，也可以匹配多个方法

    * 一个具体方法：com.itheima,dao包下的BookDao接口中的无形参无返回值的save方法
    * 匹配多个方法：所有的save方法，所有的get开头的方法，所有以Dao结尾的接口中的任意方法，所有带有一个参数的方法

    

* 通知(Advice):在切入点处执行的操作，也就是共性功能

  * 在SpringAOP中，功能最终以方法的形式呈现

  

* 通知类：定义通知的类

* 切面(Aspect):描述通知与切入点的对应关系

## AOP入门案例

* 案例设定：测定接口执行效率
* 简化设定：在接口执行前输出当前系统时间
* 开发模式：XML or注解
* 思路分析：
  * 1.导入坐标(pom.xm1)
  * 2.制作连接点方法(原始操作，Dao接口与实现类)
  * 3.制作共性功能（通知类与通知）
  * 4.定义切入点
  * 5.绑定切入点与通知关系（切面）



## AOP切入点表达式

![image-20220919154535540](.\SpringAOP.assets\image-20220919154535540.png)

## AOP通知

![image-20220919160219671](.\SpringAOP.assets\image-20220919160219671.png)

## 事物管理

![image-20220919194232743](.\SpringAOP.assets\image-20220919194232743.png)

## 事物的传播行为

![image-20220919201405665](.\SpringAOP.assets\image-20220919201405665.png)