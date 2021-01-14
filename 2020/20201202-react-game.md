---
title: 用react做一个简单小游戏
date: 2020-12-02
tags: react 杂谈
categories: 前端技术
toc: true
---

在学习 React 教程的过程中，根据网站给出的例子，一边学一边练，做出一个井字棋小游戏，基于react，采用react-cli框架

<!-- more -->

## 创建一个项目

```shell shell
npx create-react-app demo
```

将`src/`目录下的文件清空，然后新建两个文件`index.js`和`index.css`

<details>
    <summary>点击查看index.js源码</summary>

```js javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';


class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {/* TODO */}
      </button>
    );
  }
}

class Board extends React.Component {
  renderSquare(i) {
    return <Square />;
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}

// ========================================

ReactDOM.render(
  <Game />,
  document.getElementById('root')
);
```
</details>


<details>
    <summary>点击查看index.css源码</summary>

```css css
body {
  font: 14px "Century Gothic", Futura, sans-serif;
  margin: 20px;
}

ol,
ul {
  padding-left: 30px;
}

.board-row:after {
  clear: both;
  content: "";
  display: table;
}

.status {
  margin-bottom: 10px;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.square:focus {
  outline: none;
}

.kbd-navigation .square:focus {
  background: #ddd;
}

.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}

```
</details>

然后将下面三行代码拷贝到`index.js`中去，放在顶部

```js javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
```

然后运行`npm satrt`可以得到

