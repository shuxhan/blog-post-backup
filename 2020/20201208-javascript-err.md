---
title: 浏览器报错"Cannot read property 'addEventListener' of null"的解决方案
date: 2020-12-08
categories: 前端技术
---

今天在写代码的时候，浏览器抛出了一个错误

`Cannot read property 'addEventListener' of null`
<!-- more -->
调试后发现，原来在页面还没加载完成的时候，js代码就已经执行，显然我把这段代码放在了`head`之中，执行代码脚本在监听的时候，DOM节点还没有创建，浏览器自然找不到监听的元素，所以返回`null`

解决方法有：页面加载完成之后在执行这串代码

我尝试把这串代码写成文件，然后放在body后，很好浏览器没有报错，也达到了我想要的效果

同样的文件，放在不同的位置，就会产生不一样的效果，运气不好就会跟我一样报错

简单测试一下 js 文件放在不同位置触发的先后顺序

```html html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script>
        console.log('head中')
    </script>
</head>
<script>
    console.log('<head/>后，<body>前')
</script>

<body>
    <script>
        console.log('body中')
    </script>
</body>
<script>
    console.log('<body/>后，<html>前')
</script>

</html>
<script>
    console.log('</html>后')
</script>
```

![](https://i.loli.net/2020/12/08/5Y9UMHf4sAu8Eyw.png)

由此可见：**DOM是从上到下按照顺序加载**