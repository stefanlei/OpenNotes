```js
    // 需要传递两个参数 一个是开始覆盖 一个是离开覆盖
    $('.item-button button').hover(function () {
        $('img').attr('style', 'transition: transform 1s;transform: scale(1.2);')
    }, function () {
        $('img').attr('style', 'transition: transform 1s;transform: scale(1);')
    })
```

