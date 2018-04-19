# React 笔记

## 十二 - React 表单操作

在 React 表单的操作稍微的有些特殊，如果我们只是正常去写一些表单的元素和其他元素的操作并没有什么不同，但是，如果我们希望 表单中的数据，和我们 state 中的数据，保持一致时(比如给输入框设置默认value、给复选单选设置初始值)，就需要注意一些事情,如下例中的写法，React 就会报出警告，让我们给 input 添加 onChange 事件。从这个问题开始我们就需要说到两个知识点 受控组件 和 非受控组件。

```
	class Form extends React.Component {
    	constructor(arg){
            super(arg);
            this.state = {
                val: "请输入姓名",
                checked: true
            };
        }
        render(){
    		return (<form>
    			<input type="text" value={this.state.val} />
                <input type="checkbox" checked={this.state.checked} />
                <input type="submit" value="提交" />
    		</form>);
    	}
    }
``` 

### 受控组件

在 React 中我们经常需要，表单中的数据 和 state 进行绑定，表单的数据改变 state 需要同步，state 改变表单的显示也需要同步，这个时候我们就需要用到受控组件，怎么来设置受控组件我们来看看下面的[示例](demo_1.html)


```
class Form extends React.Component {
	constructor(arg){
        super(arg);
        this.state = {
            val: "请输入姓名",
            checked: true
        };
        this.changeValHandler = this.changeValHandler.bind(this);//注意 React 事件函数中 的this 是 undefined，所以要绑定 this
        this.changeCheckedHandler = this.changeCheckedHandler.bind(this);
    }
    changeValHandler(e){
        this.setState({
            val: e.target.value
        });
        // 修改了 表单中的数据，同步 state，state改变之后，又会重新 render 这个就是所谓的受控组件
    }
    changeCheckedHandler(e){
        this.setState({
            checked: e.target.checked
        });
    }
    render(){
		return (<form>
			<input 
                type="text" 
                value={this.state.val}
                onChange={this.changeValHandler} 
            />
            <input 
                type="checkbox" 
                checked={this.state.checked} 
                onChange={this.changeCheckedHandler} 
            />
            <input type="submit" value="提交" />
		</form>);
	}
}
```

### 非受控组件

如果我们只是希望给表单控件，设置一个初始值，并不需要表单和state进行绑定的话，我们就可以使用非受控组件。

非受控组件，只需用 defaultValue 和 defaultChecked 来设置表单控件的默认值

通过 defaultValue和defaultChecked来设置控件的默认值, 后期state改变了，不会同步 控件 中的数据。

非受控组件的使用方法，参考下方[示例](demo_2.html)

```
	class Form extends React.Component {
		constructor(arg){
	        super(arg);
	        this.state = {
	            val: "请输入姓名",
	            checked: true
	        };
	    }
	    componentDidMount(){
	        setTimeout(()=>{
	            this.setState({
	                val: "天气不错",
	                checked: false
	            });
	        },5000);
	    }
	    render(){
			return (<form>
				<input 
	                type="text" 
	                defaultValue={this.state.val} 
	            />
	            <input 
	                type="checkbox" 
	                defaultChecked={this.state.checked}  
	            />
	            <input type="submit" value="提交" />
			</form>);
		}
	}
``` 