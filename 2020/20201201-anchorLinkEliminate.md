---
title: 返回上一页，不是返回上一个锚链接
date: 2020-12-01
categories: 前端技术
---

这是在写一个页面时遇到的问题，再点击返回上一步前，如果点击了本页的锚链接，那么返回上一步就变成了返回上一个锚链接

<!-- more -->

很简单，给锚链接绑定一个事件
```html html
<a href="#" onclick='anchorLinkEliminate(this);return false;'></a>
```
一个函数事件
```js javascript
function anchorLinkEliminate(obj){
    location.replace(obj.href);
}
```

当点击锚链接的时候，浏览器就会抹去这一步历史操作记录，再点击返回上一步时，就正常返回上一个页面了