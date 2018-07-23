参考链接 https://www.cnblogs.com/eager/p/7133270.html

参考链接 https://www.cnblogs.com/hailexuexi/p/6708110.html


- 获取

被选择的 他们的值 来自于 value 里面 单选框和复选框也是

```html

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>index</title>
    <script src="jquery-3.3.1.js" type="text/javascript"></script>
</head>
<body>
<select name="select" id="">
    <option value="0">test0</option>
    <option value="1">test1</option>
    <option value="2">test2</option>
    <option value="3">test3</option>
</select>

</body>

<script>
    // 获取被选中项的 value    // var value = $('select option:selected').val() 
    var selected = $('select').val()    
    console.log(selected)

    // 获取被选中项目的 text  // var value = $('select option:selected').text()
    var selected = $('select').find('option:selected').text()
    console.log(selected)
</script>

</html>

```

- 赋值

```html

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>index</title>
    <script src="jquery-3.3.1.js" type="text/javascript"></script>
</head>
<body>
<select name="select" id="">
    <option value="0">test0</option>
    <option value="1">test1</option>
    <option value="2">test2</option>
    <option value="3">test3</option>
</select>

</body>

<script>
    $('select').val('3')     // 把value为3的option设置为选中
    $('select option:selected').text('aaaaa')  // 把被选中项目的 text 设置为 aaaaa
    console.log($('select option:selected').text())

</script>

</html>

```

