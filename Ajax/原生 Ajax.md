- 原生 Ajax,    get 

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
    
    var xhr = new XMLHttpRequest()               // 实例化一个对象
    xhr.open('get','cuisine_area.json',false)    // 定义请求  open(method, url, true/false)
    xhr.send(null)                               // 发送请求  如果为空  一定要写 null
    console.log(xhr.responseText)                // xhr.responseText 响应的参数
    
</script>
</html>

```

- post 

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
    var xhr = new XMLHttpRequest()
    xhr.open('post','cuisine_area.json',false)    // post 请求
    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded') // 可以加上请求头
    xhr.send('name=Lee&age=100');               // 这里带参数，也可以不带参数
    console.log(xhr.responseText)
</script>
</html>

```

- 同步异步

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
    var xhr = new XMLHttpRequest()
    xhr.open('get','cuisine_area.json',true)   // 异步的send后，立马去做其他的事情，不管有没有响应
    xhr.onreadystatechange = function (ev) {
        if(xhr.readyState == 4){
            if(xhr.status == 200){
                alert(xhr.responseText)
            }
        }
    }
    xhr.send(null)
</script>
</html>

```

- 其他

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
    var xhr = new XMLHttpRequest()
    url = 'cuisine_area.json'+encodeURIComponent('中文')    // 可以对中文进行 url 编码传输
    xhr.open('get',url,false)
    xhr.send(null)
</script>
</html>

```

