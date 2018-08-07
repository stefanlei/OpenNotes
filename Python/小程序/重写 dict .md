- 字典像对象一样访问

```
class My_Dict(dict):

    def __init__(self,*args,**kwargs):
        super().__init__(*args, **kwargs)    # 如果用了 super() __init__ 里面就不要 self ，如果用 dict.__init__(self) 就需要 self

    def __getattribute__(self, item):
        value = self[item]

        if isinstance(value,dict):
            value = My_Dict(value)
        return value

a = My_Dict({'name':'stefan','d':{'f':'1'}})

print(a.d.f)               

```

