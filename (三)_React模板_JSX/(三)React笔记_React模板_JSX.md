# React 笔记

## 三 - React 模板 -- JSX
上篇文章中我们介绍了利用 React Element 来构建我们的模板，但是可以看到构建的过程特别的麻烦，而且结构不够清晰，不方便我们后期进行维护，这一章我们会会介绍一种新的构建模板的方式 JSX

### JSX
JSX 拆开就是 JS + XML，简单的说就是给我们的 JS 添加了 XML 的语法扩展，或者说 有了 JSX 之后 我们就可以在 JS 中使用 XML [查看示例](demo_1.html)

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

从上述示例中我们看到可以直接在 js 中利用 我们熟悉html 来构建我们的模板，这样的话 结构层级非常清晰，也便于我们维护，当然上手也更便捷。但是在使用 JSX 的时候，还有些问题需要注意
- 在使用 JSX 中，不能嵌套多个同级标签，一定要在外边加一个父级
- 使用 JSX 时，我们需要使用 Babel 来进行编译，所以必须引入 Babel 并且在 script 上 添加 type="text/babel" 来提示 babel 编译我们 script 中的代码
- 使用 JSX 时，当我们要写的是一个 HTML 标签时，请全部小写
- 使用 JSX 时，所有标签都必须闭合 单标签 `<单标签 />` 也一样必须闭合

### 插值表达式
插值表达式，格式{代码} 可以让我们在 JSX 中接受一个 JS 的表达式，[查看示例](demo_2.html)
```
let a = "hello";
let b = "React";
ReactDOM.render(
	<h1>{a + b}</h1>,
	document.getElementById("root")
);	
```

- 插值表达式 在什么时候使用？
	- 当我们需要在 JSX 中使用 JS 中的数据的时候，我们就需要使用差值表达式 
	- 当我们要在 JSX 中添加 注释的时候，也需要使用插值表达式 `{/* 在这里写JSX的注释 */}`
- 使用插值表达式时的注意事项
	- {}中，接收一个 JS 表达式，可以是我们的算术表达式，变量 或函数调用, 最终 {} 接收的是表达式 计算的结果
	- {}中，接收的是 函数调用时，该函数需要由返回值	
	- {}中，不能写 if else 以及 switch 这些流程控制语句，但是可以使用三目表达式 来进行判断，[查看示例](demo_3.html)

	```
	let a = 10;
	let b = 20;
	ReactDOM.render(
		<h1>{a > b?"正确":"错误"}</h1>,
		document.getElementById("root")
	);	
	```	

	- {}中，不能写 for 或者 whlie 这些循环语句，可以使用数组方法代替，[查看示例](demo_4.html)
	
	```
	let arr = [
		"这是第一项",
		"这是第二项",
		"这是第三项"
	]
	ReactDOM.render(
		<ul>
			{arr.map((item,index)=>{
				/* 当我们要向 JSX 中添加一组元素时，一定要设置 key 属性，具体内容 我们会在后边的文章中讲到 */
				return <li key={index}>{item}</li>
			})}
		</ul>,
		document.getElementById("root")
	);
	```

- 不同类型数据在插值表达式中的数据输出：
	- 字符串、数字：原样输出
	- 布尔值、空、未定义：输出空值，也不会有错误
	- 对象：不能直接输出，但是可以通过其他方式，Object.values、Object.keys 等方法去解析对象
	- 数组：支持直接输出，默认情况下把数组通过空字符串进行拼接

### JSX的属性操作 	
- 属性值加了引号，那么就是一个普通的属性书写方式

```
ReactDOM.render(
<h1 title="React笔记">React笔记</h1>,
document.getElementById("root")
);
```

- 属性值可以直接写成差值表达式

```
let title = "React笔记" 
ReactDOM.render(
<h1 title={title}>React笔记</h1>,
document.getElementById("root")
);
```

- class：在 JSX 中需要使用 className 属性去代替 class

```
ReactDOM.render(
<h1 className="title">React笔记</h1>,
document.getElementById("root")
);
```

- style：在 JSX 中 style 的值只能是对象 style={{ property : value }}

```
let style = {
	borderBottom: "1px solid #000"
}
ReactDOM.render(
<h1 style={style}>React笔记</h1>,
document.getElementById("root")
);
```

```
ReactDOM.render(
<h1 style={{
	borderBottom: "1px solid #000"
}}>React笔记</h1>,
document.getElementById("root")
);
```

看完了 JSX 的基本操作之后，我们来看一个小例子，如何通过数据来生成一个简单的视图[查看案例](demo_5.html)
```
let data = {
	title: "巅峰榜·热歌",
	details: [
		{
			name: "体面",
			message: "《前任3：再见前任》电影插曲"
		},
		{
			name: "说散就散",
			message: "《前任3：再见前任》电影主题曲"
		},
		{
			name: "BINGBIAN病变 (女声版) ",
			message: "抖音热门歌曲"
		}	
	]
}
ReactDOM.render(
	<section className="box">
		<h2 className="title">{data.title}</h2>
		<ul className="list">
			{data.details.map((item,index)=>{
				return (
					<li>
						<p className="name">{item.name}</p>
                        <p className="message">{item.message}</p>
					</li>
				);
			})}
		</ul>
	</section>,
	document.getElementById("root")
);
```
关于 JSX 的基本使用我们就说到这后续的章节中，我们再去讲解组件拆分等复杂应用。 在下一个章节中，我们会说明 如何利用 create-react-app 来快速的搭建我们 React 的开发环境以及相关的注意事项，另外也建议大家可以把 上述的各个示例中的代码 自己动手敲一遍去巩固自己的学习深度。