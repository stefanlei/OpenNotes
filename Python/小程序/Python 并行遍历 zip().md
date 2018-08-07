##### zip ()

- zip()函数在运算时，会以一个或多个序列做为参数，返回一个元组的列表。同时将这些序列中并排的元素配对。



```
a=[1,2,3,4,5,6,7]
b=['a','b','c','d','e','f']
for x,y in zip(a,b):
    print(x,y)
    
1 a
2 b
3 c
4 d
5 e
6 f
```

> 类似的还有 izip()   izip_longest
