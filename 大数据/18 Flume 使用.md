### Flume 案例

Flume提供了很多内置的 Source  支持 Avro，exec，log4j，syslog 和 http post (body为json格式)。可以让应用程序同已有的Source直接打交道，如AvroSource，SyslogTcpSource。 如果内置的Source无法满足需要， Flume还支持自定义Source。 

#### Exec 类型的 source

==基于 Unix 系统的 command 在标准输出上生产的数据==

==exec_logger 配置文件==

```ini
# example.conf: A single-node Flume configuration

# Name the components on this agent
# 这里是定义 source channels sinks 名字
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# Describe/configure the source
# 这里是定义 source 监听的数据类型，这里检测下面文件的内容变化
a1.sources.r1.type = exec
a1.sources.r1.command = tail -F /root/logs/flume.log

# Describe the sink
# 这里是定义 sinks 输出的类型
a1.sinks.k1.type = logger

# Use a channel which buffers events in memory
# 这里是指定 channels 一些信息
a1.channels.c1.type = memory

# 这里是指管道的总容量
a1.channels.c1.capacity = 1000
# 这里是每次存和取的数量
a1.channels.c1.transactionCapacity = 100

# Bind the source and sink to the channel
# 这里是指定3者之间的连接关系
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```

##### 启动服务端

`flume-ng agent -f /root/flumeconf/exec_logger --name a1`

##### 然后执行命令

`echo 123 >> /root/logs/flume.log`

---



### avro 方式 序列化

==数据被转换成 Avro Event ，然后发送到配置的 RPC 端口上去==

如果 source 是 avro，那么传来的也必须是 avro 类型

```ini
# example.conf: A single-node Flume configuration

# Name the components on this agent
# 这里是定义 source channels sinks 名字
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# Describe/configure the source
# 这里是定义 source 监听的数据类型, 这里是 RPC 协议（avro）
a1.sources.r1.type = avro
a1.sources.r1.bind = hadoop1
a1.sources.r1.port = 55555

# Describe the sink
# 这里是定义 sinks 输出的类型
a1.sinks.k1.type = logger

# Use a channel which buffers events in memory
# 这里是指定 channels 一些信息
a1.channels.c1.type = memory

# 这里是指管道的总容量
a1.channels.c1.capacity = 1000
# 这里是每次存和取的数量
a1.channels.c1.transactionCapacity = 100

# Bind the source and sink to the channel
# 这里是指定3者之间的连接关系
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```

##### 服务的启动

`flume-ng agent -f /root/flumeconf/avro_logger --name a1`

##### 客户端启动  在（其他服务器上执行也可以，当然需要在其他服务器上 安装 flume ） 

把 `/root/zookeeper.out`里面的数据，通过 avro 的方式传递给 hadoop1 

IP 和 端口是服务器的

`flume-ng avro-client -H hadoop1 -p 55555 -F /root/zookeeper.out`

