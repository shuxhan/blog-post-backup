---
title: hexo怎么发布自制主题？
date: 2020-12-23
tags: 建站手册及问题
categories: 其他
---

其实官网已经说明了，但是没有更详细的文档，我在搜索引擎查找也没有相关的内容，因此我自己将动手写一篇教程。
<!-- more -->

[hexo 配置文档](https://hexo.io/zh-cn/docs/themes)

## fork hexojs/site 到仓库

[hexojs/site 仓库](https://github.com/hexojs/site)

```shell
git clone https://github.com/hexojs/site
cd site
npm install
```

这是官方的说明，但是我想说，"臣妾真的做不到啊！"，github因为服务器的问题速度一直很慢，平时 clone 小东西都要将近十分钟，更别说这么一个大项目。

最开始我想的事把仓库克隆到 gitee 在下载，然后在推送到 github ，感觉很麻烦，就放弃这种做法了。

这里我推荐直接在 fork 的仓库在线编辑，虽然速度慢，但是绝对稳定不出错。

## 编辑主题文件

咱们把做好的主题拿出来，放在一个新的仓库里，根据惯例一般取名为`hexo-theme-xxx`，因此我的仓库名为`shuxhan/hexo-theme-simple99`。

做好之前，根据官网的提示，来到`shuxhan/site`里面，编辑`source/_data/themes.yml`，在文件里增加自己的主题说明：

```js
- name: simple99
  description: A concise wind blog theme based on hexo system.
  link: https://github.com/shuxhan/hexo-theme-simple99
  preview: https://shuxhan.com
  tags:
    - simplicity
    - function
    - widget
    - mobile
    - 中文
```

没吃过猪肉，还没见过猪跑吗？下面那么多例子，直接对着模仿着写。

主题名称，主题说明，仓库链接，示例链接，标签。依次填上即可。

然后保存文件 commit 。

然后准备一张截图，图片像素必须为`800*500`且图片格式为 `.png`，一般截图主题博客的主页即可，处理完之后重命名为与主题同名的图片，我的话就是`simple99.png`。将这张照片放在`source/themes/screenshots`中即可。

然后保存文件 commit 。

## 申请合并

ok，基本工作已经完成，接下来就是收尾了

来到我们的`shuxhan/site`仓库，点击上方列表的`pull requests`，提出合并请求

![](https://shuxhan-imgbed.oss-cn-hangzhou.aliyuncs.com/img/20210108144505.png)

然后再点击 `new pull request`，创建新的合并申请

![](https://shuxhan-imgbed.oss-cn-hangzhou.aliyuncs.com/img/20210108144506.png)

在点击 `view pull request`，进去之后就会出现下面这个页面

![](https://shuxhan-imgbed.oss-cn-hangzhou.aliyuncs.com/img/20210108144507.png)

通过翻译可以得知是检查自己是否已经满足要求，勾勾选选，然后刷新页面，就可以啦，返回 code ，等待自己的申请通过即可

希望我的主题大家能够喜欢，记住名字哦－－**simple99**

仓库是 [https://github.com/shuxhan/hexo-theme-simple99](https://github.com/shuxhan/hexo-theme-simple99)，分享给需要的人。