# Mybatis踩坑笔记

## 1 空格

在写sql语句的时候，为了避免一行过长，我们常常会换行，如下所示：

```sql
        select role.ID as RID, role.ROLE_NAME as ROLE_NAME, role.ROLE_DESC as ROLE_DESC, user.* from role
left outer  join user_role on user_role.RID=role.ID
left outer join user on user.id = user_role.UID
```

但是，由于在xml解析sql语句时会自动前后拼接，有的时候会导致语句变成这样的

```sql
select role.ID as RID, role.ROLE_NAME as ROLE_NAME, role.ROLE_DESC as ROLE_DESC, user.* from roleleft outer  join user_role on user_role.RID=role.IDleft outer join user on user.id = user_role.UID
```

left与role连接在了一起，就不再是个关键字了，导致报错。

**解决方法很简单，在换行以后敲个空格或者tab键就可以了。**



## 2 JDK版本问题

从Mybatis第四天开始的工程不得不使用JDK 12及其以上版本。

到官网下载jdk：https://www.oracle.com/java/technologies/javase/jdk12-archive-downloads.html

Idea IDE对SDK的配置参见该博客：https://blog.csdn.net/nobb111/article/details/77116259



## 3 Mysql版本问题

在更新jdk以后，查询语句也报错了，是因为客户端不支持服务器的授权协议。

```
org.apache.ibatis.exceptions.PersistenceException: 
### Error querying database.  Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: Client does not support authentication protocol requested by server; consider upgrading MySQL client
### The error may exist in org/example/dao/IUserDao.xml
### The error may involve org.example.dao.IUserDao.findAll
### The error occurred while executing a query
### Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: Client does not support authentication protocol requested by server; consider upgrading MySQL client
```

当我将pom.xml中的Mysql版本更新成8.0.19就可以正常执行了。

```xml
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.19</version>
        </dependency>
```

