---
title: 在vscode中使用react时，html标签不能自动补全
date: 2020-11-06
tags: [react 杂谈, 工具]
categories: 前端技术
---



然后点击添加项，项输入`javascript`,值输入`javascriptreact`，确定之后返回react写代码，打出标签，再按`tab`，就可以自动生成并补全标签了
<!-- more -->

`首选项` > `设置` > 输入`includeLanguages`

就会出现下面这个页面

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-imgbed/0a2c36dd-4eb4-4387-883d-0b66813a6f5a.png)

---

当我们在使用vscode建立一个react工程目录后，着手开始写代码，但是在完成下面这一步时，出现了一些小问题
````js
import React, { Component, Fragment } from 'react';

function Demo() {
	return (
		<Fragment>
			
		</Fragment>
	);
}

export default Demo;
````

正常来说，输入html标签的时候，比如`div`，然后再按一下`tab键`就可以自动补全称为`<div></div>`

但是react中不会这样，很生硬的得自己手打`<>`框

这里有一个设置可以解决这个问题

`首选项` > `设置` > 输入`includeLanguages`

就会出现下面这个页面

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-imgbed/0a2c36dd-4eb4-4387-883d-0b66813a6f5a.png)

然后点击添加项，项输入`javascript`,值输入`javascriptreact`，确定之后返回react写代码，打出标签，再按`tab`，就可以自动生成并补全标签了