#### 元素节点的操作

- 获取节点

> 通过 id 获取

```html
//getElementById() 据 id 获取 dom 对象 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <input type="text" value="this is a text" id="i1">
    <button onclick="getById()">用id获取元素</button>
</body>
<script>
    function getById() {
        var el = document.getElementById('i1')    // 这里通过 id 获取元素，如果有多个，以第一个为准
        console.log(el.value)
    }
</script>
</html>
```

> 通过 name 属性获取节点数组

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <input type="checkbox" name="check" value="play">玩
    <input type="checkbox" name="check" value="work">工作
    <button onclick="getByName()">通过 name 获取</button>
</body>
<script>
    function getByName() {
        var els = document.getElementsByName('check')     // 因为有多个 check 所以返回数组
        for (var i =0 ;i<els.length;i++){
            if(els[i].checked){
                console.log(els[i].value)
            }
        }
    }
</script>
</html>
```

> 通过 class 属性获取节点数组

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <input type="checkbox" name="check" value="play" class="a">玩
    <input type="checkbox" name="check" value="work" class="a">工作
    <button onclick="getByClass()">通过 name 获取</button>
</body>
<script>
    function getByClass() {
        var els = document.getElementsByClassName('a')   // 通过 class 获取节点数组
        for (var i =0 ;i<els.length;i++){
            if(els[i].checked){
                console.log(els[i].value)
            }
        }
    }
</script>
</html>
```

> 通过 tag （标签名）获取节点数组

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <input type="checkbox" name="check" value="play" class="a">玩
    <input type="checkbox" name="check" value="work" class="a">工作
    <button onclick="getByTag()">通过 name 获取</button>
</body>
<script>
    function getByTag() {
        var els = document.getElementsByTagName('input')
        for (var i = 0;i < els.length;i++){
            console.log(els[i])
        }
    }
</script>
</html>
```

- 添加节点

> document.write()  在 页面写入文字

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
</body>
<script>
    document.write('<h1>Hello</h1>')
</script>
</html>
```

> 创建添加

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="button" value="添加段落" onclick="addPara()">&nbsp;
<input type="button" value="添加图片" onclick="addImg()">&nbsp;
<div id="my_div" style="position: absolute;top: 0"></div>
<select name="music" id="music">
    <option value="1">不能说的秘密</option>
    <option value="2">安静</option>
</select>
</body>
<script>
    function addPara() {
        // 得到容器
        var el = document.getElementById('my_div')
        // 创建一个 p 元素节点
        var p = document.createElement('p')
        // 创建一个文本节点
        var ptxt = document.createTextNode('今天会下雨')
        // 将文本节点添加到 p 元素节点中
        p.appendChild(ptxt)
        el.appendChild(p)

        // 第二种方式
        // 创建一个p元素节点
        var p2 = document.createElement('p')
        p2.innerText = '这是第二种方式'
        el.appendChild(p2)

        // 第三种方式
        var p3 = "<p>这是第三种方式</p>"
        el.innerHTML = el.innerHTML + p3
    }

    function addImg() {
        var mydiv = document.getElementById('my_div')
        // 第一种方式
        var img = document.createElement('img')
        img.src = 'https://www.baidu.com/img/bd_logo1.png'
        mydiv.appendChild(img)
        
        // 第二种方式
        var img2 = document.createElement('img')
        img2.setAttribute('src', 'https://www.baidu.com/img/bd_logo1.png')
        img2.setAttribute('width','100%')
        mydiv.appendChild(img2)
        
        //第三种方式
        var img3 = "<img src='https://www.baidu.com/img/bd_logo1.png'>"
        mydiv.innerHTML += img3
    }
    
    function addOption() {
        var sel = document.getElementById('music')
        // 第一种方式
        var opt = document.createElement('option')
        opt.text = '测试'
        opt.value = 3
        sel.appendChild(opt)

        // 第二种方式
        var opt2 = document.createElement('option')
        opt2.text = '测试2'
        opt2.value = 4
        sel.options.add(opt2)

        // 第三种方式
        var opt3 = '<option value="5">测试3</option>'
        sel.innerHTML += opt3
        
        // 第四种方式
        var opt4 = new Option("星晴",8);  // new Option(text, value)
	    sel.appendChild(opt4);
    }
</script>
</html>
```

- 向前向后插入

==向前和向后 都是这个函数 insertNode()==

 ```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<span id="music1">七里香</span>
