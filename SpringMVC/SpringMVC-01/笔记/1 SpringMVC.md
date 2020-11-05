[TOC]



# 1 SpringMVC



# 一 SpringMVC的基本概念

## 1 三层架构和MVC

### 1.1 三层架构

我们使用

SpringMVC开发表现层

Spring开发业务层

Mybatis开发持久层



![image-20201103102253745](images/image-20201103102253745.png)

![image-20201103102517894](images/image-20201103102517894.png)



### 1.2 MVC模型

MVC是Model，View，Controller的缩写，是我们**开发表现层的常用设计模型**。

[点击打开资料](D:\WHU\JavaEE\Javaspring_mybatis_springmvc\SpringMVC\SpringMVC-01\资料)

我们用Model封装数据，View展示数据，Controller用于处理数据。





## 2 SpringMVC概述

### 2.1 概念

SpringMVC是Java的实现MVC设计模型的请求驱动类型的轻量级Web框架。是现在的主流web开发框架。



### 2.2 SpringMVC在三层架构中的位置

![image-20201103103123080](images/image-20201103103123080.png)





### 2.3 优势

模块开发极好，支持扩展。

[点击打开资料](D:\WHU\JavaEE\Javaspring_mybatis_springmvc\SpringMVC\SpringMVC-01\资料)

![image-20201103103256824](images/image-20201103103256824.png)

### 

### 2.4 SpringMVC和Struts2的优劣分析

该部分面试可能考到，但我们只要大概了解即可，不需要掌握Struts2

![image-20201103103657066](images/image-20201103103657066.png)



# 二 SpringMVC的入门案例

## 1 入门需求

用户发送一个请求，如果请求成功了，则返回一个请求成功的页面。



## 2 搭建环境

1. 创建Webapp工程

   ![image-20201103104437722](images/image-20201103104437722.png)

2. 在创建项目的时候添加一组键值对 archetypeCatalog=internal。可以解决项目创建过慢的问题。不过我们创建项目由于配置了阿里云的maven仓库，就没有这个问题了。

   ![image-20201103104853450](images/image-20201103104853450.png)

3. 创建完以后工程的文件夹目录如下，和黑马老师的是不一样的，我们自动配置了很多需要收到配置的文件目录结构：

   ![image-20201103105845055](images/image-20201103105845055.png)

