#### g++ 简单用法

```sh
# 目录结构
➜  cpp_test_lib tree
.
├── hello.cpp
├── hello.h
└── main.cpp
```

`hello.cpp`

```cpp
#include <iostream>
using namespace std;

void SayHello(){
    cout << "Hello World" << endl;
}
```

`hello.h`

```cpp
#ifndef _HELLO_H
#define _HELLO_H
extern void SayHello();
#endif
```

`main.cpp`

```cpp
#include "hello.h"

int main(){
    SayHello();
}
```

##### 编译命令

```sh
g++ main.cpp hello.cpp -o main
```

---



#### 静态库

参考链接 https://www.cnblogs.com/codingmengmeng/p/6046481.html

参考链接 https://blog.csdn.net/seanwang_25/article/details/20702751

##### 生成目标文件

1. `g++ -c hello.cpp` 输出 hello.o

##### 生成静态库

1. `ar crv libhello.a hello.o `  libxxxxx.a 这是固定格式 输出 libhello.a

##### 第三方使用静态库

1. `g++ main.cpp libhello.a -o main` 

**使用静态库后，main 内部包含了所有需要的文件，main 可以在任意目录下执行**

---



#### 动态库

##### 生成目标文件

1. `g++ -c hello.cpp` 输出 hello.o

##### 生成动态库

1. `g++ -shared -fPIC -o libhello.so hello.o`

##### 第三方使用动态库

1. `g++ main.cpp -L. -lhello -o main`

   或者 `g++ main.cpp libhello.so -o main` 给库的完整路径

`-L. `标记告诉 g++ 动态库库位于当前目录，使用`-lhello`标记来告诉 g++ 驱动程序在连接阶段引用共享函数库 `hello.so`



**使用动态库后，main 依赖于动态库的位置，所有需要保证动态库，存在于上面指定的路径中**