<span id="music2">晴天</span>
<br>
<button onclick="insertNode()">添加到节点前面</button>
<button onclick="addNode()">添加到节点后面</button>

</body>
<script>
    function insertNode() {
        // 获取元素
        var music1 = document.getElementById('music1')

        // 新建一个节点
        var span = document.createElement('span')
        span.innerHTML = '<h1>Hello</h1>'

        // 找到父元素
        var parent = music1.parentNode;

        // 把 span 插入到指定元素前面 insertBefore(要插入的元素，参考元素)
        // 通过父元素使用
        parent.insertBefore(span, music1)

        // 或者这样  就是在文档中的位置关系
        document.body.insertBefore(span, music1)
    }

    function addNode() {
        // 获取元素
        var music1 = document.getElementById('music1')

        // 新建一个节点
        var span = document.createElement('span')
        span.innerHTML = '<h1>Hello</h1>'

        // 找到父元素
        var parent = music1.parentNode;

        // 通过父节点，把子元素加入到后面  nextSibling 是指下一个元素
        parent.insertBefore(span, music1.nextSibling)
    }
</script>
</html>
 ```

- 替换

==parent.replaceChild(newNode, music)==

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<span id="music">七里香</span>
<br>
<button onclick="testreplace()">替换</button>
</body>


<script>
    function testreplace() {
        // 获取需要替换的元素
        var music = document.getElementById('music')

        // 创建新节点
        var newNode = document.createElement('h1')
        newNode.innerHTML = 'Hello'

        // 找到 music 的父节点
        var parent = music.parentElement

        // 通过父节点替换
        parent.replaceChild(newNode, music)
    }
</script>
</html>
```

- 复制

==深复制和浅复制==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul id="music">
    <li>以父之名</li>
    <li>七里香</li>
</ul>
<button onclick="clone()">复制</button>

<div id="cloneList"></div>

</body>

<script>
    function clone() {
        // 获取需要克隆的节点
        var ul = document.getElementById('music')

        // 克隆对象
        var clone = ul.cloneNode(false)  // 浅复制  只复制第一层的标签 没有值
        var clone2 = ul.cloneNode(true)  // 深复制 复制所以的元素和值

        var mydiv = document.getElementById('cloneList')
        mydiv.appendChild(clone2)      // 深度复制
    }
</script>
</html>
```

- 删除元素中的子节点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul id="music">
    <li id="li1">以父之名</li>
    <li>七里香</li>
</ul>
<button onclick="del()">删除节点</button>
</body>
    
<script>
function del() {
    var li1 = document.getElementById('li1')
    var parent = li1.parentNode
    parent.removeChild(li1)
}
</script>
    
</html>
```

#### 节点属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>


<h1 id="h1">h1 <span>span</span></h1>

</body>
<script>
    var h = document.getElementById('h1')
    // 获取当前元素的第一个子节点
    console.log(h.firstChild)

    // 获取当前元素的最后一个子节点
    console.log(h.lastChild)

    // 获取文本内容
    console.log(h.innerText)

    // 获取 HTML 标签
    console.log(h.innerHTML)

    // 获取当前节点的后一个同级节点
    console.log(h.firstChild.nextSibling)

    // 获取当前节点的前一个同级节点
    console.log(h.lastChild.previousSibling)
    

</script>
</html>
```

- 操作属性

==在JS中有两种设置自定义属性的的方式：==

==原则是 如果两种方式都有的话 那么就使用自己的，如果一方不存在 就可以混用。==

```html
// 第一种是 xxx.index:是把当前元素当做一个普通对象，为其设置一个属性名（和页面中HTML没有关系）。
// 第二种是 xxx.setAttribute：是把元素当做特殊元素来处理，设置的自定义属性是和页面结构中的DOM元素映射在一起的。

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<input type="text" value="初始值" id="text">

</body>
<script>
    // 通过获取元素对象来操作属性 这里其实是 给对象 text 赋予了属性
    var text = document.getElementById('text')
    console.log(text.value)
    text.value = '新的值'
    console.log(text.value)

    // 通过方法来操作属性
    // 获取
    console.log('-----------------------------------')
    console.log(text.getAttribute('value'))

    // 设置 这里会影响 html 
    text.setAttribute('value','aaaaaaaa')
    console.log(text.value)
</script>
</html>
```

