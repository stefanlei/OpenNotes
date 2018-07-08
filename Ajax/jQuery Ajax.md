#### jQuery Ajax

- $.ajax()

> 使用方法 $.ajax({})

```ini
type：请求方式 

url：请求地址 url 

async：是否异步，默认是 true 表示异步 

data：发送到服务器的数据 

dataType：预期服务器返回的数据类型。如果不指定，jQuery 将自动根据 HTTP 包 MIME 信息来智能判断 ， 如果设置 json 那么就把 返回的数据当作 json 来处理

contentType：设置请求头 发送给服务器时指定的 内容类型 默认值: "application/x-www-form-urlencoded"。发送信息至服务器时内容编码类型。

success：请求成功时调用此函数 

error：请求失败时调用此函数 
```

```html

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>index</title>
    <script src="jquery-3.3.1.js" type="text/javascript"></script>
</head>
<body>

</body>
<script>
    $.ajax({                                       // 用这个方式 定义对象
        type:'get',                                // get 请求
        url:'test.json',                           // url 这里是 同域
        async:true,                                // 异步的
        success:function (data) {                  // 接收数据后的操作
            console.log(JSON.stringify(data))
        }
    })
</script>
</html>

```

- $.get()

> 这是一个简单的 GET 请求功能以取代复杂 $.ajax 。 

```html

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>index</title>
    <script src="jquery-3.3.1.js" type="text/javascript"></script>
</head>
<body>

</body>
<script>
    $.get('test.json',{name:'stefan',age:'18'},function (data) {
        console.log(data)
    })
	
    // 'dateType' 在第四个参数 
    $.get('http://iservice.itshsxt.com/restaurant/find', {},function () {

    },'json')   // 这个 json 指定 以 json 格式来处理接受到的数据
    
</script>
</html>

```

- $.post()

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>index</title>
    <script src="jquery-3.3.1.js" type="text/javascript"></script>
</head>
<body>
</body>
<script>
    
    // 'dateType' 在第四个参数 
    $.post('test.json',{name:'stefan'},function (data) {
        console.log(data)
    })
</script>
</html>

```

- 跨域访问

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>index</title>
    <script src="jquery-3.3.1.js" type="text/javascript"></script>
</head>
<body>

</body>
<script>
    $.ajax({
        url:'http://iservice.itshsxt.com/restaurant/find',
        method:'get',
        // 这里要指定 jsonp，就表示要进行跨域访问 默认以 ?callback=Jquery121423142 请求
        // 服务器端要 返回一个 这样的字符串 Jquery121423142({'name':'stefan'}) 
        dataType:'jsonp'
        success:function (data) {
            console.log(data)
        }
    })
</script>
</html>

```

- 后端代码

```python
def api(request):
    data = {'name': 'stefan', 'age': 18}
    res = '{}({})'.format(request.GET.get('callback'), data)
    return HttpResponse(res)
```

- $.getJSON()   获取同源资源  只获取 JSON 

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title></title>
    <script type="text/javascript" src="js/jquery-1.11.0.min.js" ></script>
    <script type="text/javascript">
        $.getJSON("js/cuisine_area.json",{},function(data){
            console.log(data);
        });

        $.get("js/cuisine_area.json",{},function(data){
            console.log(data);
        },"text");       // 'dateType' 在第四个参数  text 指定以text来处理接受到的参数
    </script>
</head>
<body>

</body>
</html>

```