4. 一键复制粘贴导入pom.xml的Jar包，其实在我们创建工程的时候就自动配置好了，但是黑马提供的这个pom.xml更好，它提供了spring框架的jar包绑定，以后我们需要更换spring版本的时候，只需要更改`spring.version`标签中指定的版本即可。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>cn.itcast</groupId>
     <artifactId>springmvc_day01_01_start</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>war</packaging>
   
     <name>springmvc_day01_01_start Maven Webapp</name>
     <!-- FIXME change it to the project's website -->
     <url>http://www.example.com</url>
   
     <properties>
       <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
       <spring.version>5.0.2.RELEASE</spring.version>
     </properties>
   
     <dependencies>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>${spring.version}</version>
       </dependency>
   
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-web</artifactId>
         <version>${spring.version}</version>
       </dependency>
   
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>${spring.version}</version>
       </dependency>
   
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>servlet-api</artifactId>
         <version>2.5</version>
         <scope>provided</scope>
       </dependency>
   
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>jsp-api</artifactId>
         <version>2.0</version>
         <scope>provided</scope>
       </dependency>
   
     </dependencies>
   
     <build>
       <finalName>springmvc_day01_01_start</finalName>
       <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
         <plugins>
           <plugin>
             <artifactId>maven-clean-plugin</artifactId>
             <version>3.0.0</version>
           </plugin>
           <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
           <plugin>
             <artifactId>maven-resources-plugin</artifactId>
             <version>3.0.2</version>
           </plugin>
           <plugin>
             <artifactId>maven-compiler-plugin</artifactId>
             <version>3.7.0</version>
           </plugin>
           <plugin>
             <artifactId>maven-surefire-plugin</artifactId>
             <version>2.20.1</version>
           </plugin>
           <plugin>
             <artifactId>maven-war-plugin</artifactId>
             <version>3.2.0</version>
           </plugin>
           <plugin>
             <artifactId>maven-install-plugin</artifactId>
             <version>2.5.2</version>
           </plugin>
           <plugin>
             <artifactId>maven-deploy-plugin</artifactId>
             <version>2.8.2</version>
           </plugin>
         </plugins>
       </pluginManagement>
     </build>
   </project>
   
   ```

5. 配置前端控制器

   ```xml
   <!DOCTYPE web-app PUBLIC
    "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
    "http://java.sun.com/dtd/web-app_2_3.dtd" >
   
   <web-app>
     <display-name>Archetype Created Web Application</display-name>
   
     <!--配置前端控制器-->
     <servlet>
       <servlet-name>dispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
           <!--设定要加载的配置文件的路径-->
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:springmvc.xml</param-value>
       </init-param>
         <!--启动服务器时即加载配置-->
       <load-on-startup>1</load-on-startup> 
     </servlet>
     <servlet-mapping>
       <servlet-name>dispatcherServlet</servlet-name>
       <url-pattern>/</url-pattern> <!--斜杠表示所有的请求都会经过该控制器-->
     </servlet-mapping>
   
     <!--配置解决中文乱码的过滤器-->
     <filter>
       <filter-name>characterEncodingFilter</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <init-param>
         <param-name>encoding</param-name>
         <param-value>UTF-8</param-value>
       </init-param>
     </filter>
     <filter-mapping>
       <filter-name>characterEncodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
     </filter-mapping>
   
   </web-app>
   
   ```

   

6. 将项目部署到本地服务器上，参见[本章第三小节](#3 Tomcat部署到本地)

   


## 3 Tomcat部署到本地

首先我们要安装tomcat，教程：https://www.jianshu.com/p/4d1007d1f548

我们的tomcat的保存路径如图：

![image-20201103222650288](images/image-20201103222650288.png)

然后创建服务器配置，教程参见CSDN：https://blog.csdn.net/pan_junbiao/article/details/89639004

之后接着按照视频操作即可。



## 4 工程代码编写

### 4.1 输出Hello

①首先我们先要在resources文件夹下创建一个springmvc.xml配置文件，用于导入spring框架，并且指定IOC容器需要扫描的包的位置，并开启对注解开发的支持。

在这里我们还需要配置**视图解析器**，用于指定我们编写的页面代码的存放路径以及后缀：

- prefix配置文件目录
- suffix配置文件的后缀



**注意：**开启SpringMVC框架的注解支持的同时也自动加载了注解映射器和适配器。

![image-20201104190932745](images/image-20201104190932745.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 开启注解扫描 -->
    <context:component-scan base-package="cn.itcast"/>

    <!-- 视图解析器对象 -->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <!--配置自定义类型转换器-->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="cn.itcast.utils.StringToDateConverter"/>
            </set>
        </property>
    </bean>


    <!-- 开启SpringMVC框架注解的支持 -->
    <mvc:annotation-driven conversion-service="conversionService"/>

</beans>
```

②然后我们需要编写一个controller类，来进行输出Hello的控制。

为了告诉spring这是一个要假如到IOC容器的component，我们使用@Controller注解，@Controller专门用于指明表现层的组件。

同时为了告诉springmvc我们调用sayHello方法的请求路径，我们还需要使用@RequestMapping注解。

**注意，这里的方法时return "success"，有一个十分有趣的默认，那就是spring会默认返回一个叫做success的.jsp页面，为了找到该界面，就需要配置试图解析器。**

```java
package cn.itcast.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

// 控制器类
@Controller
@RequestMapping(path="/user")
public class HelloController {

    /**
     * 入门案例
     * @return
     */
    @RequestMapping(path="/hello")
    public String sayHello(){
        System.out.println("Hello StringMVC");
        return "success";
    }

    /**
     * RequestMapping注解
     * @return
     */
    @RequestMapping(value="/testRequestMapping",params = {"username=heihei"},headers = {"Accept"})
    public String testRequestMapping(){
        System.out.println("测试RequestMapping注解...");
        return "success";
    }

}

```

