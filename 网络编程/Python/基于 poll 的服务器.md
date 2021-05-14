#### 基于 poll 的服务器

```python
import select
import socket
import queue

host = "127.0.0.1"
port = 1111

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server.setblocking(False)
server.bind((host, port))
server.listen(2048)

print(f'开始监听 {host}:{port}')

message_queue = {}

# 让它不会永远阻塞住，设置个超时时间 (毫秒)
timeout = 100000

# 按位或 只读
READ = select.POLLIN | select.POLLPRI | select.POLLHUP | select.POLLERR

# 按位或 读写
READ_AND_WRITE = READ | select.POLLOUT

# 设置 poller
poller = select.poll()

# 首次注册监听事件
poller.register(server, READ)

# 文件描述符：对应的 socket 对象
fd_to_socket = {
    server.fileno(): server,
}


def handle(client, msg):
    response = b"HTTP/1.1 200 OK\r\n\r\n" + msg
    client.send(response)



while True:
    # 等待直到至少有一个套接字准备好接受处理
    print("等待下一次循环...")

    # 开启监听
    events = poller.poll(timeout)

    for fd, flag in events:
        s = fd_to_socket[fd]

        # 处理 input 事件，说明服务端可以 recv 了
        if flag & (select.POLLIN | select.POLLPRI):
            if s is server:
                connection, client_address = s.accept()
                print(f"连接来自: {client_address}")
                connection.setblocking(False)

                fd_to_socket[connection.fileno()] = connection
                poller.register(connection, READ)

                # 给这个连接一个发送数据的队列
                message_queue[connection] = queue.Queue()
            else:
                try:
                    data = s.recv(1024)
                except ConnectionResetError:
                    print(f"客户端主动断开连接 {client_address}")
                    poller.unregister(s)
                    s.close()
                    del message_queue[s]
                else:
                    if data:
                        print(f"收到消息 {data} 来自 {s.getpeername()}")
                        message_queue[s].put(data)

                        # 添加输出通道以响应。收到数据以后，修改文件描述符为可以读写的。
                        poller.modify(s, READ_AND_WRITE)
                    else:
                        # 空的返回结果表示连接断开
                        print(f"关闭连接: {s.getpeername()}")
                        poller.unregister(s)
                        s.close()
                        del message_queue[s]

        # 通道已关闭的情况
        elif flag & select.POLLHUP:
            # 客户端挂起
            print(f"关闭连接: {s.getpeername()}")

            # 停止监听该连接的输入
            poller.unregister(s)
            s.close()

        # 处理 out 事件，说明服务端可以 send 了
        elif flag & select.POLLOUT:
            # 套接字准备好发送任何数据
            try:
                next_msg = message_queue[s].get_nowait()
            except queue.Empty:
                # 无消息等待，所以我们停止检测
                print(f"队列为空: {s.getpeername()}")
                poller.modify(s, READ)
            else:

                # 这里就可以发送业务数据了
                print(f"发送消息 {next_msg} 给 {s.getpeername()}")
                handle(s, next_msg)
                
                # 如果是短连接，那么用完就可以立马断开
    						poller.unregister(client)
                client.close()
                del message_queue[client]

        # 发送错误，停止监听
        elif flag & select.POLLERR:
            print(f"发送错误: {s.getpeername()}")
            poller.unregister(s)
            s.close()
            del message_queue[s]

```

