#### CMake 用法

##### 单目录模式

```cmake
# CMake 最低版本要求
cmake_minimum_required(VERSION 3.8)
# 项目名称
project(Test)

# 添加编译器参数
set(CMAKE_CXX_FLAGS "-v")

# set 是定义变量 这里是定义 C++ 版本
set(CMAKE_CXX_STANDARD 11)
# 设置软件版本号
set (Test_VERSION_MAJOR 1)
set (Test_VERSION_MINOR 0)

# 查找当前目录下所有的源文件，并保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
# 指定生成目标
add_executable(test ${DIR_SRCS})



# 递归选择目录下的 cpp
set(SOURCE_LIST)
file(GLOB_RECURSE SOURCE_LIST ./source/*.cpp)

add_executable(test ${SOURCE_LIST})


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


# 或者使用递归查找所有源文件，并保存到 source_list
set(source_list)
file(GLOB_RECURSE source_list ./source/*.cpp)
add_executable(test ${source_list})


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

# 添加头文件搜索目录
# 它相当于g++选项中的-I参数的作用，也相当于环境变量中增加路径到CPLUS_INCLUDE_PATH变量的作用。
# include_directories(../../../thirdparty/comm/include)
include_directories(include)

# 添加需要链接的库文件目录
# 它相当于g++命令的-L选项的作用，也相当于环境变量中增加LD_LIBRARY_PATH的路径的作用。
# link_directories("/home/server/third/lib")
link_directories(directory1 directory2 ...)

# 添加库，直接是全路径
link_libraries(“/home/server/third/lib/libcommon.a”)
# 下面的例子，只有库名，cmake 会自动去所包含的目录搜索，这个目录是上面指定的
link_libraries(common)

# 传入变量
link_libraries(${RUNTIME_LIB})
# 也可以链接多个
link_libraries("/opt/libeng.so"　"/opt/libmx.so")

```

##### 指定编译器参数

```cmake
# 这样写，可以在原有的 CMAKE_CXX_FLAGS 的基础上添加，而不是覆盖
# 版本也可以在这里指定
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -v -std=c++11")
```