href是一个相对路径的写法。[a href标签的作用讲解](https://www.w3school.com.cn/tags/att_a_href.asp)

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2018/4/29
  Time: 0:53
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

    <h3>入门程序</h3>
    <%--
        <a href="hello">入门程序</a>
    <%--href="hello"的意思是如果一会发请求，就会执行path为/hello的方法--%>
    --%>

    <a href="user/testRequestMapping?username=heihei">RequestMapping注解</a>

</body>
</html>

```

咱们的入门成功的返回界面的代码如下：

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2018/4/29
  Time: 1:02
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

    <h3>入门成功</h3>

    ${ msg }

    ${sessionScope}

</body>
</html>

```



## 5 入门案例的流程总结

其实这可以理解为开发一个简单web项目的通用流程

- 启动服务器，加载一些配置文件。**(创建对象，让注解生效)**
  - DispatcherServlet对象被创建
  - springmvc.xml被加载了
  - Hellocontroller被创建成了对象放入IOC容器中

- 发送请求，后端处理请求，然后根据返回的页面在试图解析器中解析找到页面返回给前端显示。

![image-20201104185739217](images/image-20201104185739217.png)



[详细springmvc执行流程原理图，请点击！](images/springmvc执行流程原理.jpg)



## 6 ResultMapping注解(重点)

### 6.1 简介

`@ResultMapping`是一个配置路由的注解，配置在Controller类的上方则每个需要改controller类响应的请求，都必须要经过该路由，加在类方法的上方，则需要在满足类的路由（如有)的前提下再满足方法的路由配置才可以被方法响应。

```java
// 控制器类
@Controller
@RequestMapping(path="/user")
public class HelloController {

    /**
     * 入门案例
     * @return
     */
    @RequestMapping(path="/hello")
    public String sayHello(){
        System.out.println("Hello StringMVC");
        return "success";
    }

    /**
     * RequestMapping注解
     * @return
     */
    @RequestMapping(value="/testRequestMapping",params = {"username=heihei"},headers = {"Accept"})
    public String testRequestMapping(){
        System.out.println("测试RequestMapping注解...");
        return "success";
    }

}
```

### 6.2 注解的属性

- value的别名是path，二者作用是相同的：用于指定请求的URL。只有一个属性时，value可省略不写。即@RequestMapping(value="/hello")和@RequestMapping("/hello")的作用是一样的。

- method：用于指定请求的方法是get还是post，put，delete。是一个RequestMethod枚举类对象的数组。

  写法如(前端的默认请求是GET)：

  ```java
  @RequestMapping(value="/testRequestMapping",method={RequestMethod.GET})
  ```

  

- params

  ![image-20201104191944519](images/image-20201104191944519.png)



- headers：用于指定限制消息头的条件。(不常用)



**补充：**POST请求的Jsp代码如下：

```jsp
<!-- 请求方式的示例 -->
<a href="account/saveAccount">保存账户，get 请求</a>
<br/>
<form action="account/saveAccount" method="post">
<input type="submit" value="保存账户，post 请求">
</form>
```



# 三 请求参数的绑定(重点)

## 1 绑定机制

即将请求的表单中的参数自动传入到方法的参数中调用方法。

[绑定机制说明图](images/image-20201104193213658.png)

## 2 支持的数据类型

![image-20201104193111878](images/image-20201104193111878.png)



## 3 Get请求的参数绑定

```jsp
<a href="user/testRequestMapping?username=heihei">RequestMapping注解</a>
```

```java
    /**
     * RequestMapping注解
     * @return
     */
    @RequestMapping(value="/testRequestMapping",headers = {"Accept"})
    public String testRequestMapping(String username){
        System.out.println("测试RequestMapping注解...");
        return "success";
    }

```



## 4 Post请求的参数绑定

### 4.1 简单POST

注意提交的表单对象的属性名必须与需要解析的java类对象的属性名相同。

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2018/4/29
  Time: 22:10
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>




    <%--把数据封装Account类中，类中存在list和map的集合 --%>  
    <form action="param/saveAccount" method="post">
        姓名：<input type="text" name="username" /><br/>
        密码：<input type="text" name="password" /><br/>
        金额：<input type="text" name="money" /><br/>
        <input type="submit" value="提交" />
    </form>
	

    <a href="param/testServlet">Servlet原生的API</a>

</body>
</html>

