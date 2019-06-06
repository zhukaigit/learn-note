# mysql用户授权相关知识



### 一. 创建用户:

命令:`CREATE USER 'username'@'host' IDENTIFIED BY 'password';`

例子: 

```sql
CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';
```



### 二.授权:

命令:`GRANT privileges ON databasename.tablename TO 'username'@'host'`

例子: 

```sql
GRANT SELECT, INSERT ON mq.* TO 'dog'@'localhost';
```



### 三.创建用户同时授权

```sql
delete from mysql.user where User = 'csciopbu';
grant all privileges on zzxypm.* to 'csciopbu'@'%' IDENTIFIED BY 'TtCVntqgo2Z4';
```

