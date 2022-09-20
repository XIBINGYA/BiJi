# Request-Response

# ***Request***

![image-20220610084024780](..\.imgs\image-20220610084024780.png)



## Request继承体系

![image-20220610085358636](..\.imgs\image-20220610085358636.png)







## Request获取请求数据

***1.请求行***

```get
GET/request-demo/req1?username=zhangsan HTTP/1.1
```

| 方法                         | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| String getMethod()           | 获取请求方式：GET                                            |
| String getContextParh()      | 获取虚拟目录（项目访问路径）：/request-demo                  |
| StringBuffer getRequestURL() | 获取URL（统一资源定位符）http://localhost:8080/request-demo/req1 |
| String getRequestURI()       | 获取URI（统一资源标识符）：/request-demo/req1                |
| String getQueryString ()     | 获取请求参数(GET方式)：username:=zhangsane&password=123      |

***2.请求头***

```
User-Agent:Mozilla/5.0 Chrome/91.0.4472.106
```

* String getHeader(String name): 根据请求头名称，获取值

***3.请求体***

```
username=superbaby&password=123
```

* ServletInputStream getInputStream(): 获取字节输入流
* BufferedReader getReader(): 获取字符输入流



![image-20220610102859166](..\.imgs\image-20220610102859166.png)



## Request请求参数中文乱码处理

![image-20220610112638022](..\.imgs\image-20220610112638022.png)

![image-20220610114626501](..\.imgs\image-20220610114626501.png)

## Request 请求转发



![image-20220610141830286](..\.imgs\image-20220610141830286.png)

* 实现方法

```java
req.getRequestDispatcher("资源B路径").forward(req,resp);
```

* 请求转发数据间共享数据：使用Request对象
  * void setAttribute(String name, Object o):存储数据到request域中
  * Object getAttribute(String name): 根据key，获取值
  * void removeAttribute(String name):根据key，删除键值对

* 请求转发的特点：
  * 浏览器地址栏路径不发生变化
  * 只能转发到当前服务器的内部资源
  * 一次请求，可以在转发的资源间使用request:共享数据





# ***Response***

## Response 设置响应数据功能介绍

* 响应数据分为三部分
  * 1.响应行：HTTP/1.1 200 OK
    * void setStatus(int sc) : 设置响应状态码
  * 2.响应头：Content-Type:text/html
    * void setHeader(String name, String value):设置响应头键值对
  * 3.响应体:<html><head></head><body></body></html>
    * PrintWriter getWriter():获取字节输出流
    * ServletOutputStream getOutputStream(): 获取字节输出流



## Response完成重定向

![image-20220610144443763](..\.imgs\image-20220610144443763.png)

```java
@WebServlet("/resp1")
public class ResponseDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp1....");

//        // 重定向
//        // 1.设置响应状态码 302
//        resp.setStatus(302);
//        // 2.设置响应头 Location
//        resp.setHeader("location", "/Service/resp2");

        // 简化方式完成重定向
        resp.sendRedirect("/Service/resp2");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);

    }
}

```

* 重定向特点:
  * 浏览器地址栏路径发生变化
  * 可以重定向到任意位置的资源(服务器内部、外部均可)
  * 两次请求，不能在多个资源使用request共享数据

## Resp onse 响应字符数据

* 使用：

  * 1.通过Response对象获取字符输出流

    ```java
    PrintWriter Writer = resp.getWriter();
    ```
    
  * 2.写数据
  
    ```java
    writer.write("aaa");		
    ```
  
    ![image-20220610150722672](..\.imgs\image-20220610150722672.png)
## Response 响应字节数据

* 使用：

  * 1.通过Response对象获取字符输出流

    ```java
    ServletOutputStream outputStream = resp.getOutputSream();	
    ```

  * 2.写数据

    ```java
    outputStream.write(字节数据);
    ```

    ![image-20220610151548549](..\.imgs\image-20220610151548549.png)