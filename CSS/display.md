- display

==基本用法 显示或者隐藏==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .div1{
            background: blue;
        }
        .div2{
            background-color: red;
            display: none;                        /*隐藏元素*/
        }
    </style>
</head>

<body>

<div class="div1">
    div1
</div>
<div class="div2">
    div2
</div>

</body>
</html>
```

- 行内元素和块级元素 转换

> block 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        span{
            display: block;     /*转换为块级元素以后 就可以使用块级元素的属性*/
            width: 10px;
        }
        p{
            display:inline；  /*块级元素 转换为 行内元素*/
        }
        
        h1{
            display:inline-block; /*行内块  是行内元素 但是有块元素的属性*/
        }
    </style>
</head>

<body>

<span>
    test
</span>

<p>
test    
</p>
    
<h1>
    test
</h1> 
    
</body>
</html>
```

