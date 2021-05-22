#### 基于 selector 抽象层的服务器

```python
import socket
import selectors

my_sel = selectors.DefaultSelector()
keep_running = True


def read(connection, m):
    global keep_running
    try:
        data = connection.recv(1024)
        if data:
            print(f"received {data}")
            connection.sendall(data)
        else:
            print("closing")
            my_sel.unregister(connection)
            connection.close()
            keep_running = False
    except ConnectionResetError as e:
        print("客户端主动关闭连接")
        my_sel.unregister(connection)
        connection.close()
        keep_running = False


def accept(s, m):
    new_connection, addr = s.accept()
    new_connection.setblocking(False)
    my_sel.register(new_connection, selectors.EVENT_READ, read)


server_address = ("127.0.0.1", 1111)
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server.setblocking(False)
server.bind(server_address)
server.listen(5)

print(f"Listen on {server_address}")

my_sel.register(server, selectors.EVENT_READ, accept)

while keep_running:
    for key, mask in my_sel.select(timeout=1):
        callback = key.data
        callback(key.fileobj, mask)

print("shutting down")
my_sel.close()

```

