# JSP

* 概念：Java Server Pages， Java服务端页面
* 一种动态的网页技术，其中既可以定义HTML，JS，Css等静态内容，还可以定义Java代码的动态内容
* JSP = HTML + Java



 ![image-20220611093236812](..\.imgs\image-20220611093236812.png)

## ***快速入门***

* 1.导入JSP坐标

* ```xml
  <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
        <scope>provided</scope>
  </dependency>
  ```

  

* 2.创建JSP文件

  ![image-20220611095248841](..\.imgs\image-20220611095248841.png)

* 3.编写HTML标签和Java代码

* ```jsp
  <body>
  	<h1>hello jsp~</h1>
  	<% System.out.println("jsp hello~")%>
  </body>
  ```

* 

## ***JSP原理***

* JSP本质上就是一个Servlet
* JSP在被访问时，由JSP容器(Tomcat)将其转换为Java文件(Servlet),在由SP容器(Tomcat)将其编译，最终对外提供服务的其实就是这个字节码文件
   ![image-20220611095758348](..\.imgs\image-20220611095758348.png)

## ***JSP脚本***

![image-20220611100425162](..\.imgs\image-20220611100425162.png)

![image-20220611101515143](..\.imgs\image-20220611101515143.png)

## EL表达式

## JSTL标签

![image-20220611103650697](..\.imgs\image-20220611103650697.png)![image-20220611110823141](..\.imgs\image-20220611110823141.png)



## MVC模式和三层架构

![image-20220611111454415](..\.imgs\image-20220611111454415.png)