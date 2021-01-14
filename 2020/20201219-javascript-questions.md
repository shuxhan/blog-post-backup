---
title: javascript 进阶问题
date: 2020-12-19
categories: 前端技术
toc: true
---

Github：https://github.com/lydiahallie/javascript-questions

相当不错的一个 Github 仓库，`javascript questions` ，作者每周都会发布一些有关 javascript 的题目，虽然不难但是考察细节，刷了一会，其实一些很简单的问题，结果因为细节思考不到位直接出错。
<!-- more -->

>引用作者的话：我在我的 Instagram 上每天都会发布 JavaScript 的多选问题，并且同时也会在这个仓库中发布。

>从基础到进阶，测试你有多了解 JavaScript，刷新你的知识，或者帮助你的 coding 面试！ 💪 🚀 我每周都会在这个仓库下更新新的问题。

>答案在问题下方的折叠部分，点击即可展开问题。祝你好运 ❤️



希望这个仓库能够提升自己，⛽加油！

顺便记录一下自己觉得还不错的知识点

---

## 2020.12.19

### 1.关于一元操作符

1. +true
一元操作符加号将布尔值转换为 `number`，true > 1, false > 0
2. !"Lisa"
字符串`"Lisa"`是一个真值，`!`取反则返回`false`

### 2.静态方法不能被实例调用

```js
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor
    return this.newColor
  }

  constructor({ newColor = 'green' } = {}) {
    this.newColor = newColor
  }
}

const freddie = new Chameleon({ newColor: 'purple' })
freddie.colorChange('orange')

// 返回 TypeError
```

`colorChange` 是一个静态方法。静态方法被设计为只能被创建它们的构造器使用（也就是 `Chameleon`），并且不能传递给实例。因为 `freddie` 是一个实例，静态方法不能被实例使用，因此抛出了 `TypeError` 错误。


### 3.构造函数和原型

之前写过有关面向对象编程的概念，讲述了构造函数和原型对象的一些概念
[创建构造函数](../20201114-object-oriented/#3-构造函数)

## 2020.12.21

### 1.一元运算符

```js
let number = 0
console.log(number++)
console.log(++number)
console.log(number)

// 0 2 2
```

**一元后自增运算符 ++：**

1. 返回值（返回 0）
2. 值自增（number 现在是 1）

**一元前自增运算符 ++：**

1. 值自增（number 现在是 2）
2. 返回值（返回 2）

### 2.标记模版字面量

```js
function getPersonInfo(one, two, three) {
  console.log(one)
  console.log(two)
  console.log(three)
}

const person = 'Lydia'
const age = 21

getPersonInfo`${person} is ${age} years old`
```

* A: `"Lydia"` `21` `["", " is ", " years old"]`
* B: `["", " is ", " years old"]` `"Lydia"` `21`
* C: `"Lydia"` `["", " is ", " years old"]` `21`

答案: B

如果使用标记模板字面量，第一个参数的值总是包含字符串的数组。其余的参数获取的是传递的表达式的值！