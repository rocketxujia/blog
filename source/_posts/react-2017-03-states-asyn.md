---
title: 理解 setState 更新异步性
layout: fasle
date: 2017-03-26 11:31:30
updated:
comments:
tags:
	- react state lifecycle
categories:
	- react
---

## 一、setState 异步性

### 官方文档描述
根据 **官方文档 [state-and-lifecycle](https://facebook.github.io/react/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous)**，有下面这段话：

````
State Updates May Be Asynchronous
React may batch multiple setState() calls into a single update for performance.
Because this.props and this.state may be updated asynchronously, you should not rely on their values for calculating the next state.
````

### 异步性示例 

可点击 [CodePen](http://codepen.io/rocketxujia/pen/BWPdRy)查看，具体代码如下：

````javascript
class SelectBox extends React.Component {
  constructor(props) {
    super(props);
    this.state={value: 'initValue'};
    // This binding is necessary to make `this` work in the callback
    this.handleChange = this.handleChange.bind(this);
    // console.log(this.handleChange);
  }

  handleChange (e) {
    this.setState({value: e.target.value});
    console.log('handleChange: ',this.state.value);
  }

  render() {
      return(
         <div>
             <select onChange={this.handleChange} value={this.state.value}>
                <option value="JavaScript" key={1}>JavaScript</option>
                <option value="Angular2" key={2}>Angular2</option>
                <option value="React" key={3}>React</option>
             </select>
             <h1>{this.state.value}</h1>
         </div>
      );
   }
}

ReactDOM.render(
  <div><SelectBox /></div>,
  document.getElementById('root')
);
````

我们在`handleChange`方法中，调用`setState`来更新选项的值，然后在控制台中输出这个值。看起来这两个值应该是同步相同的。但你如果一执行就会发现，在控制台中输出的`this.state.value`，并不会在执行`setState`方法后立即就变动。像下面的执行的结果图一样:




## 二、怎么解决示例问题

### 参照上述官方文档的解决方案为：
```
To fix it, use a second form of setState() that accepts a function rather than an object. 
That function will receive the previous state as the first argument, 
and the props at the time the update is applied as the second argument:
```

所以可以修改上述 Demo 如下：
````Javascript
this.setState( 
	{
		value: e.target.value
	}, 
	function(){ 
		console.log('After setState invoke，this.state.value = ', this.state.value); 
	}
);
````

### 使用 componentDidUpdate()

另一个解决方案就是使用这个生命周期方法，把确定state更新后要执行的代码放在里面，如下面的代码:
````javascript
componentDidUpdate(){
  console.log('After setState invoke，this.state.value = ', this.state.value); 
}
````

## 三、React这么处理的原因

``
setState这个方法，它在React中的执行行为可以认为"异步的"
``
setState方法与包含在其中的执行是一个很复杂的过程。这段程式码从React最初的版本到现在，也有无数次的修改。它的工作除了要更动this.state之外，还要负责触发重新渲染(render)，这里面要经过React核心中diff演算法，最终才能决定是否要进行重渲染，以及如何渲染。而且为了性能的理由，多个setState执行有可能在执行过程中还需要被合并，所以它被设计以异步的或延时的来进行执行是相当合理的。

那么setState会在何时以同步的方式来执行，也就是立即更动this.state？ 答案是在React库控制之外时，它就会以同步的方式来执行。

可点击 [CodePen](http://codepen.io/rocketxujia/pen/mWjpZJ)查看，具体代码如下：
````javascript
var Demo = React.createClass({
  getInitialState: function() {
    return ({
      counter: 0
    });
  },
  componentDidMount: function() {
    // Automatically update the state every 5 seconds.
    setInterval(this.updateState, 5000);
    // Update the state on mouse-down.
    // --
    // NOTE: We are implementing our own event binding here - not using the
    // React Element props to manage the event handler.
    ReactDOM.findDOMNode(this)
      .addEventListener("mousedown", this.updateState);
  },
  // I render the component based on the current state.
  render: function() {
    return (
      React.DOM.span({
          onClick: this.updateState,
          className: "button"
        },
        ("Counter at " + this.state.counter)
      )
    );
  },
  // I update the state and log the existing state value on either side of
  // the operation in an attempt to see if the state mutation operation is
  // synchronous or asynchronous.
  updateState: function(event) {
    console.log("= = = = = = = = = = = =");
    console.log("EVENT:", (event ? event.type : "timer"));
    console.log("Pre-setState:", this.state.counter);
    this.setState({
      counter: (this.state.counter + 1)
    });
    console.log("Post-setState:", this.state.counter);
  }
});

// Render the root Demo and mount it inside the given element.
ReactDOM.render(
  React.createElement(Demo),
  document.getElementById("root")
);
````

从示例中看出, `updateState()` 被三种方式调用：

* setTimeout() - not managed by ReactJS.
* mousedown - not managed by ReactJS.
* onClick - managed by ReactJS.

其中一个被React管理的`click`事件回调方法中的`setState`是异步的，
而另二个我们自定义的，未被React管理的`mousedown`,及定时器`timer` 却是同步的。




