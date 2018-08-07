- CSS 层叠样式表

==基本格式==

```html
元素名{
    属性:属性值;
    属性:属性值;
}
注意:
区分大小写
多个单词的属性 要用引号括起来
建议一行写一个样式
每一个样式结尾要用分号;结束


```

```html

内部样式要写在 <head></head>标签中<style></style>
<head>
<!--这是 html 的注释-->
<style type="text/css">
/*这是 css 的注释 设置div的背景颜色为红色*/
div {
    background-color: red;
}
</style>   
</head>
```

- 3 种样式的声明

==内联样式 内联样式是 style 属性值==

```html
通常是在当前元素使用 style 属性的方式
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div style="margin: auto">
    这里内联样式
</div>
</body>
</html>
```

==内部样式==

```html
内部样式通常写在 <head> 中的 <style> 标签里面
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        div{
            background-color: red;
        }
        /*这里是内部样式*/
    </style>
</head>
<body>
<div>

</div>
</body>
</html>
```

==外部样式==

```html
外部样式通过 <link> 标签引入进来
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="css/index.css">    <!--通过link引入css-->
</head>
<body>
<div>

</div>
</body>
</html>
```

- 多重样式优先级

==离元素最近的优先级最高==

```
优先级 
内联样式 大于 内部样式 大于 外部样式
如果有相同的样式 根据优先级选择 如果不相同 同时保留
```

- 强制优先级

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style type="text/css">
        div{
            color:red !important;       /*添加!important后优先级高于一切*/
        }
    </style>

</head>
<body>
<div>
    文字                              <!--红色-->
</div>
</body>
</html>
```



- 选择器

==通用选择器==

```
格式
* {
    属性:属性值;
    
}

<p>
这是段落      所有的标签都会被应用 *{ } 里面的格式
</p>
```

==类选择器==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="css/index.css">
    <style type="text/css">  <!--这里是类选择器-->
        .test{
            font-size: px;
        }
    </style>
</head>
<body>
<div class="test">
这里是 test 样式
</div>
</body>
</html>
```

==id 选择器==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="css/index.css">
    <style type="text/css">              <!--这里是 id 选择器  前面需要加 井 号-->
        #div1{ 
            font-size: 10px;
        }
    </style>
</head>
<body>
<div id="div1">                          <!--需要给元素设置 id 属性-->
    应用了 div1 样式
</div>
</body>
</html>
```

==属性选择器==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="css/index.css">
    <style type="text/css">              <!--这里是 属性选择器 有name属性的就会被应用 -->
        [name]{
        font-size: 40px;
        color: red;
    }
    </style>
</head>
<body>
<p name="p1" class="a">testddd</p>     
</body>
</html>
```

==分组选择器 混用==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="css/index.css">
    <style type="text/css">              <!--同时使用多个选择器-->
        p,.test{
        font-size: 50px;
    }
    </style>
</head>
<body>
<p name="p1" class="a">testddd</p>
</body>
</html>
```

==选择器的优先级==

> 一般而言，选择器越特殊，它的优先级越高。也就是选择器指向的越准确， 它的优先级就越高。 

```
元素：1 
类：10 
id：100 
内联样式：1000 
比如：  .polaris span {color:red;}的选择器优先级是 10 + 1 也就是 11； 而 .polaris 的优先级是 10
```

- 强制优先级

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style type="text/css">
        div{
            color:red !important;             
        }
    </style>

</head>
<body>
<div>
    文字
</div>
</body>
</html>
```



- 组合选择器

==后代选择器 所有的后代==

> 用于选择指定标签元素下的所有后辈元素 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="css/index.css">
    <style type="text/css">                         <!--会把food下所有的li都设置成blue-->
        .food li{                                 
            color: blue;
        }
    </style>
</head>
<body>

<ul class="food">
    <li>水果
        <ul>
            <li>苹果</li>
            <li>西瓜</li>
            <li>栗子</li>
        </ul>
    </li>
    <li>蔬菜
        <ul>
            <li>白菜</li>
            <li>油菜</li>
        </ul>
    </li>
</ul>

</body>
</html>
```

==子元素选择器==

> 用于选择指定标签元素的第一代子元素。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="css/index.css">
    <style type="text/css">         <!--子元素选择器  strong 为 h1 的直接子元素-->  
        h1 > strong{
            color: red;
        }
    </style>
</head>
<body>

<h1>
    This is
    <strong>
        very               红色
    </strong>
    <strong>
        very               红色
    </strong>
    important.
</h1>
<h1>This is <em>really <strong>very</strong></em> important.</h1>
</body>
</html>
```

==相邻兄弟选择器==

> 只选择相邻的那些元素 个数不限

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style type="text/css">
        p + p{                         /*相邻兄弟选择器*/
            color: red;
        }
    </style>

</head>
<body>
<p>1111</p>
<p>2222</p>                  <!--2222是红色-->
<h1>hhhh</h1>
<p>3333</p>
</body>
</html>
```

==普通兄弟选择器==

> 选择有相同父元素的两个元素中，第一个元素后所有的第二个元素。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style type="text/css">
        p ~ p{                         /*普通兄弟选择器*/
            color: red;
        }
    </style>

</head>
<body>
<p>1111</p>
<p>2222</p>                  <!--2222是红色-->
<h1>hhhh</h1>
<p>3333</p>                  <!--3333是红色-->
</body>
</html>
```