![井字图](https://i.loli.net/2020/12/02/oTKWhUq82J4FiN1.jpg)

## 了解一下React

React 是一个声明式，高效且灵活的用于构建用户界面的 JavaScript 库。使用 React 可以将一些简短、独立的代码片段组合成复杂的 UI 界面，这些代码片段被称作“组件”

来写一个最简单的组件例子
```javascript javascript
class List extends React.Component {
  render() {
    return (
      <div>
        <ul>
          <li>hello</li>
          <li>world</li>
        </ul>
      </div>
    )
  }
}

// 然后将组件代表的名字以标签的形式写进参数
ReactDOM.render(
  <List />,
  document.getElementById('root')
);
```
其中`List`是一个react组件类，然后通过`render()`方法返回到视图结构

`render()`的返回值`return`描述了希望在屏幕上看到的内容，然后react根据描述，把结果展示出来

每一个组件都是一个小整体，可以调整组件内的内容，通过无数的组件可以组合成一个页面，每个组件都可以单独运行，这样就可以通过无数个简单的小组间来构建出一个复杂的ui页面

如果某一个组件出现问题，或者不需要这部分的内容，可以直接在render里不渲染他，也不会影响整体的运行

## 游戏代码

查看一下`index.js`的代码，可与发现目前存在三个组件。分别是`Square`，`Board`，`Game`

Square 组件渲染了一个单独的 `<button>`。Board 组件渲染了 9 个方块。Game 组件渲染了含有默认值的一个棋盘，我们一会儿会修改这些值。到目前为止还没有可以交互的组件

将数据从`Board`组件中传递到`Square`组件中

```javascript javascript
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />
  }
}
```

然后修改`Square`组件中的`render()`方法，

```javascript javascript
class Square extends React.Component {
  render() {
    return (
      <button className="value">
        {this.props.value}
      </button>
    );
  }
}
```

在浏览器看一下变化，修改前：
![修改前](https://react.docschina.org/static/1566a4f8490d6b4b1ed36cd2c11fe4b6/6a91e/tictac-empty.png)

修改后：
![修改后](https://react.docschina.org/static/685df774da6da48f451356f33f4be8b2/01bf6/tictac-numbers.png)

方格发生了一些变化，刚才成功的把 prop 从父组件`Board`传递到了子组件`Square`

## 给组件添加交互功能

接下来，让每个方格被点击后都能落下一个 'x' 作为棋子

首先，把`Square`组件中`<button>`标签修改一下

```javascript javascript
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={function() { alert('click'); }}>
        {this.props.value}
      </button>
    );
  }
}
```

接下来，我们希望方格被点击后，被 'x' 填充，可以使用`state`来实现这个功能

可以通过在 react 组件中的构造函数设置`this.state`来初始化 sate ，`this.state`应该被视为一个组件的私有属性

在`this.state`中存储每个方格(Square)的值，在方格被点击时改变这个值

先向 class 中添加一个构造函数：

```javascript javascript
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }

}
```

## 调整细节

现在，我们来修改一下 Square 组件的`render()`方法，这样，每当方格被点击的时候，就可以显示当前`state`的值了：

1. 在`<button>`标签中，把`this.props.value`替换为`this.state.value`。
2. 将`onClick={...}`事件监听函数替换为`onClick={() => this.setState({value: 'X'})}`。
3. 为了更好的可读性，将`className`和`onClick`的 prop 分两行书写

`Square`组件就变成了下面这样：

```javascript javascript
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button
        className="square"
        onClick={() => this.setState({value: 'X'})}
      >
        {this.state.value}
      </button>
    );
  }

}
```

在 Square 组件`render`方法中的`onClick`事件监听函数中调用`this.setState`，我们就可以在每次`<button>`被点击的时候通知 React 去重新渲染 Square 组件。组件更新之后，Square 组件的`this.state.value`的值会变为`'X'`，因此，我们在游戏棋盘上就能看见`X`了。点击任意一个方格，`X` 就会出现了。

每次在组件中调用`setState`时，React 都会自动更新其子组件。

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-imgbed/52c019a3-0efc-4587-941f-ccf416bf7d9c.png)

## 完善游戏

当我们点击的时候，发现问题来了，全部都是`x`，井字棋游戏不是这样玩的啊，所以我们要继续调整，让`x`和`o`交替放置，判断出胜者

为 Board 组件添加构造函数，将 Board 组件的初始状态设置为长度为9的空值数组

```javascript javascript
class Board extends React.Component {
  // 添加的内容
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }
  // 添加的内容

  renderSquare(i) {
    return <Square value={i} />;
  }
```

```javascript javascript
renderSquare(i) {
  retunrn <Square value={this.state.squares[i]}>;
}
```

这样，每一个 Square 都可以接收到一个`value`prop了，然后修改一下 Square 的事件监听函数

由于 `state` 对于每个组件来说都是私有的，因此我们不能直接通过 Square 来更新 Board 的 `state`

相反，从 Board 组件向 Square 组件传递一个函数，当 Square 被点击的时候，这个函数就会被调用。接着，我们将 Board 组件的 `renderSquare` 方法改写为如下效果

```javascript javascript
renderSquare(i) {
  return (
    <Square
      value={this.state.squares[i]}
      onClick={() => this.handleClick(i)}
    />
  );
}
```

现在我们从 Board 组件向 Square 组件中传递两个 props 参数：value 和 onClick。onClick prop 是一个 Square 组件点击事件监听函数。接下来，我们需要修改 Square 的代码：

1. 将 Square 组件的`render()`方法中的`this.state.value`替换为`this.props.value`。
2. 将 Square 组件的`render()`方法中的`this.setState()`替换为`this.props.onClick()`。
3. 删掉 Square 组件中的构造函数`constructor`，因为该组件不需要再保存游戏的 state。

Square 组件就会变成下面这个样子：
```javascript javascript
class Square extends React.Component {
  render() {
    return (
      <button 
        className="square" 
        onClick={() => this.props.onClick({value: 'X'})}
      >
        {this.props.value}
      </button>
    );
  }
}
```

## 实现过程

每一个 Square 被点击时，Board 提供的 onClick 函数就会触发。我们回顾一下这是怎么实现的：

1. 向 DOM 内置元`<button>`添加 onClick prop，让 React 开启对点击事件的监听。
2. 当`<button>`被点击时，React 会调用 Square 组件的`render()`方法中的`onClick`事件处理函数。
3. 事件处理函数触发了传入其中的`this.props.onClick()`方法。这个方法是由 Board 传递给 Square 的。
4. 由于 Board 把`onClick={() => this.handleClick(i)}`传递给了 Square，所以当 Square 中的事件处理函数触发时，其实就是触发的 Board 当中的`this.handleClick(i)`方法。
5. 现在我们还尚未定义`handleClick()`方法，所以代码还不能正常工作。如果此时点击 Square，你会在屏幕上看到红色的错误提示，提示内容为：`“this.handleClick is not a function”`。

## handleClick()方法

这时候我们点击 Square 的时候，浏览器会报错，因为我们还没有定义 `handleClick()`方法。我们现在来向 Board 里添加`handleClick()`方法

```javascript javascript
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }

  // 添加的内容
  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = 'X';
    this.setState({squares: squares});
  }
  // 添加的内容

  renderSquare(i) {
    return <Square 
              value={this.state.squares[i]}
              onClick={() => this.handleClick(i)}
            />;
  }
```

现在，我们可以通过点击 Square 来填充那些方格，效果与之前相同。但是，当前 state 没有保存在单个的 Square 组件中，而是保存在了 Board 组件中。每当 Board 的 state 发生变化的时候，这些 Square 组件都会重新渲染一次。把所有 Square 的 state 保存在 Board 组件中可以让我们在将来判断出游戏的胜者

因为 Square 组件不再持有 state，因此每次它们被点击的时候，Square 组件就会从 Board 组件中接收值，并且通知 Board 组件。在 React 术语中，我们把目前的 Square 组件称做“受控组件”。在这种情况下，Board 组件完全控制了 Square 组件

注意，我们调用了`slice()`方法创建了`squares`数组的一个副本，而不是直接在现有的数组上进行修改。在下一节，我们会介绍为什么我们需要创建`square`数组的副本

>关于使用`slice()`方法创建了数组的一个副本，而不是直接修改现有的数组，这牵扯到一个概念－－不可变性。[为什么不可变性在 React 中那么重要？](../20201202-react-immutability)

## 函数组件

然后将 Square 组件重写成函数形式，也就是函数组件

如果组件中只包含一个`render()`方法，没有 state ，那么**函数组件**就会比较简单

不需要定义一个继承于`React.component`的类，直接定义一个函数，这个函数接收`props`作为参数，返回需要渲染的元素

函数组件写起来并不像 class 组件那么繁琐，很多组件都可以使用函数组件来写

```javascript javascript
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

## 轮流落子

目前还不能在棋盘上标记 “O”，我们将 “X” 默认设置为先手棋。可以通过修改 Board 组件的构造函数中的初始`state`来设置默认的第一步棋子


```javascript javascript
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
      xIsNext: true,
    };
  }
