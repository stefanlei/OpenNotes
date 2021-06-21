#### 双端队列 Deque

```python
class Deque(object):

    def __init__(self):
        self.items = []

    def is_empty(self):
        return self.items == []

    def add_front(self, item):
        self.items.insert(0, item)

    def add_back(self, item):
        self.items.append(item)

    def remove_front(self):
        return self.items.pop(0)

    def remove_back(self):
        return self.items.pop(-1)

    def size(self):
        return len(self.items)

```

##### 回文检测

```python
def checker(string):
    deque = Deque()

    for ch in string:
        deque.add_back(ch)

    still_equal = True

    while deque.size() >= 1:
        if deque.size() == 1:
            return still_equal
        if deque.remove_front() != deque.remove_back():
            still_equal = False
            break

    return still_equal
```

