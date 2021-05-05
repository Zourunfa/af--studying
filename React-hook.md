## react函数式组件

- 不能够用组件实例的三大属性 state props refs
- 函数式组件没有生命周期
- 函数式组件没有this

react-hooks就是让函数式组件可以实现这些东西







## useEffect和react生命周期的比较



**componentDidMount()组件第一次渲染完成，此时dom节点已经生成，可以在这里调用ajax请求，返回数据setState后组件会重新渲染**

**componentWillUnmount ()在此处完成组件的卸载和数据的销毁。**

- clear你在组建中所有的setTimeout,setInterval
- 移除所有组建中的监听 removeEventListener

**componentDidUpdate组件更新完毕后，react只会在第一次初始化成功会进入componentDidmount,之后每次重新渲染后都会进入这个生命周期，这里可以拿到prevProps和prevState，即更新前的props和state。**

```react
 const researchToSelect = React.createElement(() => {
      const { useEffect, useRef, useState } = window.React;
     const [count,setCount] = useState(0)

        useEffect(()=>{
            // 相当于 componentDidMount
            console.log(document.getElementById('btn'),1)

            return ()=>{
            	// 相当于 componentWillUnmount,在这个阶段，该dom节点将销毁，				下面的不会打印
                console.log(document.getElementById('btn'),2)
            }
			//不打印
            console.log(document.getElementById('btn'),3)
        },[])

        const btnClick = ()=>{
            console.log(count)
        }

        useEffect(()=>{
         	// 相当于 componentDidUpdate
            document.getElementById('num').innerHTML = count
        })
      

      return  <div className="content">
            <button id="btn" onClick={btnClick}>
            	点击
            	<span id="num">{count}</span>
            </button>
      </div>
    })

  ReactDOM.render(researchToSelect,document.getElementById("example"));

```



- **在使用useEffect的时候，有时候一步小心就会容易触发无限次的更新，有一种原因是因为没有使用useEffect的第二个参数的原因，监控值的变化，但是如果第二个参数数组里面传入的是函数的话，也会触发无限更新，因为函数是引用类型，所以第一次和第二次创建的引用地址完全不同，也会重复调用useEffect,这个时候解决的方法就是将这个在useEffect中要调用的函数用useCalback包裹，因为useCallBack会缓存旧的函数，**
- **而且如果要用async await语法的话是不能直接将async函数传入useEffect（）的也会有警告，报错的原因就是async函数会返回一个promise对象，但是，useEffect不应该返回任何内容**，这时需要在useEffect的回调函数中声明和使用这个async函数









## useRef

以前主要用于获取DOM元素

`useRef`这个hooks函数，除了传统的用法之外，**它还可以“跨渲染周期”保存数据**。



```react
import React, { useState, useEffect, useMemo, useRef } from 'react';

export default function App(props){
  const [count, setCount] = useState(0);

  const doubleCount = useMemo(() => {
    return 2 * count;
  }, [count]);

  const timerID = useRef();
  
  useEffect(() => {
    timerID.current = setInterval(()=>{
        setCount(count => count + 1);
    }, 1000); 
  }, []);
  
  useEffect(()=>{
      if(count > 10){
          clearInterval(timerID.current);
      }
  });
  
  return (
    <>
      <button ref={couterRef} onClick={() => {setCount(count + 1)}}>Count: {count}, double: {doubleCount}</button>
    </>
  );
}

```

在上面的例子中，我用`ref`对象的`current`属性来存储定时器的ID，这样便可以在多次渲染之后依旧保存定时器ID，从而能正常清除定时器。



## 自定义hook

**将组件逻辑提取到可重用的函数中**,其实和以前的高阶组件的目的一样，最根本的思想还是封装的思想

其实本质上是一个函数，只不过函数中可以添加react的逻辑，比如useEffect，等



vue3.0也开始拥有自定义hook

```tsx
import {useState,useEffect} from 'react'

// 自定义hook必须要use开头
// 在两个组件中使用相同的hook不会共享state
const useMousePosition = () =>{
    const [position,setPosition] = useState({x:0,y:0})
    useEffect(()=>{
        console.log("add Effect",position.x);
        
        const updateMouse = (e:MouseEvent)=>{
        
            setPosition({x:e.clientX,y:e.clientY})
        }
        document.addEventListener('mousemove', updateMouse)
        return ()=>(
            // 清楚前面的click事件的Effect
            // console.log(1)
            document.removeEventListener('click', updateMouse)
            
        )

    },[])
    return position
}

export default useMousePosition

```

```tsx
import useMousePosition from './Hook/useMousePosition'


....



  return (
   <div style={{margin:"auto 0"}}>
      <p>X:{position.x},y:{position.y}</p>
        <LikeBUtton></LikeBUtton>
        {/* <MouseTrack></MouseTrack> */}
     
    </div>
  );

```

