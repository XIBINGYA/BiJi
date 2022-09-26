# SpringMVC

* SpringMVC 相当与Servlet 但使用起来更简洁

## 实现步骤

![image-20220920092502606](.\SpringMVC.assets\image-20220920092502606.png)

![image-20220920092512357](.\SpringMVC.assets\image-20220920092512357.png)

![image-20220920092519549](.\SpringMVC.assets\image-20220920092519549.png)

![image-20220920092529250](.\SpringMVC.assets\image-20220920092529250.png)

## Controller加载控制与业务bean加载控制

![image-20220920094628769](.\SpringMVC.assets\image-20220920094628769.png)

## 请求映射路径

![image-20220920150519389](.\SpringMVC.assets\image-20220920150519389.png)

# 

## 请求参数

![image-20220920153854646](.\SpringMVC.assets\image-20220920153854646.png)

![image-20220920153917675](.\SpringMVC.assets\image-20220920153917675.png)

![image-20220920153930178](.\SpringMVC.assets\image-20220920153930178.png)

![image-20220920153946756](.\SpringMVC.assets\image-20220920153946756.png)

## 响应

![image-20220920161022567](.\SpringMVC.assets\image-20220920161022567.png)

![image-20220920161048890](.\SpringMVC.assets\image-20220920161048890.png)

# REST风格

![image-20220920161514548](.\SpringMVC.assets\image-20220920161514548.png)

![image-20220920161700614](.\SpringMVC.assets\image-20220920161700614.png)

![image-20220920172930708](.\SpringMVC.assets\image-20220920172930708.png)

# 异常处理

```java
// 处理异常类
// 开启异常处理器
@RestControllerAdvice
public class ProjectExceptionAdvice {
    // 拦截异常的种类
    @ExceptionHandler(Exception.class)
    public Result doException(Exception ex){
        System.out.println("嘿嘿，异常你来啦");
        return new Result(666,null, "出异常啦");
    }
}
```

### 自定义异常处理







```java
package com.itheima.exception;


// 系统异常处理类
public class SystemException extends RuntimeException{
    // 异常编号
    private Integer code;

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }


    public SystemException(Integer code, Throwable cause) {
        super(cause);
        this.code = code;
    }


    public SystemException( Integer code, String message, Throwable cause) {
        super(message, cause);
        this.code = code;
    }



}

```



```java
package com.itheima.exception;

public class BusinessException extends RuntimeException{
    // 异常编号
    private Integer code;

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public BusinessException(Integer code, String message) {
        super(message);
        this.code = code;
    }


    public BusinessException( Integer code, String message, Throwable cause) {
        super(message, cause);
        this.code = code;
    }


}

```

```java
package com.itheima.controller;

import com.itheima.exception.BusinessException;
import com.itheima.exception.SystemException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

// 处理异常类
// 开启异常处理器
@RestControllerAdvice
public class ProjectExceptionAdvice {
    // 处理系统异常
    @ExceptionHandler(SystemException.class)
    public  Result doSystemException(SystemException ex){
        // 记录日志
        // 发送消息日志
        // 发送邮件给开发人员
        return new Result(ex.getCode(), ex.getMessage());

    }
	
    // 处理业务层异常
    @ExceptionHandler(BusinessException.class)
    public  Result doBusinessException(BusinessException ex){
        // 记录日志
        // 发送消息日志
        // 发送邮件给开发人员
        return new Result(ex.getCode(), ex.getMessage());

    }

    //拦截其他异常的种类
    @ExceptionHandler(Exception.class)
    public Result doException(Exception ex){
        System.out.println("嘿嘿，异常你来啦");
        return new Result(Code.SYSTEM_TIMEOUT_REE,null, "系统繁忙，请稍后再试");
    }
}

```

# 拦截器

* 拦截器(Interceptor)是一种动态拦截方法调用的机制

* 作用：
  * 在指定的方法调用前后执行预先设定后的的代码
  * 阻止原始方法的执行



## 入门案例

* 1声明拦截器的bean，并实现HandlerInterceptor接口(注意:扫描加载bean)

```java
package com.itheima.controller.interceptor;

import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
public class ProjectInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle....");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle.....");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion.....");
    }
}

```

* 2.定义配置类，继承WebMvcConfigurationSupport,实现addInterceptor方法(注意：扫描加载配置)

```java
package com.itheima.config;

import com.itheima.controller.interceptor.ProjectInterceptor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurationSupport;

@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport {
    @Autowired
    ProjectInterceptor projectInterceptor;

    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
    }

    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        // 3.添加拦截器并设定拦截的访问路径，路径可以通过可变参数设置多个
        registry.addInterceptor(projectInterceptor).addPathPatterns("/books");
    }
}

```



![image-20220922150050143](.\SpringMVC.assets\image-20220922150050143.png)
