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