```

我们的前端界面这时就是一个表单，我们只需要输入数据提交即可:

![image-20201104195320661](images/image-20201104195320661.png)



### 4.2 传递为类对象的属性

如果我们的Account类对象中有一个User类对象属性呢？我们该如何在表单传递User类对象呢？这时候我们只需要传user.xxxx，把user的xxxx属性都传过来即可写法如下:

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2018/4/29
  Time: 22:10
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

    <%--请求参数绑定--%>

    把数据封装Account类中
    <form action="param/saveAccount" method="post">
        姓名：<input type="text" name="username" /><br/>
        密码：<input type="text" name="password" /><br/>
        金额：<input type="text" name="money" /><br/>
        用户姓名：<input type="text" name="user.uname" /><br/>
        用户年龄：<input type="text" name="user.age" /><br/>
        <input type="submit" value="提交" />
    </form>

    <a href="param/testServlet">Servlet原生的API</a>

</body>
</html>

```



### 4.3 传递集合类属性

如果我们的Account类对象的User是一个集合，而且有别的集合怎么办？

```java
package cn.itcast.domain;

import java.io.Serializable;
import java.util.List;
import java.util.Map;

public class Account implements Serializable{

    private String username;
    private String password;
    private Double money;

   // private User user;

    private List<User> list;
    private Map<String,User> map;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Double getMoney() {
        return money;
    }

    public void setMoney(Double money) {
        this.money = money;
    }

    public List<User> getList() {
        return list;
    }

    public void setList(List<User> list) {
        this.list = list;
    }

    public Map<String, User> getMap() {
        return map;
    }

    public void setMap(Map<String, User> map) {
        this.map = map;
    }

    /*

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }
*/

    @Override
    public String toString() {
        return "Account{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", money=" + money +
                ", list=" + list +
                ", map=" + map +
                '}';
    }
}

```

传表单的方法:

list[0].umane的意思是将uname封装到list的0对象上。

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2018/4/29
  Time: 22:10
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>



    <%--把数据封装Account类中，类中存在list和map的集合    --%>
    <form action="param/saveAccount" method="post">
        姓名：<input type="text" name="username" /><br/>
        密码：<input type="text" name="password" /><br/>
        金额：<input type="text" name="money" /><br/>

        用户姓名：<input type="text" name="list[0].uname" /><br/>
        用户年龄：<input type="text" name="list[0].age" /><br/>

        用户姓名：<input type="text" name="map['one'].uname" /><br/>
        用户年龄：<input type="text" name="map['one'].age" /><br/>
        <input type="submit" value="提交" />
    </form>




    <a href="param/testServlet">Servlet原生的API</a>

</body>
</html>

```



## 5 特殊情况

假如用户有一个Date属性表示生日。

```java
private Date date;
```

如果我们输入`2000/11/11`这种正常的日期格式，springmvc也能够将其从字符串封装成Date类型的日期。

当如果我们输入2000-11-11这种错误格式就会封装失败。这时候我们就要用到`自定义类型转换器`

### 5.1 自定义类型转换器

创建一个新的包叫utils，在下面创建我们的自定义类型转换器类。由于我们的转换器实现了String向Date转换的接口，所以之后的请求如果出现了将String向Date转换的案例，则会直接寻求自定义的实现是否能够满足要求，即自定义的转换会取代默认。

```java
package cn.itcast.utils;

import org.springframework.core.convert.converter.Converter;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * 把字符串转换日期
 */
public class StringToDateConverter implements Converter<String,Date>{

    /**
     * String source    传入进来字符串
     * @param source
     * @return
     */
    public Date convert(String source) {
        // 判断
        if(source == null){
            throw new RuntimeException("请您传入数据");
        }
        // 新建支持的转换格式
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd");

        try {
            // 把字符串转换日期
            return df.parse(source);
        } catch (Exception e) {
            throw new RuntimeException("数据类型转换出现错误");
        }
    }

}

```



### 5.2 在springmvc.xml中开启自定义类型转换器

class：org.springframework.context.support.ConversionServiceFactoryBean是固定的

property name是固定的

id 我们自己取

bean class是我们的自定义转换器类的全路径

```xml
    <!--配置自定义类型转换器-->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="cn.itcast.utils.StringToDateConverter"/>
            </set>
        </property>
    </bean>
```

然后在mvc标签中开启该转换器

```xml
    <!-- 开启SpringMVC框架注解的支持 -->
    <mvc:annotation-driven conversion-service="conversionService"/>
```



## 6 获取Servelet的原生api

如果我们想获取API的原生对象，如request以及response，那么该如何获取呢？

我们只需要在方法的参数里面写就行了，如下:

```java
    /**
     * 原生的API
     * @return
     */
    @RequestMapping("/testServlet")
    public String testServlet(HttpServletRequest request, HttpServletResponse response){
        System.out.println("执行了...");
        System.out.println(request);

        HttpSession session = request.getSession();
        System.out.println(session);

        ServletContext servletContext = session.getServletContext();
        System.out.println(servletContext);

        System.out.println(response);
        return "success";
    }
