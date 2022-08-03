# 1.React的数据

> React组件的数据分成2种，props和state。props和state的改变都有可能引发组件的重新渲染。
>
> - props
>
>   - props是React组件的对外接口。
>
>   - props接受任何入参。
>
>   - props是不能修改的，所有React组件都必须像纯函数一样保护props不被修改。
>
>   - 函数式组件使用props
>
>    ```{JSX}
>   //index.js文件内容
>   import React from 'react';
>   import ReactDOM from 'react-dom';
>   import Counter from './Counter';
>   
>   ReactDOM.render(
>     <React.StrictMode>
>       <Counter number = {0}/>
>       <Counter number = {10}/>
>       <Counter number = {20}/>
>     </React.StrictMode>
>   )
>    ```
>
>     ```{JSX}
>     //Counter.js文件内容
>     import React, { useState } from 'react';
>   
>     export default function Counter(props)
>     {
>       const [counter,setCounter] = useState(props.number); //这样Counter组件就可以接受一个number作为输入。
>       return (
>         <input type = "button" value = "-" onClick = {() => {setCounter((prev) => prev - 1)}}/>
>         <input type = "button" vlaue = "+" onClick = {() => setCounter((prev) => prev + 1)}/>
>         <span>{counter}</span>
>       )
>     }
>
>
> - state
>
>   - state是React组件的内部状态。
>   - state可以被修改。
>   - 父组件可以选择把自己的state作为props传入子组件。

# 2.React不能通过返回false阻止默认行为

> ```{html}
> <form onsubmit="console.log('You clicked submit.'); return false">
>   <button type = "submit">Submit</button>
> </form>
> ```
>
> ```{JSX}
> function Form(){
>   function handleSubmit(e){
>     e.preventDefault();
>     console.log('You clicked submit');
>   }
> }
> 
> return(
>   <form onsubmit={handleSubmit}>
>     <button type = "submit">Submit</button>
>   </form>
> )
> ```
>
> 

# 3.使用state需要注意的3点

> - 不要直接修改state
>
>   - React机制规定修改state必须使用setState()方法，不能直接对state进行修改，构造函数是唯一可以给this.state赋值的地方。（类组件）
>
>     ```{JSX}
>     this.setState({
>       comment: "hello",
>     })
>     ```
>
>     
>
> - state的更新可能是异步的
>
>   - 由于性能考虑，React会把多个setState()合并成一个调用，所以setState实际上是异步的。（异步的表现就是执行顺序和代码顺序不一致，异步是指不等一个比较耗时的任务执行完，就会执行之后的任务）。
>
> - 

