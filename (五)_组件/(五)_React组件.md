# React 笔记

## React 组件
在第三讲 JSX 中我们用过一个案例，一个页面的简单机构，稍微回顾一下，[查看示例](demo_1.html)

```
ReactDOM.render(
	<div>
		<header>
			<h1>页面标题</h1>
		</header>
		<div>
			<aside>侧边栏</aside>
			<article>页面内容</article>
		</div>
		<footer>页面底部</footer>
	</div>
	,
	document.getElementById("root")
);
```
但是，该结构只是一个最简单的结构，如果在稍微复杂一些，可读性和维护性就比较差，并且很多时候，在一个页面或者多个页面中会有很多重复的结构，那在向之前这么写就特别的麻烦，所以我们需要一种更简单的书写方式组件，[查看示例](demo_2.html)

```
ReactDOM.render(
	<div>
		<header>
			<h1>页面标题</h1>
			<nav>
				<a href="#">首页</a>
				<a href="#">关于我们</a>
				<a href="#">加入我们</a>
			</nav>
		</header>
		<div>
			<aside>
				<nav>
					<a href="#">首页</a>
					<a href="#">关于我们</a>
					<a href="#">加入我们</a>
				</nav>
			</aside>
			<article>页面内容</article>
		</div>
		<footer>页面底部</footer>
	</div>
	,
	document.getElementById("root")
);	
```

### 编写 React 组件
在编写组件之前，先要注意一个问题，在 React 中组件的命名必须以大写字母开始，而标签必须是小写，如 `<Img />` 代表的是调用 Img 组件，`<img />` 则是 img 标签。
在 React 中，编写组件有多种方法，在本教程中，我们只介绍最常用的一种方法：
1. 新建一个js文件，一般便于管理我们会把文件的命名`nav.js`
2. 在文件中引入 React `import React from 'react';`;
3. 从 React.Component 中继承一个类出来 `class Nav extends React.Component {}`
4. 然后在 class 中新建个 render 方法，render 的 return 中书写我们组件的内容，那这个组件就已经建好了
5. 最后方便在其他文件中调用我们把这个组件导出 `export default Nav`;
6. 如果我们想在其他文件中使用 这个组件的话，只需要引入这个文件 `import Nav from "./nav.js";`, 然后在使用的地方直接 写 `<Nav />` 就可以直接使用这个组件
按照上述6个步骤大家就会有一个自己的组件了，[查看完整代码](dome_3/)
关于组件的编写我们就介绍到这，在下一个章节，我们会讲解利用props在父组件中向子组件传递数据。 




