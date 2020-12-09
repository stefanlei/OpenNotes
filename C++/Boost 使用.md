#### Boost 基本使用

```c++
#include <boost/regex.hpp>
#include <iostream>
#include <string>

int main()
{
    std::string line;
    boost::regex pat( "^Subject: (Re: |Aw: )*(.*)" );

    while (std::cin)
    {
        std::getline(std::cin, line);
        boost::smatch matches;
        if (boost::regex_match(line, matches, pat))
            std::cout << matches[2] << std::endl;
    }
}
```

```tex

# 定义变量
local BOOST_ROOT = "/usr/local/Cellar/boost/1.74.0" ;

# 打印消息
ECHO "Boost path is: " $(BOOST_ROOT) ;



# 目前没有发现有什么作用，删除也没有错误。
project
    : requirements 
      <include>$(BOOST_ROOT)
    ;



# 添加依赖的库和头文件 boost_dy_lib 相当于是定义的变量 后面会用到
lib boost_dy_lib
    :
    : <file>$(BOOST_ROOT)/lib/libboost_regex.a
      <file>$(BOOST_ROOT)/lib/libboost_regex.a  # 如果有多个就这样写
    :
    : <include>$(BOOST_ROOT)/include
      <include>$(BOOST_ROOT)/include            # 如果有多个就这样写
    ;



# 生成的可执行文件名称，后面是主文件和依赖库集合 boost_dy_lib 是上面定义的。
exe main
    : main.cpp
      boost_dy_lib
    ;
```



