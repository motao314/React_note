# React 笔记

## React 入门
React 是目前使用的最火的渐进式前端框架之一。React 主要关注的是UI界面的构建，本身逻辑简单而且性能也很不错，
所以收到了越来越多人的喜欢。

### 基础
在学习 React 之前，大家需要先行掌握 HTML、CSS、JS 基础，由于文章中的 demo 使用了大量的 ES6 的语法，所以大家还需要先行掌握 ES6 

### 安装
鉴于本篇是入门教程，我们先采取最简单的办法 -- 直接引入文件到我们我们的页面中
文件下载：
	- [react.js](js/react.development.js) 
	- [react-dom.js](js/react-dom.development.js)
	- [babel.js](js/babel.min.js)
文件说明：
	- react.js 是 React 的核心代码库
	- react-dom.js 提供了对dom的操作方法
	- babel.js 会对我们页面中的代码进行编译，如 ES6 的代码会编译成浏览器识别的 ES5 的代码，把 JSX 编译成我们浏览器识别的格式
**该教程中使用的是React版本为v16.0.0**

除了上述安装方法之外，还可以通过 npm 或 yarn 等包管理工具安装，或者直接使用 create-react-app 快速 安装react环境，我们在后续的教程中一一讲解。

### 基本使用
- ReactDOM.render(template,targetDOM)
	- ReactDom.render 这是 React 中最基本的一个方法，用于把我们的模板(template)转成浏览器可以识别的html的代码，并把 转换后html 挂在到 我们指定的 页面元素上 (targetDOM)
	- template 模板
		- 字符串，会直接把这段字符串 当做 targetDOM 中的文本 查看[dome](demo_1.html)

		```
		<script>
			ReactDOM.render(
				"hello! React",
				document.getElementById("root")
			);	
		</script>
		```

		- ReactElement 这是React创建的一个虚拟dom节点，稍后我们详细的讲,查看[demo](demo_2.html)

		```
		<script>
			var Element = React.createElement(
				"h1",
				null,
				"hello! React"
			);
			ReactDOM.render(
				Element,
				document.getElementById("root")
			);	
		</script>	
		```

		- JSX，使用JSX时，要注意最外层必须有一个包含父级，具体的内容我们在后续文章中详细的讲解,查看[demo](demo_3.html); 

		```
		<script type="text/babel">
		/* 使用 JSX 时 我们需要babel编译我们的代码，所以一定要加上  type="text/babel" */
		ReactDOM.render(
			<h1>hello! React</h1>,
			document.getElementById("root")
		);	
		</script>
		```

	- targetDOM 页面中的元素节点，不建议是 body 、html 及 document

上述说明中，我们看到了如何把模板渲染到页面，这个也是 React 最基本的一项工作，在下篇文章中 我们来看看 如何来编写我们的模板

