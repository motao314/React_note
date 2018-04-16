# React 笔记

## 七 - 事件和state
上一篇我们整理了 props 和 父组件向子组件通信，但是子组件向父组件通信，不过在具体讲通信之前，我们需要先来看看另外两个东西 事件 和 state

### React中的事件添加
在 React 中我们一样也可以正常的使用事件，但是有些小的注意事项：

- 事件书写的时候，需要on之后每个单词的首字母大写，如 onClick/onMouseOver

- 在 React 中添加事件，类似于我们在JS中添加行间事件，示例如下：

```
class App extends React.Component {
	handleClick(){
		alert("Hello React")
	}
	render(){
		return <button onClick ={this.handleClick}>按钮</button>
	}	
}
```

- 另外，方便管理我们通常会被会把事件函数的this指向组件`<button onClick ={this.handleClick.bind(this)}>按钮</button>`，那在事件函数中，如何拿到触发当前事件的元素呢？如下(也可查看[demo](demo_1.html))：

```
class App extends React.Component {
	constructor(arg){
		super(arg);/* 注意 组件的构造函数中必须接受第一个参数，这个参数是React 默认传的 参数的内容是我们的props */
		this.handleClick = this.handleClick.bind(this);
		/* 方便代码阅读性,通常我们会在构造函数中去做相关的 this 绑定 */
	}
	handleClick(e){
		console.log(this); //组件
		console.log(e.target);// button
		console.log(e.currentTarget);// button
	}
	render(){
		return <button  onClick ={this.handleClick}>按钮</button>
	}	
}
```

### state 状态
React 中组件内部的修改主要通过 state 也就状态完成，每次 state 更新之后，就可以重新渲染我们的模板，具体[示例](demo_2.html)如下：

```
class App extends React.Component {
	constructor(arg){
		/* 注意 React 中，都组件继承自 Component ，在写构造函数时，一定要继承第一个参数，这个参数是 父组件传入 相关信息 */
		super(arg); 
		/* 我们会在 构造函数中 设置 初始状态 */
		this.state = {
			like: true
		}
		this.toLike = this.toLike.bind(this);
	}
	toLike(){
		/*修改状态的时，我们需要调用 setState 方法，如果只是直接修改 state 对象，模板并不会渲染*/
		this.setState({
			like: !this.state.like
		});
		/*
			调用完 setState 之后，React 会重新调用 该组件的 render 方法，然后重新渲染模板，具体的流程我们在之后的生命周期中，详细的说
		*/
	}
	render(){
		return (<div>
			<button onClick={this.toLike}>切换状态</button>
			<p className={this.state.like?'like':'disLinke'}>我{this.state.like?'喜欢':'不喜欢'}体育运动</p>
		</div>);
	}	
}
``` 

上边的例子中，我们看到了 state 的初步应用，另外需要注意两个问题：state 中有多个状态时，我可以选择性的只修改一个或几个，另外要注意多次 setState 会被合并，最终一会render一次，具体[示例](demo_3.html)如下:

```
class App extends React.Component {
	constructor(arg){
		super(arg);
		this.state = {
			date: "周六",
			action: "打篮球",
			class: "red"
		}
		this.changeDate = this.changeDate.bind(this);
		this.changeAction = this.changeAction.bind(this);
	}
	changeDate(){
		this.setState({
			date: this.state.date === "周六"?"周日":"周六"
		});
		/*
			state 中，有 三个属性，但是我们只修改了，React 会自己帮我们 进行合并，剩余的两个属性 还是保留之前的值
		*/
	}
	changeAction(){
		this.setState({
			action: this.state.action === "打篮球"?"踢足球":"打篮球"
		});
		this.setState({
			class: this.state.class === "red"?"blue":"red"
		});
		/*
			这里我们调用了 两次 setState，但是 React 会把 这两次 setState 操作进行合并，最终 只会 render 一次，大家也可以 在 render 方法中 添加 console 来查看

			当然 上边的 两个 setState 可以 合并写 这里主要是为了给大家演示
			this.setState({
				class: this.state.class === "red"?"blue":"red",
				action: this.state.action === "打篮球"?"踢足球":"打篮球"
			});

		*/
	}
	render(){
		return (<div>
			<button onClick={this.changeDate}>切换日期</button>
			<button onClick={this.changeAction}>切换动作</button>
			<p className={this.state.class}>{this.state.date},我要去{this.state.action}</p>
		</div>);
	}	
}
```

关于 事件 和 状态我们就整理到这里，在下篇文章中，我们会用一个完整的案例，把组件通信给讲完整



