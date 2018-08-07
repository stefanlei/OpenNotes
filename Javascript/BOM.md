#### window 对象

- 系统对话框 

  1.  消息框 alert() 
  2. 输入框 prompt()  返回提示框中的值

  ```
  var a = prompt('你好','默认值')
  alert(a)
  ```

  3. 确认框 confirm  ，返回 true / false

     confirm() 方法用于显示一个带有指定消息和 OK 及取消按钮的对话框。 

  ```
  <script>
      var b = confirm('你好')
      alert(b)
  </script>
  ```

  ​

- 打开窗口

  1. window.open()

  ```
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>

  </head>
  <body>
  <input type="button" value="submit" id="button">
  <script>
      function test() {
          window.open('http://www.baidu.com')    // 打开 www.baidu.com
          window.open()                          // 打开空白窗口
      }
      var button = document.getElementById('button')
      button.onclick = test
  </script>

  </body>
  </html>
  ```

- 关闭窗口

  1. window.close() 关闭窗口

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>

  </head>
  <body>
  <input type="button" value="submit" id="button">
  <script>
      function test() {
          window.close()   // 关闭窗口
      }
      var button = document.getElementById('button')
      button.onclick = test
  </script>

  </body>
  </html>
  ```

- 时间函数

  ==setTimeout() 在指定的毫秒数后调用函数或计算表达式。== 

  ==setInterval() 可按照指定的周期（以毫秒计）来调用函数或计算表达式。== 

  ==在线更新时间例子==

  ```
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>

  <div id="time"></div>
  </body>
  <script>
      function showTime() {
          var date = new Date()
          var time = date.toLocaleString()
          var div = document.getElementById('time')
          div.innerText = time
      }
      setInterval(showTime, 1)
  </script>
  </html>
  ```


#### history 对象

==history 对象是历史对象。包含用户（在浏览器窗口中）访问过的 URL。 history 对象是 window 对象的一部分，可通过 window.history 属性对 其进行访问。==

1.  history 对象的属性

   length

2.  history 对象的方法

   back()：加载 history 列表中的前一个 URL。

   forward()：加载 history 列表中的下一个 URL。 当页面第一次访问时，还没有下一个 url。 

   go()：加载 history 列表中的某个具体页面。 如：go(-1)，到上一个页面 

   ==文件 index.html==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<a href="test.html">test页面</a>
<button onclick="forward()">前进</button>
</body>

<script>
    function forward() {
        window.history.forward()
    }
</script>
</html>
```

​	==文件 test.html==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<button onclick="back()">后退</button>
</body>
<script>
    function back() {
        history.back()
    }
</script>
</html>
```

#### document 对象

> document 代表空白区域
>
> document 是 window 的属性
>
> document.title : 标题栏

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<a href="javascript:void (0)" onclick="testTitle()">test</a>
</body>
<script>
    function testTitle() {
        window.document.title = '标题'
    }
</script>
</html>
```

#### cookie 对象

> cookie 的意图：在本地客户端的磁盘上以很小的文件形式保存数据。 
>
> cookie 的应用，如：保存用户信息，下次 自动登录。 
>
> cookie 的组成，有名/值对形式的文本组成：name=value。 

==完整格式==

> name=value;[expires=date];[path=path];[domain=somewhere.com];[se cure]

```

```

