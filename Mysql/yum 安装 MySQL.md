# CentOS 7下yum系统源与mysql安装

https://blog.csdn.net/gsl371/article/details/78726715

关于密码长度问题 

https://www.cnblogs.com/ivictor/p/5142809.html

```
set global validate_password_policy=0; # 关闭密码复杂度检查
set global validate_password_length=4; # 设置最短密码为 4
```

设置默认编码

修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置，如下所示：  

这里 服务端编码

[mysqld]  character_set_server=utf8mb4  

init_connect='SET NAMES utf8mb4'



设置客户端编码

进入MySQL  执行  set names utf8mb4

这一步就相当于 在 my.conf 里面做了配置 配置了下面的三个参数

character_set_connection、character_set_client、character_set_results 

从实际上可以看到，当客户端连接服务器的时候，它会将自己想要的字符集名称发给mysql服务器，然后服务器就会使用这个字符集去设置character_set_client、character_set_connection、character_set_results这三个值。如cmd是用gbk，而SQLyog是用utf8. 

