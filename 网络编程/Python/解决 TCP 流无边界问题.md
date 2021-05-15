#### 解决 TCP 流无边界问题

`server.py`

**思路就是，需要自己定义消息格式，消息头+消息正文，消息头包括（版本，消息正文长度，其他数据），消息头长度要求固定。**

**所以需要在协议封装之前就定义好公共的消息格式，给客户端和服务端使用**

```python
import socket
import time
import threading
from ThreadPool import ThreadPool
import struct

headerSize = 12


def handle_connection(stream):
    for body in parser(stream):
        print(body)


def parser(stream):
    buffer = bytes()
    while True:
        data = stream.recv(3)
        if data:
            buffer = buffer + data
            while True:

                # 说明还没有读取到完整的消息头
                if len(buffer) < headerSize:
                    # print(f"数据包（{len(buffer)}Byte）小于消息头部长度，跳出小循环")
                    break

                # 读取消息头
                headerPacket = struct.unpack("!3I", buffer[:headerSize])

                bodySize = headerPacket[1]

                # 说明还没有读取到完整的消息 完整的消息（header + body)
                if len(buffer) < headerSize + bodySize:
                    # print(f"数据包（{len(buffer)} Byte）不完整（总共 {headerSize + bodySize} Byte），跳出小循环")
                    break

                # 读取消息正文内容
                body = buffer[headerSize:headerSize + bodySize]

                yield body

                # 说明上一次获取的数据，有一部分是下一个消息的一部分。
                # 所以把这部分数据，放到 buffer 最前面，下次大循环还能用。
                buffer = buffer[headerSize + bodySize:]


def run_server():
    host = "127.0.0.1"
    port = 1111

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

    s.bind((host, port), )
    s.listen(1)
    print(f"Listen on {host}:{port} ...")
    pool = ThreadPool(4)

    while True:
        stream, addr = s.accept()
        pool.execute(target=handle_connection, args=(stream,))


if __name__ == "__main__":
    run_server()

```



`client.py`

```python
# Python Version:3.7.0
import socket
import time
import struct
import json

host = "127.0.0.1"
port = 1111

ADDR = (host, port)

if __name__ == '__main__':
    client = socket.socket()
    client.connect(ADDR)

    # 正常数据包定义
    ver = 1
    body = json.dumps(dict(hello="world"))
    print(body)
    cmd = 101
    header = [ver, len(body), cmd]
    headPack = struct.pack("!3I", *header)
    sendData1 = headPack + body.encode()

    # 分包数据定义
    ver = 2
    body = json.dumps(dict(hello="world2"))
    print(body)
    cmd = 102
    header = [ver, len(body), cmd]
    headPack = struct.pack("!3I", *header)
    sendData2_1 = headPack + body[:2].encode()
    sendData2_2 = body[2:].encode()

    # 粘包数据定义
    ver = 3
    body1 = json.dumps(dict(hello="world3"))
    print(body1)
    cmd = 103
    header = [ver, len(body1), cmd]
    headPack1 = struct.pack("!3I", *header)

    ver = 4
    body2 = json.dumps(dict(hello="world4"))
    print(body2)
    cmd = 104
    header = [ver, len(body2), cmd]
    headPack2 = struct.pack("!3I", *header)

    sendData3 = headPack1 + body1.encode() + headPack2 + body2.encode()

    # 正常数据包
    client.send(sendData1)
    time.sleep(3)

    # 分包测试
    client.send(sendData2_1)
    time.sleep(0.2)
    client.send(sendData2_2)
    time.sleep(3)

    # 粘包测试
    client.send(sendData3)
    time.sleep(3)
    client.close()

```

