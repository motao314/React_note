# React 笔记

## 二 - React 模板 -- ReactElement

上篇文章中我们介绍了 ReactDOM.render 中接收的三种模板：字符串，ReactElement和JSX。字符串 没有什么可以说的，真实的开发中我们也不可能只是向页面中单纯的添加一些文本。那接下我们来说下 ReactElement。

### ReactElement
ReactElement 本身是 React 中的一个方法，调用这个方法会给我们返回一个 Element 对象，这个 Element 对象，就是刚刚我们一直说的 ReactElement 或者说 React 节点，在调用 render() 方法以后，会把我们整个 Element 对象解析成 DOM

- Element 对象的结构

	```
	var element = {
	    // This tag allow us to uniquely identify this as a React Element
	    $$typeof: REACT_ELEMENT_TYPE,
	 
	    // Built-in properties that belong on the element
	    type: type,
	    key: key,
	    ref: ref,
	    props: props,
	 
	    // Record the component responsible for creating this element.
	    _owner: owner,
	  };
	```	

	- REACT_ELEMENT_TYPE 帮助我们标记着是否是一个 React Element
	- type 、key、ref 和 props 是我们一个节点基本组成部分
		- type 是这个节点的类型 如"li","div"
		- key是这个节点的唯一标示，会影响到我们节点的更新等很多问题，后边我们会详细的说道
		- ref 可以帮助我们 在生命周期中找到这个元素，后边我们也会详细的讲到
		- props 本身是一个对象，记录了我们加在节点上的相关属性
	- owner 记录了该节点时的父级或者说属于哪个组件

这里我们只是简单的看了一下 Element 整个对象的组成，如果大家想进一步学习ReactElement 整个方法 可以 [查看ReactElement的源码](js/create.js)。接下来我们来看下 平时我们可以怎么来创建 React Element

### React.createElement

虽然都叫 createElement 但要注意这个方法不是创建DOM节点的。在这个方法的 return 中 调用了 ReactElement 函数，也就是说调用 React.createElement 方法 就可以创建我们的 React 节点
```
ReactElement.createElement = function(type[, config, children]) {
	……
	return ReactElement(……);
}
```

在调用 createElement 方法时 我们需要 需要传入三个参数：

- type 必写参数 接收一个字符串，并且必须 是一个小写的 html 标签名,查看[demo](demo_1.html)

```
<div id="root"></div>
<script>
var Element = React.createElement("h1");
ReactDOM.render(
	Element,
	document.getElementById("root")
);	
</script>
```
	
- config 配置对象，该对象中的属性，都可以改成是我们要加给 该Element 的属性
	- 在 config 中除了 key、ref、self、source 这四个固定属性之外，其余的属性 最终都会被添加到 React Element 的 props 中 [查看demo](demo_2.html)

		```
		<div id="root"></div>
		<script>
		var Element = React.createElement(
			"h1",{
				id: "title",
				className: "title",
				title: "前端笔记", 
				style:{
					width: "100px",
					height: "100px",
					background: "red"
				}		
			}
		);
		ReactDOM.render(
			Element,
			document.getElementById("root")
		);	
		</script>
		```	

	- config 参数可以不写，但是如果 要设置 children 参数的话，就必须要添加，当然 可以 设置为 null

		```
		var Element = React.createElement("h1",null,"前端笔记");
		```

- children 这个节点的子节点 或者 文本
	- 如果只是想给这个节点设置文本内容，我们可以直接传入一个字符串 [查看demo](demo_3.html);

		```
		var Element = React.createElement("h1",null,"前端笔记");
		```		

	- 如果想要添加一个子节点 [查看demo](demo_4.html);

		```
		var Li = React.createElement("li",null,"前端笔记");
		var Ul = React.createElement("ul",null,Li);
		```		

	- 如果要添加多个子节点的话，我们可以有两种操作方法：
		- 从第2个子节点，开始以后所有参数都是子节点	[查看demo](demo_5.html);

			```
			var Li1 = React.createElement("li",null,"前端笔记");
			var Li2 = React.createElement("li",null,"前端笔记");
			var Ul = React.createElement("ul",null,Li1,Li2);
			```
		- 直接传入一个数组来作我们的第2个参数 [查看demo](demo_6.html);

			```
			var Li1 = React.createElement("li",null,"前端笔记");
			var Li2 = React.createElement("li",null,"前端笔记");
			var children = [Li1,Li2]
			var Ul = React.createElement("ul",null,children);
			```

在上述介绍中，我们简单的看到了，如何利用 React Element 来制作模板，不过我们列举的只是单一标签 或者 简单的结构，如果 我们结构比较复杂的时候，该怎么来写呢? [查看demo](demo_6.html);

```
<div id="root"></div>
<script>
var Title = React.createElement("h1",null,"页面标题");
var Header = React.createElement("header",null,Title);

var Sider = React.createElement("aside",null,"侧边栏");
var Content = React.createElement("article",null,"页面内容");
var Main = React.createElement("div",null,Sider,Content);

var Footer = React.createElement("footer",null,"页面底部");

var Page = React.createElement("div",null,Header,Main,Footer);
ReactDOM.render(
	Page,
	document.getElementById("root")
);	
</script>
```

通过上述demo，我们看到了一个稍复杂的模板的编写，但是这种模板从层级结构上来看极其不清晰，所以在真正开发时不推荐大家使用 ReactElement 的这种方式来编写我们的模板。本篇文章也只是给大家进行一个知识点的了解,跳过也并不影响到我们的后续学习。
下篇文章中，我们会继续讲解 React 的模板编写，但是我们会换一种更优雅的方式来编写我们的模板 