##### Boost 基本使用

```she
tree 

.
├── Jamroot.jam
├── include
│   └── hello.h
├── main.cpp
└── source
    └── hello.cpp
```

`hello.cpp`

```cpp
#include <iostream>
void sayHello(){
    std::cout << "Hello" << std::endl;
}
```

`main.cpp`

```cpp
#include <iostream>
#include <string>
#include <hello.h>

int main()
{
    sayHello();
}
```

```jamroot

# 定义变量
local BOOST_ROOT = "/usr/local/Cellar/boost/1.74.0" ;
# 打印消息
ECHO "Boost path is: " $(BOOST_ROOT) ;

# 也可以用 project，也可以指定 <include>
# project      
#    : requirements
#      <include>./include
#    ;

# 生成一个动态库 指定库名和所依赖的 cpp
lib sayHello
    : ./source/hello.cpp
    ;

# 生成一个可执行文件，指定依赖的头文件和 cpp
exe test
    : ./source/hello.cpp main.cpp
    : <include>./include
    ;
```

---

##### 使用 project 来管理

```cpp
tree 
  
.
├── Jamroot
├── application
│   ├── app1
│   │   ├── Jamfile.v2
│   │   ├── include
│   │   └── source
│   │       └── hi.cpp
│   └── app2
│       ├── Jamfile.v2
│       ├── include
│       │   └── hello.h
│       └── source
│           └── hello.cpp
└── main.cpp
```

`app1/Jamfile.v2`

```cpp

project
    : requirements
      <include>./include
    ;

lib hi
    : ./source/hi.cpp
    ;
```

`app2/Jamfile.v2`

```cpp
project
    : requirements
      <include>./include
    ;

lib hello
    : ./source/hello.cpp
    ;
```

`Jamroot`

