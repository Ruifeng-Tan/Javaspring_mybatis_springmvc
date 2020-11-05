# 2 SpringMVC

今天我们要创建的还是一个webapp项目。

首先我们将昨天项目的web.xml还有pom.xml，springmvc.xml都复制粘贴过来。

# 一 响应数据和结果视图

## 1 返回值类型

### 1.1 字符串

以后开发的常见方式如下：

①后台查数据(此处代码为模拟)放到Map中，即放进了request域中：

```java
package cn.itcast.controller;

import cn.itcast.domain.User;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Controller
@RequestMapping("/user")
public class UserController {

    /**
     * 返回String
     * @param model
     * @return
     */
    @RequestMapping("/testString")
    public String testString(Model model){
        System.out.println("testString方法执行了...");
        // 模拟从数据库中查询出User对象
        User user = new User();
        user.setUsername("美美");
        user.setPassword("123");
        user.setAge(30);
        // model对象
        model.addAttribute("user",user);
        return "success";
    }

}

```

②前端通过${}传值展示数据

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2018/5/1
  Time: 1:18
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

    <h3>执行成功</h3>

    ${user.username}
    ${user.password}

</body>
</html>

```

### 1.2 返回void

我们想要用控制器方法返回void，我们不进行return是行不通的。这样会返回一个默认界面，默认界面是根据@RequestMapping的路径来的，比如以下方法就会返回testVoid.jsp页面，如果没有这个页面就会返回错误状态码。

```java
    /**
     * 是void
     * 请求转发一次请求，不用编写项目的名称
     */
    @RequestMapping("/testVoid")
    public void testVoid(HttpServletRequest request, HttpServletResponse response) throws Exception {
        System.out.println("testVoid方法执行了...");

    }
```

解决方法有两种

#### 1.2.1 编写请求转发的程序

```java
    /**
     * 是void
     * 请求转发一次请求，不用编写项目的名称
     */
    @RequestMapping("/testVoid")
    public void testVoid(HttpServletRequest request, HttpServletResponse response) throws Exception {
        System.out.println("testVoid方法执行了...");
        // 编写请求转发的程序
        request.getRequestDispatcher("/WEB-INF/pages/success.jsp").forward(request,response);


        return;
    }
```

#### 1.2.2 重定向

request.getContextPath()用于获取项目路径，即`http://localhost:8080/springmvc_day02_01_response`

```java
    /**
     * 是void
     * 请求转发一次请求，不用编写项目的名称
     */
    @RequestMapping("/testVoid")
    public void testVoid(HttpServletRequest request, HttpServletResponse response) throws Exception {
        System.out.println("testVoid方法执行了...");

        // 重定向
        response.sendRedirect(request.getContextPath()+"/index.jsp");


        return;
    }
```

#### 1.2.3  直接进行响应

```java
    /**
     * 是void
     * 请求转发一次请求，不用编写项目的名称
     */
    @RequestMapping("/testVoid")
    public void testVoid(HttpServletRequest request, HttpServletResponse response) throws Exception {
        System.out.println("testVoid方法执行了...");

        // 设置中文乱码
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");

        // 直接会进行响应
        response.getWriter().print("你好");

        return;
    }
```

