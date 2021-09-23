#### 服务端

```python
import socketserver
import sys
import socket
import struct
from typing import Tuple
import select
import selectors

default_select = selectors.DefaultSelector()


class RequestHandle(socketserver.BaseRequestHandler):
    cache = dict()

    def get_ipv4(self, hostname):
        ipv4 = self.cache.get(hostname)
        if not ipv4:
            ipv4 = socket.gethostbyname(hostname)
        return ipv4

    def setup(self) -> None:
        super().setup()

    def handle(self) -> None:
        version, n_methods = struct.unpack("!BB", self.request.recv(2))

        if version != 5:
            return

            # 客户端支持的认证方法
        methods_list = []
        for item in range(n_methods):
            methods_list.append(struct.unpack("!B", self.request.recv(1)))

        # 回复认证方法，不认证
        self.request.send(struct.pack("!BB", 5, 0))

        # 客户端返回的消息
        version, cmd, _, address_type = struct.unpack("!BBBB", self.request.recv(4))

        # ipv4 地址
        if address_type == 1:
            dst_addr = socket.inet_ntoa(self.request.recv(4))
            dst_port = struct.unpack(">h", self.request.recv(2))[0]

            print(f"Connect {dst_addr}:{dst_port}", file=sys.stderr)

            remote = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            remote.connect((dst_addr, dst_port))
            local_addr, local_port = remote.getsockname()

            local_addr = struct.unpack("!I", socket.inet_aton(local_addr))[0]
            reply = struct.pack("!BBBBIH", version, 0, 0, 1, local_addr, local_port)
            self.request.send(reply)
            self.exchange_loop(self.request, remote)
        # 域名
        elif address_type == 3:
            addr_len = struct.unpack("!B", self.request.recv(1))[0]
            hostname = self.request.recv(addr_len)
            ipv4_addr = self.get_ipv4(hostname)
            dst_port = struct.unpack(">h", self.request.recv(2))[0]

            print(f"Connect {hostname.decode()}:{dst_port}", file=sys.stderr)

            remote = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            remote.connect((ipv4_addr, dst_port))
            local_addr, local_port = remote.getsockname()

            local_addr = struct.unpack("!I", socket.inet_aton(local_addr))[0]
            reply = struct.pack("!BBBBIH", version, 0, 0, 1, local_addr, local_port)
            self.request.send(reply)
            self.exchange_loop(self.request, remote)

    # selector 抽象层
    def exchange_loop(self, client, remote):
        default_select.register(client, selectors.EVENT_READ, )
        default_select.register(remote, selectors.EVENT_READ, )

        while True:
            for key, mask in default_select.select(timeout=2):
                if client is key.fileobj:
                    data = client.recv(4096)
                    if remote.send(data) <= 0:
                        break
                if remote is key.fileobj:
                    data = remote.recv(4096)
                    if client.send(data) <= 0:
                        break

    # 原生 select
    def exchange_loop_select(self, client, remote):

        while True:

            # wait until client or remote is available for read
            r, w, e = select.select([client, remote], [], [])

            if client in r:
                data = client.recv(4096)
                if remote.send(data) <= 0:
                    break

            if remote in r:
                data = remote.recv(4096)
                if client.send(data) <= 0:
                    break

    def finish(self) -> None:
        super().finish()


class Socks5Server(socketserver.ThreadingTCPServer):
    allow_reuse_address = True

    def process_request(self, request: bytes, client_address: Tuple[str, int]) -> None:
        super().process_request(request, client_address)
        # print(f"Connect from {client_address[0]}:{client_address[1]}", file=sys.stderr)


if __name__ == '__main__':
    host = "127.0.0.1"
    port = 1081
    print(f"listen on {host}:{port}", file=sys.stderr)
    server = Socks5Server((host, port), RequestHandle)
    server.serve_forever()

```

