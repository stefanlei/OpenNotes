##### make 使用 

编写 makefile



```txt

├── header
│   └── coordin.h
├── makefile
└── source
    ├── coordin.cpp
    └── main.cpp
```

##### 普通方式

```makefile

main:main.o coordin.o
	g++ -o main main.o coordin.o
main.o:source/main.cpp
	g++ -c source/main.cpp
coordin.o:source/coordin.cpp
	g++ -c source/coordin.cpp



.PHONY:clean
clean:
	rm -rf *.o
```

##### 使用通配符的方式

```makefile
#获取.cpp文件 这里可以指定源文件在哪个目录
SrcFiles=$(wildcard source/*.cpp)
#使用替换函数获取.o文件
ObjFiles=$(patsubst %.cpp,%.o,$(SrcFiles))


#生成的可执行文件
all:main
#目标文件依赖于.o文件
main:$(ObjFiles)
	g++ -o $@  $(SrcFiles)
	
#.o文件依赖于.cpp文件，通配使用，一条就够
%.o:%.cpp
	g++ -c  $<

.PHONY:clean
clean:
	rm -rf *.o
	rm -rf main
```

