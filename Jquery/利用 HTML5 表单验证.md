#### 表单验证

[参考链接](http://www.php.cn/html5-tutorial-359639.html)

http://www.runoob.com/js/js-validation-api.html

利用 HTML5的方法 setCustomValidity()  



- 原生

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
</head>
<body>

<input id="id1" type="text" required pattern="\w{8,19}">   // 这里可以写正则或者其他的
<button onclick="myFunction()">验证</button>

<p id="demo"></p>

<script>
    function myFunction() {
        var inpObj = document.getElementById("id1");
        inpObj.setCustomValidity('')           // 每次验证前，先清空 之前的信息，
        if (inpObj.checkValidity()==false) {   // checkValidity 验证是否满足验证
            inpObj.setCustomValidity('长度在 8-19之间')
            //这里就是我们设置的 setCustomValidity
            document.getElementById("demo").innerHTML = inpObj.validationMessage;
        } else {
            inpObj.setCustomValidity('正确')
            document.getElementById("demo").innerHTML = inpObj.validationMessage;
        }

    }
</script>

</body>
</html>
```

- 和 bootstrap 结合

```html
<form class='needs-validation' novalidate id="LoginForm">    // 指定了novalidate说明自定义验证
                    <div class="form-group">
                        <label for="EmailLogin">邮箱地址</label>
                        <input type="email" class="form-control" id="EmailLogin" aria-describedby="emailHelp" placeholder="Enter email" required
                               pattern="[a-zA-Z0-9]+@[a-zA-Z0-9]+\.(com|net|org|cn)">
                        <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
                        <div class="invalid-feedback" id="loginEmailFeedback">   //通过这里定义错误提示消息
                            邮箱格式错误
                        </div>
                    </div>
```

```javascript
// loginMedal 的表单验证
function loginCheck() {
    $('#loginCheck').click(function (event) {
        var LoginForm = document.getElementById('LoginForm')
        if (LoginForm.checkValidity() === false){
            event.preventDefault();            // 让提交事件失效
            event.stopPropagation();
        }
        $(LoginForm).addClass('was-validated')   // 添加了这属性后，<div class="invalid-feedback" id="loginEmailFeedback"> 这里面的提示信息就会显示
    })
}
```



