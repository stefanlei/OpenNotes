#### 基于队列的线程池

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

