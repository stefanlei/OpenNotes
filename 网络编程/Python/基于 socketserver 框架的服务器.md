#### 基于 socketserver

`socketserver `模块是一个创键网络服务器的框架。它定义了通过 TCP，UDP，Unix 流和 Unix 数据报处理同步网络请求（服务器请求处理程序阻塞，直到请求完成）的类。它还提供了混合类，可以轻松转换服务器，为每个请求使用单独的线程或进程

`socketserver` 是标准库

##### 服务器类型

`socketserver` 定义了 5 种不同的服务器类

- `BaseServer` 定义了 API，但不能直接使用和实例化
- `TCPServer` 使用 TCP/IP 套接字进行通信
- `UDPServer` 使用数据报套接字
- `UnixStreamServer` 使用 Unix 域流套接字
- `UnixDatagramServer` 使用 Unix 域数据报套接字



##### 服务器对象

要构造服务器，请向其传递一个侦听请求的地址和一个请求处理程序类，所以我们需要定义一个处理请求的 `handle` 类，可以继承`BaseRequestHandler`

```python
class EchoRequestHandle(socketserver.BaseRequestHandler):

    def setup(self):
        # 这里可以做一些其他资源初始化的动作
        print("setup")

    def handle(self):
        data = self.request.recv(1024)
        # 在这里处理请求，负责编译请求，处理数据，发送响应
        self.request.send(data)

    def finish(self):
        # 这里做一些 setup 里面资源释放的动作
        print("finish")
```



##### 回显服务器例子 (多路复用 单线程)

```python
import logging
import sys
import socketserver
import time


class EchoRequestHandle(socketserver.BaseRequestHandler):

    def setup(self):
        # 这里可以做一些其他资源初始化的动作
        print("setup")

    def handle(self):
        data = self.request.recv(1024)
        # 在这里处理请求，负责编译请求，处理数据，发送响应
        time.sleep(50)
        self.request.send(data)

    def finish(self):
        # 这里做一些 setup 里面资源释放的动作
        print("finish")


class EchoServer(socketserver.TCPServer):
    """
    TCPServer 里面有很多方法，根据需求重写方法。
    """

    def __init__(self, server_address, handler_class=EchoRequestHandle):
        super().__init__(server_address, EchoRequestHandle)

    def get_request(self):
        client, addr = self.socket.accept()
        return client, addr

    def serve_forever(self, poll_interval=0.5):
        # 这里就是一个无限循环，里面是 select.poll 。
        super().serve_forever(poll_interval)


if __name__ == '__main__':
    host = "127.0.0.1"
    port = 1111

    server = EchoServer((host, port), EchoRequestHandle)
    print(f"Listen on {host}:{port} ...")
    server.serve_forever()

```

##### 回显服务器例子 (多路复用 多线程)

```python
import logging
import sys
import socketserver
import time


class EchoRequestHandle(socketserver.BaseRequestHandler):

    def setup(self):
        # 这里可以做一些其他资源初始化的动作
        print("setup")

    def handle(self):
        data = self.request.recv(1024)
        # 在这里处理请求，负责编译请求，处理数据，发送响应
        time.sleep(50)
        self.request.send(data)

    def finish(self):
        # 这里做一些 setup 里面资源释放的动作
        print("finish")


# 实际上就是 ThreadingTCPServer 里面重写了 process_request 方法。
# serve_forever 里面会调用 process_request 方法
class EchoThreadServer(socketserver.ThreadingTCPServer):
    def __init__(self, server_address, handler_class=EchoRequestHandle):
        super().__init__(server_address, EchoRequestHandle)

    def get_request(self):
        client, addr = self.socket.accept()
        return client, addr

    def serve_forever(self, poll_interval=0.5):
        # 这里就是一个无限循环，里面是 select.poll 。
        super().serve_forever(poll_interval)


if __name__ == '__main__':
    host = "127.0.0.1"
    port = 1111

    server = EchoThreadServer((host, port), EchoRequestHandle)
    print(f"Listen on {host}:{port} ...")
    server.serve_forever()

```

