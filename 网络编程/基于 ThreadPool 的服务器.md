#### 基于 ThreadPool 的服务器

`server.py`

```python
import socket
import time
import threading
from ThreadPool import ThreadPool


def handle_connection(stream):
    buffer: str = stream.recv(1024).decode("utf8")

    if buffer.startswith("GET / HTTP/1.1\r\n"):
        with open("hello.html", "rb") as f:
            body = f.read()
            response = b"HTTP/1.1 200 OK\r\n\r\n" + b"hello\n"
            stream.send(response)
    else:
        response = b"HTTP/1.1 404 NOT FOUND\r\n\r\n"
        stream.send(response)

    stream.close()


def run_server():
    host = "127.0.0.1"
    port = 2222

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

    s.bind((host, port), )
    s.listen(2048)
    print(f"Listen on {host}:{port} ...")
    pool = ThreadPool(4)

    while True:
        stream, addr = s.accept()
        pool.execute(target=handle_connection, args=(stream,))


if __name__ == "__main__":
    run_server()

```

`ThreadPool.py`

```python
import threading
import queue


class Worker:

    def __init__(self, idx, my_queue: queue.Queue):
        self.idx = idx
        self.queue = my_queue

        t = threading.Thread(target=self.handler)
        t.start()

    def handler(self, ):
        print(f"Worker {self.idx} starting...", )
        while True:
            job = self.queue.get()
            # print(f"Worker {self.idx} got a job; executing.", )
            job.execute()


class Job:

    def __init__(self, target, args=()):
        self.func = target
        self.args = args

    def execute(self):
        self.func(*self.args)


class ThreadPool:

    def __init__(self, size):
        self.workers = []
        self.queue = queue.Queue()

        for idx in range(0, size):
            self.workers.append(Worker(idx, self.queue))

    def execute(self, target, args=(), ):
        self.queue.put_nowait(Job(target, args))

```