```





# 四 常用注解

restful编程风格尽量的根据Http请求的类型与参数来决定调用的控制器的方法，而不是给每一个方法都配置一个专门的路由。

![image-20201105155230592](images/image-20201105155230592.png)

## 1 RequestParam

作用： 把请求中指定名称的参数给控制器中的形参赋值。 

属性：

-  value：请求参数中的名称。

-  required：请求参数中是否必须提供此参数。默认值：true。表示必须提供，如果不提供将报错。



使用该方法可以让我们请求的名字与controller方法的参数名字不一样。

```java
@RequestMapping("/useRequestParam")
public String   useRequestParam(@RequestParam("name")String username,
@RequestParam(value="age",required=false)Integer age){ System.out.println(username+","+age);
return "success";
}

```



## 2 RequestBody

作用：

用于获取请求体内容。直接使用得到是 key=value&key=value...结构的数据。 get 请求方式不适用。

属性：

- required：是否必须有请求体。默认值是:true。当取值为 true 时,get 请求方式会报错。如果取值为 false，get 请求得到是 null。



如以下代码所示，当我们给控制器的一个方法形参加上了该注解以后，**这个形参取回的就是请求的整个表单**，而不再是一个同名的请求参数。

```java
//post 请求 jsp 代码：
<!-- request body 注解 -->
<form action="springmvc/useRequestBody" method="post">
用户名称：<input type="text" name="username" ><br/>
用户密码：<input type="password" name="password" ><br/>
用户年龄：<input type="text" name="age" ><br/>
<input type="submit" value="保存">
</form>
//get 请求 jsp 代码：
<a href="springmvc/useRequestBody?body=test">requestBody 注解 get 请求</a>控制器代码：
/**
* RequestBody 注解
* @param user
* @return
*/ @RequestMapping("/useRequestBody")
public String   useRequestBody(@RequestBody(required=false) String body){ System.out.println(body);
return "success";
}

```



## 3 PathVaribale

作用：

用于绑定 url 中的占位符。例如：请求 url 中 /delete/{id}，这个{id}就是 url 占位符。

url 支持占位符是 spring3.0 之后加入的。是 springmvc 支持 rest 风格 URL 的一个重要标志。

属性：

- value：用于指定 url 中占位符名称。
- required：是否必须提供占位符。



```jsp
<a href="anno/testPathVariable/10">testPathVariable</a>
```

```java
    /**
     * PathVariable注解
     * @return
     */
    @RequestMapping(value="/testPathVariable/{sid}")
    public String testPathVariable(@PathVariable(name="sid") String id){
        System.out.println("执行了...");
        System.out.println(id);
        return "success";
    }
```



## 4 RequestHeader(不常用)

作用：

用于获取请求消息头。

属性：

- value：提供消息头名称
-  required：是否必须有此消息头
- 

注:

在实际开发中一般不怎么用。



## 5 CookieValue

作用：

用于把指定 cookie 名称的值传入控制器方法参数。

属性：

- value：指定 cookie 的名称。
- required：是否必须有此 cookie。

```java
    /**
     * 获取Cookie的值
     * @return
     */
    @RequestMapping(value="/testCookieValue")
    public String testCookieValue(@CookieValue(value="JSESSIONID") String cookieValue){
        System.out.println("执行了...");
        System.out.println(cookieValue);
        return "success";
    }
```



## 6 ModelAttribute

作用：

该注解是 SpringMVC4.3 版本以后新加入的。它可以用于修饰方法和参数。

出现在方法上，表示当前方法会在控制器的方法执行之前，先执行。它可以修饰没有返回值的方法，也可以修饰有具体返回值的方法。

出现在参数上，获取指定的数据给参数赋值。

属性：

- value：用于获取数据的 key。key 可以是 POJO 的属性名称，也可以是 map 结构的 key。

应用场景：

当表单提交数据不是完整的实体类数据时，保证没有提交数据的字段使用数据库对象原来的数据。

例如：

我们在编辑一个用户时，用户有一个创建信息字段，该字段的值是不允许被修改的。在提交表单数据是肯定没有此

字段的内容，一旦更新会把该字段内容置为 null，此时就可以使用此注解解决问题。

JSP代码如下：

```jsp
    <form action="anno/testModelAttribute" method="post">
        用户姓名：<input type="text" name="uname" /><br/>
        用户年龄：<input type="text" name="age" /><br/>
        <input type="submit" value="提交" />
    </form>

