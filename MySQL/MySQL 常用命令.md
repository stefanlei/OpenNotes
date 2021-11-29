#### fMySQL 常用命令

##### MySQL 客户端命令

```shell
# -h 指定主机，-u 指定用户名 -p 指定密码 -P 指定端口
mysql -h 127.0.0.1 -u root -p -P 3306
```

##### 数据库操作

```mysql
# 查看所有数据库
show databases;

# 进入数据库
use dataname;

# 查看所有表
shwo tables;

# 查看表结构
desc 表名
desc table_name
```


##### 用户管理

```mysql
# 创建一个用户名和密码都为 test 的用户
create user 'test' identified by 'test';

# @后面的主机，指定这个账号可以在哪个主机上使用
create user 'test'@'主机' identified by 'password';

# 删除用户
drop user '用户名@主机';
drop user 'test';
drop user 'test@localhost';
```

##### 权限管理

```mysql
# 查看用户的权限
show grants for 'username'@'主机';
show grants for 'root'@'localhost';

# 授权
# grant 权限列表 on 数据库对象 to 用户

# 给 test 用户 select mysql 数据库的权限
grant select on mysql.* to 'test'@'localhost';  
# 给 test 用户访问 mysql.user 表的所有权限
grant all privileges on mysql.user to 'test'; 
```

##### 密码管理

```mysql
# 设置当前用户的密码   
set password = 'password';     
# 设置指定用户的密码
SET PASSWORD FOR 'jeffrey'@'localhost' = 'auth_string';    
```

##### 导入导出

```mysql
# 导出整个数据库 
# mysqldump -u 用户名 -p 数据库名 > 导出的文件名 
mysqldump -u dbuser -p dbname > dbname.sql 

# 导出一个表
# mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名 
mysqldump -u dbuser -p dbname users> dbname_users.sql 



# 导入整个数据库，先自己创建数据库,然后进入数据库中
mysql -u root -p
mysql > use 数据库
# 然后使用source命令，后面参数为脚本文件(如这里用到的.sql)
mysql>source d:/dbname.sql

# 导入数据库到某个表
mysql -uroot -p 数据库 表名 < abc.sql
```

