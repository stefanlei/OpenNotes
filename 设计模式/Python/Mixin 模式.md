#### Mixin 模式

**Mixin 有助于减少系统中的重复功能及增加函数复用**

举例说明，代码来自 `Python` 标准库 `socketserver`

```python
class ThreadingTCPServer(ThreadingMixIn, TCPServer): 
  pass


# 这里面调用的 self.finish_request 等，当前类中都没有，但是不会报错。
# 因为没有直接用 ThreadingMixIn 实例化，所以不会报错。
# 子类同时继承 ThreadingMixIn 和 TCPServer ，因为 ThreadingMixIn 在前面，所以 process_request 等方法，用的是 
# ThreadingMixIn 里面的，而其他的方法用的是 TCPServer 里面的。
class ThreadingMixIn:
    """Mix-in class to handle each request in a new thread."""

    # Decides how threads will act upon termination of the
    # main process
    daemon_threads = False
    # If true, server_close() waits until all non-daemonic threads terminate.
    block_on_close = True
    # For non-daemonic threads, list of threading.Threading objects
    # used by server_close() to wait for all threads completion.
    _threads = None

    def process_request_thread(self, request, client_address):
        """Same as in BaseServer but as a thread.

        In addition, exception handling is done here.

        """
        try:
            self.finish_request(request, client_address)
        except Exception:
            self.handle_error(request, client_address)
        finally:
            self.shutdown_request(request)

    def process_request(self, request, client_address):
        """Start a new thread to process the request."""
        t = threading.Thread(target = self.process_request_thread,
                             args = (request, client_address))
        t.daemon = self.daemon_threads
        if not t.daemon and self.block_on_close:
            if self._threads is None:
                self._threads = []
            self._threads.append(t)
        t.start()

    def server_close(self):
        super().server_close()
        if self.block_on_close:
            threads = self._threads
            self._threads = None
            if threads:
                for thread in threads:
                    thread.join()
```

