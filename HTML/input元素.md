#### input

- text 文本框

```html
<input type='text' name='email'/>  
```

- button 按钮

```html
<input type='button' value='click me' onclick='msg()'/>
```

- checkbox 复选框

==name 名称相同的为一组==

```html
<input type='checkbox' name='city' value='南昌'/>
<input type='checkbox' name='city' value='上海'/>
```

- file 文件

发送文件时候 表单里面 enctype 必须为  multipart/form-data

```html
<input type="file" name='myfile'/>   
```

- hidden 隐藏域

```html
<input type='hidden' name='id' value='noway'/>
```

- image 图像提交 

```html
<input type='image' src='submit.jpg' alt='submit' />
```

- password 密码框

==定义密码字段。密码字段中的字符会被掩码（显示为星号或原点）==

```html
<input type="password"  name='pwd'/> 
```

- radio 单选框

==名字相同的为一组==

```html
    <input type='radio' name='city' value='南昌'/>南昌
    <input type='radio' name='city' value='上海'/>上海
```

- reset button 重置按钮

==定义重置按钮。重置按钮会清除表单中的所有数据==

```
<input type='reset'/>       
```

- submit 提交按钮

==提交按钮用于向服务器发送表单数据。数据会发送到表单的 action 属性中指定的页面==

```html
<form action="form_action.asp" method="get">
Email: <input type="text" name="email" />
<input type="submit" />
</form>
```

