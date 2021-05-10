#### 基于 select 的服务器

```python
import select
import socket
import queue
import ThreadPool
import time

host = "127.0.0.1"
port = 1111

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server.setblocking(False)
server.bind((host, port))
server.listen(2048)

print(f'开始监听 {host}:{port}')

read = [server, ]
write = []

message_queue = {}


def handle(client, msg):
    response = b"HTTP/1.1 200 OK\r\n\r\n" + next_msg
    client.send(response)

    # 如果是短连接，那么用完就可以里面断开
    client.send(response)
    if client in write:
        write.remove(client)
    if client in read:
        read.remove(client)
    client.close()
    del message_queue[client]


while True:
    print("等待下一次循环...")
    readable, writeable, exceptionable = select.select(read, write, read)
    for s in readable:
        if s is server:
            stream, client_addr = s.accept()
            print(f"新的连接: {client_addr}")
            stream.setblocking(0)
            read.append(stream)
            message_queue[stream] = queue.Queue()
        else:
            try:
                data = s.recv(1024)
            except ConnectionResetError:
                print(f"客户端主动断开连接 {client_addr}")
                if s in write:
                    write.remove(s)
                read.remove(s)
                s.close()
                del message_queue[s]
            else:
                if data:
                    print(f"收到消息 {data} 来自 {s.getpeername()}")
                    message_queue[s].put(data)
                    if s not in write:
                        write.append(s)
                else:
                    print(f"数据为空，关闭客户端: {client_addr}")
                    if s in write:
                        write.remove(s)
                    read.remove(s)
                    s.close()
                    del message_queue[s]

    for s in writeable:
        try:
            next_msg = message_queue.get(s).get_nowait()
        except queue.Empty:
            print(f"消息队列为空 {client_addr}")
            write.remove(s)
        else:
            # 如果是短连接，那么用完就可以立马断开
            # write.remove(s)
            # if s in read:
            #     read.remove(s)

          
            # 这里可以用来处理业务了
            print(f"发送消息 {s.getpeername()} 给 {next_msg}")
            response = b"HTTP/1.1 200 OK\r\n\r\n" + next_msg
            s.send(response)
            
            # s.close()
            # del message_queue[s]



    for s in exceptionable:
        print(f"发生异常: {s.getpeername()}")
        if s in read:
            read.remove(s)
        if s in write:
            write.remove(s)
        s.close()

        del message_queue[s]

```

