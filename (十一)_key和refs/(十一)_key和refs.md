# React 笔记

## 十一 - key和refs

今天我们来说说 React 中两个特殊的存在 refs 和 key

### refs 

refs 是 React 中比较特殊的一个属性，它可以帮助我们获取到 React 中，真实的 DOM 节点 或者 组件实例，然后来去做一些特殊的事情，比如添加动画

refs 的使用参照以下[案例](demo_1.html)： 

	```
		class Txt extends React.Component {
	    	render(){
	    		return (<p>前端笔记</p>);
	    	}
	    }
	    class Demo extends React.Component {
	    	componentDidMount(){
	    		console.log(this.refs);// {txt1: p, txt2: Txt} refs 对象的存储的是 refs 获取到的数据
	    		console.log(this.refs.txt1); // <p >妙味课堂</p>  获取到真实的p标签
	    		console.log(this.refs.txt2); 
	    		/*
	    		获取到 Txt 组件实例 
	    			Txt {props: {…}, context: {…}, refs: {…}, updater: {…}, _reactInternalFiber: FiberNode, …}
	    		*/

	    		/* 最后注意 函数式组件 不能添加 ref */
	    	}
	    	render(){
	    		return (<div>
	    			<p ref="txt1">妙味课堂</p>
	    			<Txt ref="txt2" />
	    		</div>);
	    	}
	    }
	    ReactDOM.render(
	        <Demo />,
	        document.getElementById("app")
	    );
	```

refs 的使用就说到这，如果大家还关心如何利用refs添加动画，配合原生js的话，可以查看[完整示例](demo_2.html)

### key

在 React 中, 我们通过数组创建一组子元素时，React 通常会报出一个警告 `Each child in an array or iterator should have a unique "key" prop`。翻译过来的意思就是我们应该给每一个子项添加一个 key 属性。如下例:

```
	let list = [
        "列表项1",
        "列表项2",
        "列表项3"
    ];
    class List extends React.Component {
        render(){
            return (<ul>
                {list.map((item)=>{
                    return <li>{item}</li>
                })}
            </ul>);
        }
    }
    ReactDOM.render(
        <List />,
        document.getElementById("app")
    );
```

这个key属性是用来做什么的呢？在我们程序中又起到了什么作用，我们来具体说说。

React 本身是一种 MVVM 的架构模式。这种模式的运行过程，我们简单介绍下 M：数据模型、 V：视图、VM：视图模型(也就是大家常说的虚拟DOM)。在 React 的运行过程中，我们的数据( state 和 props)发生了改变之后，React 并不会直接去修改我们的视图(也就是我们真实的DOM)，修改 DOM 本身在JS中是一件特别耗性能的事。因此 React 会先生成一个虚拟DOM 层(也就是我们说的VM 层)，每次我们修改数据的时候，React 都会先在虚拟DOM层中找出和上次不一样的节点，然后在 DOM 只对不一样的节点进行重新渲染，这样就极大的节省了我们的DOM操作，优化了操作性能。

简单了解一下 React 的渲染机制之后，我们在说回 key，key属性在这个过程中起到了什么作用呢？key 本身我们可以理解 React 给一组子项中，每一项加的标示, 在数据改变后，在对比的过程中，React 会根据标识对比该子项 数据更新前 和 数据更新后 内容是否有不同，然后判断是否需要重新渲染这个元素。如下示例：

```
class List extends React.Component {
    constructor(arg){
        super(arg);
        this.state = {
            list: [
               {
                    id:1,
                    content: "列表项1"
               },{
                    id:2,
                    content: "列表项2"
               },{
                    id:3,
                    content: "列表项3"
               }
            ]
        }
    }
    componentDidMount(){
        setTimeout(()=>{
            this.setState({
                  list:[{
                    id:1,
                    content: "列表项1"
               },{
                    id:2,
                    content: "列表项2-新"
               },{
                    id:3,
                    content: "列表项3"
               }] 
            })
        },5000);
        /*
            当我们更新完数据之后，React 在对比时，发现只有 key为2的子项对应的数据发生了变话，那在渲染 DOM 时，就只会更新 key 为 2 的元素的DOM
        */
    }
    render(){
        let {list} = this.state;
        return (<ul>
            {list.map((item)=>{
                return <li key={item.id}>{item.content}</li>
            })}
        </ul>);
    }
}
ReactDOM.render(
    <List />,
    document.getElementById("app")
);
```

key 的合理运用可以帮助我们极大的提高性能，但是刚接触 React 的同学，有两个经常犯的错误要注意一下

- 在一组数据中 key 值重复。如下所示，这样写的话，在 React 新版本中会直接报出警告。**在 React 中，一组数据的每个key的值一定都是唯一的**

	```
		render(){
            let {list} = this.state;
            return (<ul>
                {list.map((item)=>{
                    return <li key={1}>{item.content}</li>
                })}
            </ul>);
        }
	```

- 使用数组的索引来当组 key 值。这样操作的风险在于 数组进行删除或者添加操作，会改变数组的顺序，那么在对比时，会导致大批数据和之前不一样，需要对多个节点进行重新渲染，如下例中，我们删除了数组的第0项，"列表项2"就变成了第0项，那在对比 key 为0 的子项是，就会发现数据发生了，进而更新 DOM，依次往下，每一项都和之前不一样，所以整体都需要重新渲染，这样就造成了很大的性能浪费

	```
	class List extends React.Component {
	    constructor(arg){
	        super(arg);
	        this.state = {
	            list: [
	                "列表项1",
	                "列表项2",
	                "列表项3"
	            ]
	        }
	    }
	    componentDidMount(){
	        
	        setTimeout(()=>{
	            let {list} = this.state;
	            list.shift();
	            this.setState({
	                  list
	            })
	        },5000);
	    }
	    render(){
	        let {list} = this.state;
	        return (<ul>
	            {list.map((item,index)=>{
	                return <li key={index}>{item}</li>
	            })}
	        </ul>);
	    }
	}
	```

平时编写 React 时，如何设置我们的 key 呢？个人建议，如果后端给我们传输的数据中，有数据 id 的话，就直接使用数据 id，如果没有 id，就自己设置一个 id，当然 id 的值一定在这组数据中是唯一的，担心冲突的话，可以使用 Symbol，示例如下：

```
list: [
   {
        id:Symbol(),
        content: "列表项1"
   },{
        id:Symbol(),
        content: "列表项2"
   },{
        id:Symbol(),
        content: "列表项3"
   }
]

```

其实，在 React 中，每个 React 元素都有 key 属性，只是不同时生成一组元素时，没有设置 key，并不会对我们的程序，造成很大影响，所以 React 并不强制每个元素都设置 key 属性。

React 中的两个特殊属性 ref 和 key 我们就聊到这儿，下一遍我们会讲解一下 React 中 关于表单的操作。