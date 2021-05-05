## redux的工作流程

![image-20210417160503084](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210417160503084.png)

> Redux：Redux就是应用的状态管理，即Redux用一个单独的常量状态树保持了一整个应用的状态，这个对象不能被直接改变，如果一些数据变化了，一个新的对象被创建

**设计和使用的三大原则**

- state以单一对象的存储在store对象中
- Reducer只能接收state，不能改变state
- reducer必须是纯函数,纯函数的概念就是，如果函数的调用参数相同，则永远返回相同的结果，它不依赖于程序执行期间函数外部任何状态或数据的改变，一句话说就是纯函数必须有参数决定返回结果



![image-20210418131759408](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210418131759408.png)



![image-20210418132358654](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210418132358654.png)

![image-20210418132411692](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210418132411692.png)





**网上把redux工作流程经常比作人在图书馆借书的过程，其中react的每一个组件(component)就像一个借书的人，借书的人想要借书，就必须先像图书管理员汇报，其中的action就相当于像图书管理员说我要借哪本书，store就相当于图书管理员，action一般需要在组件的某事件发生后要改变转态state时，此函数事件必须生成一个相应类型的action对象，再调用store的dispatch方法，这个对象必须有一个type属性。action转发后会到store中去找，最后通过reducer响应这个action,reducer传入旧状态的state,进行逻辑处理后返回新状态的state.它就相当于图书管理员的借书记录本，完成这一次借书状态的记录**





**Redux结合获取异步数据**

Redux不能直接在其中使用异步请求获取数据，应该在react的componentDidMount生命周期获取数据后，转发一个action,改变state的数据状态，后面有中间件一般放在中间件中









