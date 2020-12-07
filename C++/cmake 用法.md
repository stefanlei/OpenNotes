#### CMake 用法

##### 单目录模式

```cmake
# CMake 最低版本要求
cmake_minimum_required(VERSION 3.8)
# 项目名称
project(Test)


# set 是定义变量 这里是定义 C++ 版本
set(CMAKE_CXX_STANDARD 11)
# 设置软件版本号
set (Test_VERSION_MAJOR 1)
set (Test_VERSION_MINOR 0)

# 查找当前目录下所有的源文件，并保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
# 指定生成目标
add_executable(test ${DIR_SRCS})


```

---



##### 多目录模式

**主文件**

```cmake
# ~/VSCodeProjects/cmake_test/CMakeLists.txt

# CMake 最低版本要求
cmake_minimum_required(VERSION 3.8)
# 项目名称 随便写
project(Test)


# set 是定义变量 这里是定义 C++ 版本
set(CMAKE_CXX_STANDARD 11)
# 设置软件版本号
set (Test_VERSION_MAJOR 1)
set (Test_VERSION_MINOR 0)


# 查找当前目录下所有的源文件，并保存到 DIR_SRCS 变量，变量名称随便
aux_source_directory(. DIR_SRCS)
# 指定生成目标 test 是生成的可执行文件的名称
add_executable(test ${DIR_SRCS})


# 添加链接库, test 是项目名称， SayHello 是链接库的名称，这个名称需要在其他目录的 CMakeLists.txt 里面定义
target_link_libraries(test SayHello)
# 添加子目录, src 是其他目录的名称
add_subdirectory(src)

```

**其他文件**

```cmake
# ~/VSCodeProjects/cmake_test/src/CMakeLists.txt

# 查找当前目录下，所有源文件
aux_source_directory(. DIR_LIB_SRCS)

# 这里定义的 SayHello ，会在主文件中用到。
add_library(SayHello ${DIR_LIB_SRCS})
```

