#### 栈 Stack

```python
class Stack(object):
    def __init__(self):
        self.items = []

    def is_emtpy(self):
        return self.items == []

    def push(self, item):
        self.items.append(item)

    def pop(self):
        return self.items.pop()

    def peek(self):
        return self.items[-1]

    def size(self):
        return len(self.items)
```

##### 符号匹配

```python
def checker(symbol_string):
    open_symbol = "([{<"
    close_symbol = ")]}>"

    s = Stack()
    index = 0

    while index < len(symbol_string):
        symbol = symbol_string[index]
        if symbol in open_symbol:
            s.push(symbol)
        elif symbol in close_symbol:
            # 如果为空，就表示 close_symbol 出现在最前面。显然是不行的
            if s.is_emtpy():
                return False
            # 如果不为空，就表示之前已经有 open_symbol 入栈了，当前是一个 close_symbol
            # 同时还需要判断，之前最近一个 open_symbol 要和 symbol 匹配
            elif open_symbol[close_symbol.index(symbol)] == s.peek():
                s.pop()
        index = index + 1
    return s.is_emtpy() is True

```

---

#### 前序、中序和后序表达式

- 前序表达式就是运算符在前面
- 中序表达式就是运算符在中间
- 后序表达式就是运算符在后面
- 中序表达式转化给其他形式前，需要手动写成 完全括号表达式

![截屏2021-06-19 下午5.02.21](https://tva1.sinaimg.cn/large/008i3skNgy1grnonhvnblj31we08a74x.jpg)

![截屏2021-06-19 下午5.08.56](https://tva1.sinaimg.cn/large/008i3skNgy1grnouhetzij31w20c63zp.jpg)