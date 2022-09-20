# Servlet(动态资源)

## 快速入门





## Servlet 执行流程

***1.创建web项目，导入Servlet依赖坐标***

```xml
<dependency>
<groupld>javax.servlet</groupld>
<artifactld>javax.servlet-api</artifactld>
<version>3.1.0</version>
<scope>provided</scope>
</dependency>
```



***2.创建：定义一个类，实现Servlet接口，并重写接口中所有方法，并在service方法中输入一句话***

```java
public class ServletDemo1 implements Servlet{
	public void service(){}
}

```



***3.配置：在类上使用@WebServlet注解，配置该Servicet的访问路径***

```java
@WebServlet("/demo1")
public class ServletDemo1 implements Servlet{}
```



***4.访问：启动Tomcat，浏览器输入URL访问Servlet***

```url
http://localhost:8080/web-demo/demo1
```



## Servlet 生命周期

![image-20220609151356480](..\.imgs\image-20220609151356480.png)

![image-20220609151425944](..\.imgs\image-20220609151425944.png)

## Servlet 体系结构

![image-20220609152643824](..\.imgs\image-20220609152643824.png)

## Servlet urlPattern 配置

![image-20220609155620781](..\.imgs\image-20220609155620781.png)

### 配置规则

![image-20220609155807263](..\.imgs\image-20220609155807263.png)



## XML 配置方式编写Servlet