```

棋子每移动一步，`xIsNext`（布尔值）都会反转，该值将确定下一步轮到哪个玩家，并且游戏的状态会被保存下来。我们将通过修改 Board 组件的 `handleClick()`函数来反转`xIsNext`的值:

```javascript javascript
handleClick(i) {
  const squares = this.state.squares.slice();
  squares[i] = this.state.xIsNext ? 'X' : 'O';
  
  this.setState({
    squares: squares,
    xIsNext: !this.state.xIsNext,
  });
}
```

现在尝试在页面中玩一下，可以达到'X'和'O'轮流落子的效果了

接下来修改 Board 组件`render()`方法中 status 的值，这样就可以显示下一步是哪个玩家的了

```javascript javascript
render() {
  // const status = 'Next player: X';
  const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');

```

## 判断胜者

至此我们就可以看出下一步会轮到哪位玩家，与此同时，我们还需要显示游戏的结果来判定游戏结束。拷贝如下`calculateWinner(squares)`函数并粘贴到`index.js`文件底部

```javascript javascript
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

接着，在 Board 组件的`render()`方法中调用`calculateWinner(squares)`检查是否有玩家胜出，如果胜出，显示响应信息

修改 Board 组件中的`render()`代码

```javascript
render() {
  // const status = 'Next player: X';
  // const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
  const winner = calculateWinner(this.state.squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
  }
```

然后修改`handleClick()`事件，当有玩家胜出时，页面不会再执行，函数不做任何处理直接返回

```javascript javascript
handleClick(i) {
  const squares = this.state.squares.slice();

  // 添加的内容
  if (calculateWinner(squares) || squares[i]) {
    return;
  }
  // 添加的内容

  squares[i] = this.state.xIsNext ? 'X' : 'O';
  
  this.setState({
    squares: squares,
    xIsNext: !this.state.xIsNext,
  });
}
```

至此，井字棋游戏已经完成了

源码git仓库 >>> [https://github.com/shuxhan/jingziqi-game](https://github.com/shuxhan/jingziqi-game)

<details>
    <summary>点击查看index.js源码，copy之后覆盖到原来的index.js中</summary>

```javascript javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';


function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}

class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
      xIsNext: true,
    };
  }

  handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }

  renderSquare(i) {
    return <Square 
              value={this.state.squares[i]}
              onClick={() => this.handleClick(i)}
            />;
  }

  render() {
    // const status = 'Next player: X';
    // const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      xIsNext: true,
    };
  }

  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}



// ========================================

ReactDOM.render(
  <Game />,
  document.getElementById('root')
);
```
</details>
