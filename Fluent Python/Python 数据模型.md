> Guido 对语言设计美学的深入理解让人震惊。我认识不少很不错的编程语言设计者，他们设计出来的东西确实很精彩，但是从来都不会有用户。Guido 知道如何在理论上做出一定的妥协，设计出来的语言让使用者觉得如沐春风，这真是不可多得。				
>
> ​																			—— Jim Hugunin
>
> ​											Jython 的作者，AspectJ 的作者之一， .NET DLR 架构师

*本片文章介绍的是 Python 设计哲学之一，数据模型。这是一篇软知识文章，往能有所悟。*

#### 一、如何理解数据模型？

最近我在阅读一本专门讲述 Python 语言特性的书（本文部分内容来自 **Fluent Python** 这本书），书中提到了**数据模型**这个词，数据模型是不是我们经常说的数据类型？其实不是，数据模型是对 Python 框架的描述，他规范了自身构建模块的接口，这些接口我们可以理解为是 Python 中的特殊方法，例如 `__iter__`、`__len__`、`__del__` 等。这些模块包括但不限于序列、迭代器、函数、类和上下文管理器。假如我们在讨论，拥有哪些方法和属性的对象可以称为序列，实际上我们就是在讨论序列的数据模型。

**Python 最好的品质之一是一致性，当你使用 Python 一段时间以后，你就会开始理解 Python 语言，并且能正确的猜出对你来说是全新的语言特性。**

 如果你带着其他编程语言的经历来使用 Python ，你可能会为 `len(object) ` 而不是 `object.len()` 而感到好奇，在 `Java` 语言中常常使用 `object.len()` 这种方法取得对象的长度。**当你进一步的理解这种不适感背后的强大之处的时候，你会被 Python 的设计哲学所折服，这正是建立在 Python 数据模型之上的结果，Python 数据模型的 API ，为我们使用地道的 Python 特性构建对象提供了工具，这正是我们常常说的 `Pythonic`。**

不管在哪种框架下写程序，都会花费大量的时间区实现那些会被框架本身调用的方法，Python 框架本身也不例外。当你在使用 `object[item]` 的时候，背后实际上是调用了 `object.__getitem__` 这个方法。这样的好处的是什么，这样我们就可以对自建的对象使用 `[]` 运算符，我们只需要在类当中定义 `__getitem__` 方法即可。

`__getitem__` 是 Python 的特殊方法之一，常见的特殊方法还有 `__len__`、`__iter__`、`__enter__`、`__call__` 等

这些特殊方法，能让你自己的对象实现和支持以下的语言架构，并与之交互。

- 迭代
- 集合类
- 属性访问
- 运算符重载
- 函数和方法的调用
- 对象的创建和销毁
- 字符串表示形式和格式化
- 上下文管理器

#### 二、实现自己的序列类

数据模型提供了使用 Python 语言特性的来构建对象的 API ，那么我们尝试着实现自己的序列类。

```python
# @Author   : stefanlei
# @File     : app.py


class MyList(object):
    
    def __init__(self, ):
        self.items = [str(x) for x in range(10)]
        self.len = 10

    def __getitem__(self, item):
        return self.items[item]

    def __len__(self):
        return self.len


a = MyList()

for x in a:
    print(x)
print('第一个元素', a[0])
print("长度:", len(a))

```

我们自己定义的类支持迭代，支持切片，也支持`len` 方法，这其实就是实现了 Python 序列的协议（非正式接口），我们通过数据模型实现了自定义的序列，而非子类化内置序列，这其实也是鸭子模型的一种语言，当某个类的行为看起来像鸭子（这里指代序列），那么这个类就可以说是序列。不在乎是通过子类化，还是序列协议实现的。

我们已经可以体会到通过使用特殊方法来利用 Python 数据模型的好处，作为你的类的用户，不必去记住标准操作的各式名称（“怎么得到长度？” 是 .size() 还是 .length() 还是别的什么 ？）

上面的实例中，`MyList` 类可以进行迭代和切片，切片的功能是由 `__getitem__` 提供的，迭代的功能实际上是由 `__iter__` 提供的，它返回一个可迭代对象。但是 `MyList` 类中没有 `__iter__` 方法，这是怎么回事呢？

因为如果没有实现 `__iter__` 方法， 但是实现了`__getitem__` 方法， Python 会创建一个迭代器，尝试按顺序（ 从索引 0 开始）获取元素。

现在我们尝试打印 `a` 对象

```python
class MyList(object):
    
    def __init__(self, ):
        self.items = [str(x) for x in range(10)]
        self.len = 10

    def __getitem__(self, item):
        return self.items[item]

    def __len__(self):
        return self.len

a = MyList()
print(a)
```

```sh
>> <__main__.MyList object at 0x0000015AEAFC95F8>
```

结果是一串我们不在意的内存地址，但是我们经常发现，一些 Python 内置模块，或者第三方的库，当我们直接打印某个对象的时候，输出的是我们可以看懂的信息，而不是这样的内存地址。

其实这个是可以通过特殊方法 `__str__` 来实现的，它返回一个字符串，我们可以通过为 `MyList` 类添加一个 `__str__` 特殊方法实现。

```python
# @Author   : stefanlei
# @File     : app.py

class MyList(object):

    def __init__(self, ):
        self.items = [str(x) for x in range(10)]
        self.len = 10

    def __getitem__(self, item):
        return self.items[item]

    def __len__(self):
        return self.len

    def __str__(self):
        return ','.join(self.items)


a = MyList()
print(a)
```

```sh
>> 0,1,2,3,4,5,6,7,8,9
```



#### 三、为什么 len 不是普通方法

> 以下这段话，来自中文版《流畅的Python》

我在2013年问核心开发者 RaymondHettinger 这个问题时，他用 “ Python 之禅”（https://www.python.org/doc/humor/#the-zen-of-python）里的原话回答了我：“实用胜于纯粹。”  如果 x 是一个内置类型的实例，那么 len(x)的速度会非常快。背后的原因是 CPython 会直接从一个 C 结构体里读取对象的长度，完全不会调用任何方法。获取一个集合中元素的数量是一个很常见的操作，在 str、list、memoryview 等类型上，这个操作必须高效。换句话说，len 之所以不是一个普通方法，是为了让 Python 自带的数据结构可以走后门，abs 也是同理。但是多亏了它是特殊方法，我们也可以把 len 用于自定义数据类型。这种处理方式在保持内置类型的效率和保证语言的一致性之间找到了一个平衡点，也印证了“ Python 之 ”中的另外一句话：“不能让特例特殊到开始破坏既定规则。”



#### 四、数据模型与特殊方法

数据模型描述的是对象协议，而特殊方法正是内置对象的所实现的协议，为了让我们的代码风格表现的和内置类型一样，或者说更 Python 风格的代码，我们可以使用特殊方法，而不是子类化。

对象的一个基本要求就是它得要有合理的字符串表示形式，我们可以通过 `__str__` 和 `__repr__` 来满足这个要求。`__repr__` 方便我们调试，`__str__` 适合给终端用户看。这就是数据模型中存在特殊方法 `__repr__` 和 `__str__` 的原因。

Python 中的特殊方法还有很多，这里主要讲述的还是数据模型，希望大家可以理解 Python 语言的设计哲学，以及思考如何写出更 `Pythonic` 的代码。