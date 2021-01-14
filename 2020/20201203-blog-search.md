---
title: 在站内添加搜索文章功能
date: 2020-12-03
tags: 建站手册及问题
categories: 其他
---

站内搜索功能，快速根据关键词找到自己需要的文章
<!-- more -->
对于写博客的朋友来说，刚开始的时候肯定是比较高兴的，经常写一些自己喜欢的内容，过了一段时间后来就会发现页面首页目录特别长，有时候想看自己之前写过的文章要找好长时间，这时，一个站内搜索功能的重要性就体现出来了

hexo拥有很多好用快捷的插件，`hexo-generator-search`就是其中之一

[https://github.com/wzpan/hexo-generator-search](https://github.com/wzpan/hexo-generator-search)

这是他的git源码库，感兴趣的话可以阅读一下源码

### 添加功能

在终端执行，下载安装插件：

```shell shell
npm install --save hexo-generator-search
```

然后在合适的位置，比如，header头部，或者单独开一个页面，写进搜索的html代码

我是在主题配置中新增了一个页面，用来显示搜索页的

```html html
<div class="search">
    <div id="site_search">
        <input type="text" id="local-search-input" placeholder="搜索文章..." results="0"/>
        <div id="local-search-result"></div>
    </div>
</div>
```

~~新建一个 search.js 文件，在 head.ejs 中引入他：~~

这里有一些问题，在实际操作中，如果把脚本放在`head.ejs`中，就会在页面还没加载完的时候，先执行脚本，然后报错

详情可以看一下这篇文章[《浏览器报错"Cannot read property 'addEventListener' of null"的解决方案》](../20201208-javascript-err)

正确的写法是将脚本置于页面加载后的区域，比如上面 html 代码的底部引入脚本，hexo 的写法是：

```js javascript
<%- js('js/search') %>
```
然后在主题文件夹新建`./source/js/search.js`文件

<details>
    <summary>点击查看 search.js 代码(修改后完整版)</summary>

```javascript javascript
var searchFunc = function (path, search_id, content_id) {
    'use strict'; //使用严格模式
    $.ajax({
        url: path,
        dataType: "xml",
        success: function (xmlResponse) {
            // 从xml中获取相应的标题等数据
            var datas = $("entry", xmlResponse).map(function () {
                return {
                    title: $("title", this).text(),
                    content: $("content", this).text(),
                    url: $("url", this).text()
                };
            }).get();
            //ID选择器
            var $input = document.getElementById(search_id);
            var $resultContent = document.getElementById(content_id);
            $input.addEventListener('input', function () {
                var str = '<ul class=\"search-result-list\">';
                var keywords = this.value.trim().toLowerCase().split(/[\s\-]+/);
                $resultContent.innerHTML = "";
                if (this.value.trim().length <= 0) {
                    return;
                }
                // 本地搜索主要部分
                datas.forEach(function (data) {
                    var isMatch = true;
                    var content_index = [];
                    var data_title = data.title.trim().toLowerCase();
                    var data_content = data.content.trim().replace(/<[^>]+>/g, "").toLowerCase();
                    var data_url = data.url;
                    var index_title = -1;
                    var index_content = -1;
                    var first_occur = -1;
                    // 只匹配非空文章
                    if (data_title != '' && data_content != '') {
                        keywords.forEach(function (keyword, i) {
                            index_title = data_title.indexOf(keyword);
                            index_content = data_content.indexOf(keyword);
                            if (index_title < 0 && index_content < 0) {
                                isMatch = false;
                            } else {
                                if (index_content < 0) {
                                    index_content = 0;
                                }
                                if (i == 0) {
                                    first_occur = index_content;
                                }
                            }
                        });
                    }
                    // 返回搜索结果
                    if (isMatch) {
                        //结果标签
                        str += "<li><a href='../" + data_url + "' class='search-result-title'>" + "> " + data_title + "</a>";
                        var content = data.content.trim().replace(/<[^>]+>/g, "");
                        if (first_occur >= 0) {
                            // 拿出含有搜索字的部分
                            var start = first_occur - 6;
                            var end = first_occur + 6;
                            if (start < 0) {
                                start = 0;
                            }
                            if (start == 0) {
                                end = 10;
                            }
                            if (end > content.length) {
                                end = content.length;
                            }
                            var match_content = content.substr(start, end);
                            // 列出搜索关键字，定义class加高亮
                            keywords.forEach(function (keyword) {
                                var regS = new RegExp(keyword, "gi");
                                match_content = match_content.replace(regS, "<em class=\"search-keyword\">" + keyword + "</em>");
                            })
                            str += "<p class=\"search-result\">" + match_content + "...</p>"
                        }
                    }
                })
                $resultContent.innerHTML = str;
            })
        }
    })
};
var path = "../search.xml";
searchFunc(path, 'local-search-input', 'local-search-result');
```
</details>

现在`hexo s`运行一下可以看到，功能已经成功实现了

具体的样式代码可以自定义，根据自己的喜好



如果有什么具体的操作没弄清楚或者其他问题，欢迎评论留言或者私信邮箱，博主会在最快时间看到并回复