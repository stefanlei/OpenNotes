#### Python 简易聊天室

`server.py`

```python
import threading
import socket

host = "127.0.0.1"
port = 5555

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 地址复用
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server.bind((host, port))
server.listen()

clients = []
nicknames = []


def broadcast(message, source=None):
    for target in clients:
        if source:
            if target.address != source.address:
                target.stream.send(f"{source.nickname}: {message}".encode("utf8"))
        else:
            target.stream.send(f"System: {message}".encode("utf8"))


def client_close(client):
    index = clients.index(client)
    clients.remove(client)
    client.stream.close()
    nickname = nicknames[index]
    nicknames.remove(nickname)

    broadcast(f"{nickname} left the chat !")
    print(f"{nickname} left the chat !")


def handle(client):
    while True:
        try:
            message = client.stream.recv(1024).decode("utf8")
            # 连接断开的时候，操作系统会发送一个 FIN 数据包过来，所以内容是空的。
            if message == "":
                client_close(client)
                break
            print(message)
            broadcast(message, client)
        except:
            client_close(client)
            break


class Client(object):

    def __init__(self, stream, address, nickname):
        self.stream = stream
        self.address = address
        self.nickname = nickname


def run_server():
    while True:
        stream, address = server.accept()
        print(f"Connected with {address[0]}:{address[1]}")

        nickname = stream.recv(1024).decode("utf8")

        client = Client(stream, address, nickname)

        nicknames.append(nickname)
        clients.append(client)

        print(f"Nickname of the client is {nickname}!")

        stream.send("Connected to the server!".encode("utf8"))
        thread = threading.Thread(target=handle, args=(client,))
        thread.start()


if __name__ == "__main__":
    print(f"Listen on {host}:{port} ...")
    run_server()

```



`client.py`

```python
import socket
import threading

nickname = input("Choose a nickname: ")

host = "127.0.0.1"
port = 5555
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect((host, port))

client.send(nickname.encode("utf8"))


def receive():
    while True:
        try:
            message = client.recv(1024).decode("utf8")
            # 连接断开的时候，操作系统会发送一个 FIN 数据包过来，所以内容是空的。
            if message == "":
                print("System Shutdown !")
                client.close()
                break
            else:
                print(message)
        except:
            print("An error occurred!")
            client.close()
            break

    exit(0)


def write():
    while True:
        message = input()
        client.send(message.encode("utf8"))

# 读线程
receive_thread = threading.Thread(target=receive, )
receive_thread.start()

# 写线程
write_thread = threading.Thread(target=write, )
write_thread.start()

```