```

### 6.1 有return的ModelAttribute方法

若方法有返回值，请求参数变成了方法的返回值，接下来在控制器类中就会寻找与返回值同类型同名的控制器方法继续处理请求

控制器代码如下：

```java
    @RequestMapping(value="/testModelAttribute")
    public String testModelAttribute(@ModelAttribute("abc") User user){
        System.out.println("testModelAttribute执行了...");
        System.out.println(user);
        return "success";
    }

    /**
     * 该方法会先执行
	*/
    @ModelAttribute
    public User showUser(String uname){
        System.out.println("showUser执行了...");
        // 通过用户查询数据库（模拟）
        User user = new User();
        user.setUname(uname);
        user.setAge(20); // 请求表单没有提交用户年龄以及生日属性
        user.setDate(new Date());
        return user;
    }
     
```



### 6.2 无return的ModelAttribute方法

@ModelAttribute的方法没有返回值时可以将数据存入到Map结构中，然后在控制器方法中从Map中取值。

```java
    /**
     * ModelAttribute注解
     * @return
     */
    @RequestMapping(value="/testModelAttribute")
    public String testModelAttribute(@ModelAttribute("abc") User user){
        System.out.println("testModelAttribute执行了...");
        System.out.println(user);
        return "success";
    }

    @ModelAttribute
    public void showUser(String uname, Map<String,User> map){
        System.out.println("showUser执行了...");
        // 通过用户查询数据库（模拟）
        User user = new User();
        user.setUname(uname);
        user.setAge(20);
        user.setDate(new Date());
        map.put("abc",user);// 将数据存入map中
    }
```



## 7 SessionAttribute

作用：

用于多次执行控制器方法间的参数共享。

属性：

- value：用于指定存入的属性名称
-  type：用于指定存入的数据类型。



### 7.1 存入值

如果我们想让控制器处理请求，并在处理请求之后将一定的信息传入到返回页面上展示，就要用到该注解。

**spring专门提供了一个Model类来处理这种请求信息的存储获取**。

测试的JSP代码如下：

```jsp
<a href="anno/testSessionAttributes">testSessionAttributes</a>
```



控制器代码如下：

```java
    /**
     * SessionAttributes的注解
     * @return
     */
    @RequestMapping(value="/testSessionAttributes")
    public String testSessionAttributes(Model model){
        System.out.println("testSessionAttributes...");
        // 底层会存储到request域对象中
        model.addAttribute("msg","美美");
        return "success";
    }
```



这时候我们在success界面中就可以通过${}获取存在请求域的值，如果我们在控制器类上也加一个`@SessionAttributes(value={"msg"})`注解，就可以把键为`msg`的键值对存入到session域中。这时候我们就可以在`${sessionScope}`也看到我们存入的‘美美“。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

    <h3>入门成功</h3>

    ${ msg }

    ${sessionScope}

</body>
</html>

```

前端展示如图：

![image-20201105163041166](images/image-20201105163041166.png)



### 7.2 获取值

当将数据存到到session域之后，在同一session的会话，就可以在处理请求的过程中随时获取保存在session域中的属性进行需要的操作。比如我现在的以下方法就可以取出之前的`testSessionAttributes`方法存进session域中的”美美“。

```java
    /**
     * 获取值
     * @param modelMap
     * @return
     */
    @RequestMapping(value="/getSessionAttributes")
    public String getSessionAttributes(ModelMap modelMap){
        System.out.println("getSessionAttributes...");
        String msg = (String) modelMap.get("msg");
        System.out.println(msg);
        return "success";
    }
```



### 7.3 删除值

调用SessionStatus对象的setComplete方法，表示设置完成，清空session中的所有数据。

```java
    /**
     * 清除
     * @param status
     * @return
     */
    @RequestMapping(value="/delSessionAttributes")
    public String delSessionAttributes(SessionStatus status){
        System.out.println("getSessionAttributes...");
        status.setComplete();
        return "success";
    }
```

